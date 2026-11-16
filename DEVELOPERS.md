# Localist for Developers

Localist gives every app on the Mac a local AI model behind one address. It speaks the OpenAI
Chat Completions format, so the OpenAI SDKs and most tools that let you set a custom server
address work with it as they are. There's no account, no API key and no cost per request. And
nothing your app sends leaves the Mac.

## The basics

- **Address:** `http://127.0.0.1:42042/v1`
- **API key:** none. SDKs that insist on one accept any text, such as `localist`.
- **Model:** `auto` picks the model the person chose in Localist. See "Choosing a model" below.
- **Name your app:** send `X-Localist-Client: <your bundle id>`. Localist shows it in the menu
  bar under "Using Localist now". Without it, Localist uses the first word of your User-Agent,
  which is just "OpenAI" for the OpenAI SDKs.

Localist answers only on this Mac. Other computers can't reach it, and requests from web pages
are refused. Building for the web? Call it from your server, not from browser code.

If port 42042 is taken by something else, Localist uses the first free port from 42043 to 42047.
`GET /hub/v1/hello` answers `{"product": "localist", "port": …}` on the right one.

## Try it

```bash
curl http://127.0.0.1:42042/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "X-Localist-Client: com.example.notes" \
  -d '{
    "model": "auto",
    "messages": [{"role": "user", "content": "Write a haiku about autumn."}]
  }'
```

## Python

With the official `openai` package, streaming the reply:

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://127.0.0.1:42042/v1",
    api_key="localist",  # Localist doesn't check it, but the SDK wants one
    default_headers={"X-Localist-Client": "com.example.notes"},
)

stream = client.chat.completions.create(
    model="auto",
    messages=[{"role": "user", "content": "Summarize why the sky is blue in two sentences."}],
    stream=True,
)
for chunk in stream:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
print()
```

## JavaScript

With the official `openai` package, asking for JSON that matches a schema:

```javascript
import OpenAI from "openai";

const client = new OpenAI({
  baseURL: "http://127.0.0.1:42042/v1",
  apiKey: "localist",
  defaultHeaders: { "X-Localist-Client": "com.example.recipes" },
});

const reply = await client.chat.completions.create({
  model: "auto",
  messages: [{ role: "user", content: "Give me a simple pancake recipe." }],
  response_format: {
    type: "json_schema",
    json_schema: {
      name: "recipe",
      schema: {
        type: "object",
        properties: {
          title: { type: "string" },
          ingredients: { type: "array", items: { type: "string" } },
          minutes: { type: "integer" },
        },
        required: ["title", "ingredients", "minutes"],
      },
    },
  },
});

console.log(JSON.parse(reply.choices[0].message.content));
```

The reply always matches the schema. Localist doesn't check it afterwards; it keeps the model
from writing anything else in the first place.

## Swift

No package needed. Streaming with `URLSession`:

```swift
import Foundation

struct Chunk: Decodable {
    struct Choice: Decodable {
        struct Delta: Decodable { var content: String? }
        var delta: Delta
    }
    var choices: [Choice]
}

var request = URLRequest(url: URL(string: "http://127.0.0.1:42042/v1/chat/completions")!)
request.httpMethod = "POST"
request.setValue("application/json", forHTTPHeaderField: "Content-Type")
request.setValue(Bundle.main.bundleIdentifier ?? "com.example.app", forHTTPHeaderField: "X-Localist-Client")
request.httpBody = try JSONSerialization.data(withJSONObject: [
    "model": "auto",
    "stream": true,
    "messages": [["role": "user", "content": "Suggest a name for a hiking app."]],
])

let (bytes, _) = try await URLSession.shared.bytes(for: request)
for try await line in bytes.lines where line.hasPrefix("data: ") {
    let payload = line.dropFirst(6)
    if payload == "[DONE]" { break }
    if let chunk = try? JSONDecoder().decode(Chunk.self, from: Data(payload.utf8)),
       let text = chunk.choices.first?.delta.content {
        print(text, terminator: "")
    }
}
print()
```

A sandboxed Mac app needs the Outgoing Connections (Client) capability to reach Localist.

## Tools

Describe functions the way you would for OpenAI. The model answers with a call for your app to
run:

```python
from openai import OpenAI

client = OpenAI(base_url="http://127.0.0.1:42042/v1", api_key="localist")

tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Current weather for a city",
        "parameters": {
            "type": "object",
            "properties": {"city": {"type": "string"}},
            "required": ["city"],
        },
    },
}]

reply = client.chat.completions.create(
    model="auto",
    messages=[{"role": "user", "content": "Do I need an umbrella in Lyon today?"}],
    tools=tools,
)
call = reply.choices[0].message.tool_calls[0]
print(call.function.name, call.function.arguments)
```

Send the result back as a `tool` message with the call's `tool_call_id`, as with OpenAI.
`tool_choice` accepts `auto`, `none`, `required` or a named function.

## Images

Send images as `data:` URLs in an `image_url` part, and use a model that can see them:

```python
import base64
from openai import OpenAI

client = OpenAI(base_url="http://127.0.0.1:42042/v1", api_key="localist")

with open("receipt.png", "rb") as f:
    image = base64.b64encode(f.read()).decode()

reply = client.chat.completions.create(
    model="vision",
    messages=[{
        "role": "user",
        "content": [
            {"type": "text", "text": "What's the total on this receipt?"},
            {"type": "image_url", "image_url": {"url": f"data:image/png;base64,{image}"}},
        ],
    }],
)
print(reply.choices[0].message.content)
```

Localist never fetches images from the internet, so web addresses in `image_url` are refused.
A request can be up to 64 MB.

## Choosing a model

Ask for what you need rather than for a particular model. Localist maps it to the best model
the person has installed (they choose which ones):

| Model name | Use it for |
|---|---|
| `auto` | Everyday chat, writing, tools and JSON. Start here. |
| `vision` | Anything with images. |
| `reasoning` | Harder questions where step-by-step thinking helps. |
| `fast` | Short, quick replies. |

`GET /v1/models` lists these names and the installed models, with what each can do
(`capabilities`), its `state` and whether it fits the Mac. You can also ask for an installed
model by its full name, such as `lmstudio-community/granite-4.2-3b-MLX-4bit`.

If nothing installed can do what you asked, you get `model_not_installed`. The person can then
pick a suitable model in Localist.

## Extra options

Localist reads an optional `x_hub` object in the request. OpenAI SDKs pass it with `extra_body`
in Python and as an extra field in JavaScript.

| Field | What it does |
|---|---|
| `priority` | `interactive` (the default), `normal`, or `background` for work that can wait. Background requests wait for everyone else and pause on battery or when the Mac is hot. |
| `session_id` | A name for an ongoing conversation. Localist keeps its context ready, so the next turn can start sooner. |
| `keep_warm_seconds` | Keeps the model awake this long after your request, instead of the usual five minutes. |
| `reasoning` | `"on"` lets a model that can think it through do so. Its thinking arrives as `reasoning_content`. |

Every reply also carries an `x_hub` object: the model that answered (`resolved_model`), the name
you asked for (`alias`), time to the first word (`ttft_ms`) and `tokens_per_second`.

While streaming, Localist sends progress before the text: chunks with an empty `delta` and an
`x_hub.status` of `queued`, `loading_model` or `generating`. The OpenAI SDKs pass over them, so
you can ignore them or use them to show "Waking up the model…".

## When Localist is busy

There's one model in memory and one answer at a time. So requests take turns:

- A request waits while another app's answer finishes. If too many are waiting, you get `busy`.
- A request for a different model waits for the current answer, then Localist switches models,
  which takes a few seconds.
- If the Mac runs short of memory, you get `out_of_memory`. Try again a little later.
- When Localist knows how long to wait, the error says so in `retry_after_ms` and a
  `Retry-After` header.
- Closing the connection cancels the request at once, wherever it is.

Errors have one shape, and `message` is written to be shown to a person as is:

```json
{"error": {"code": "model_not_installed", "message": "This model isn't downloaded yet.",
           "hint": "Open the hub to install it (about 6.0 GB).", "retry_after_ms": null}}
```

| Code | Status | Meaning |
|---|---|---|
| `invalid_request` | 400 | Something in the request is wrong; `message` says what. |
| `permission_required` | 403 | Not allowed, such as a request from a web page. |
| `model_not_installed` | 404 | No installed model can do this. |
| `model_too_large` | 422 | The model doesn't fit this Mac's memory. |
| `capability_unsupported` | 422 | Localist can't do this yet. |
| `busy` | 429 | Too many requests waiting. Retry after the delay. |
| `out_of_memory` | 503 | Not enough memory right now. Retry after the delay. |
| `hub_paused` | 503 | Background work is paused for battery or heat. |

## Checking on Localist

`GET /hub/v1/status` returns memory, the model in memory, heat, power and the queue, plus your
own app's recent activity. Add `?stream=true` for one update per second.

## Things to know

- Models run on the Mac's own chip, so speed depends on the Mac and the model. Waking a sleeping
  model takes a few seconds.
- Ask for `max_tokens` when you only need a short answer. It'll save time.
- `n` other than 1 isn't supported.
- Embeddings aren't available yet.
