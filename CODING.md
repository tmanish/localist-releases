# Localist with Coding Apps

Coding apps that can talk to an OpenAI-compatible server can use the model on your Mac through
Localist. Your code stays on your Mac, and the coding app shares the model with everything else
instead of loading its own.

## The three things every app asks for

- **Address (base URL):** `http://127.0.0.1:42042/v1`
- **API key:** anything, such as `localist`. Localist doesn't check it; the field just can't be
  left empty in most apps.
- **Model:** `auto`

That's all most apps need. If an app asks for a "provider", choose **OpenAI compatible**,
**Custom** or **Locally hosted**, whichever it offers.

If port 42042 is taken, Localist uses the next free one up to 42047. The address at the top of
this page is the one it is using right now.

## Xcode

1. Open **Xcode ▸ Settings ▸ Intelligence**.
2. Add a model provider and choose **Locally Hosted**.
3. Enter port `42042` and give it a name, such as Localist.
4. Pick `auto` from the model list in the coding assistant.

## Visual Studio Code

Most coding extensions have an "OpenAI compatible" option. Two common ones:

**Continue.** Open its config file and add a model:

```yaml
models:
  - name: Localist
    provider: openai
    model: auto
    apiBase: http://127.0.0.1:42042/v1
    apiKey: localist
```

**Cline.** In its settings choose **OpenAI Compatible** as the API provider, then enter the
address, `localist` as the key and `auto` as the model ID.

## Zed

Add Localist under `language_models` in Zed's settings:

```json
{
  "language_models": {
    "openai_compatible": {
      "Localist": {
        "api_url": "http://127.0.0.1:42042/v1",
        "available_models": [
          { "name": "auto", "display_name": "Localist", "max_tokens": 32768 }
        ]
      }
    }
  }
}
```

When Zed asks for an API key, enter `localist`.

## Anything else

Look for a setting called base URL, API base, endpoint or custom server, and use the three
values above. The menus in these apps move from version to version; the values stay the same.

## Getting good results

- **Use `auto`.** Localist picks the installed model that suits the job. To pin one, use its
  name from the Models page.
- **Give it a moment the first time.** A sleeping model takes a few seconds to wake. After
  that, each turn of the same conversation starts faster, because Localist keeps the
  conversation ready.
- **One thing at a time.** One model answers at a time, so a coding app and a chat take turns.
  The coding app waits its turn; it doesn't fail.
- **Keep the context modest.** Models get 32,000 tokens of context. If the app lets you set a
  context length, use 32768 or less. Very large files are better attached a part at a time.
- **Agents that edit files** send long instructions and many tools with every request. They
  work, but a small model follows them less reliably than a large hosted one. For those, use
  the largest model that fits your Mac.

## If it doesn't connect

- Check that Localist's menu bar icon is there. If not, open Localist.
- Make sure the address ends in `/v1` and starts with `http://`, not `https://`.
- Use `127.0.0.1` exactly. Localist doesn't answer on your network address.
- Completion as you type uses `/v1/completions`, which Localist answers. It works best with a
  small, quick model, or at temperature 0 with Ornith 1.5 or Qwen 3.5 9B, which then run about
  1.6 times as fast. If suggestions lag, pick a smaller model for completion in the coding app, or
  turn completion off there and keep chat and edits.

Your coding app then shows up under **Connected apps** in Localist, with how much it has asked.
