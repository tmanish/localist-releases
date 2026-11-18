# Localist Help

Localist runs AI models on your Mac. You can ask it questions from the menu bar, and the apps
you use can share the same model. Nothing you type leaves your Mac.

## Getting started

1. Drag Localist to the Applications folder and open it.
2. Pick your first model. Localist suggests one from each family that fits your Mac, and shows
   the download size and how much memory it needs (so there are no surprises).
3. Wait for the download. You can close the window; it keeps going, and the menu bar shows how
   far along it is.
4. Ask it something in the welcome window, then choose **Start using Localist**.

From then on, Localist lives in the menu bar. Click its icon to ask a quick question or see how
your Mac is doing. Open Localist from Applications (or choose **Open Localist** in its menu) for
the full window.

macOS tells you that Localist added a background item. That's expected. It's the part that
keeps your model ready for your apps, even when the menu is closed.

## Asking a question

Click the Localist icon in the menu bar, type a question, and press Return. The answer appears
as it's written.

- **Ask a follow-up** in the same field. Localist remembers the conversation until you choose
  **New question**.
- **Stop** an answer you don't need with the red button.
- If the model is asleep, the first answer takes a few seconds longer. It's waking up.

## Reading the menu bar

The icon changes with what Localist is doing:

- **Faded outline, empty chip:** asleep. No memory in use.
- **Filled chip:** a model is awake and ready.
- **Chip with pins, pulsing gently:** answering.
- **Small dot:** something needs you, such as allowing Localist to run in the background.

When you open the menu you see:

- **The memory gauge.** Dark olive is the model, light olive is your other apps, and gray is
  free. The number in the middle is how much memory is free.
- **A word for how roomy it is.** *Plenty of room* or *All yours* when at least 30% is free,
  *Getting tight* below that, and *Running low* under 15%.
- **Speed.** *Quick*, *Steady* or *Slow*, from the last answer.
- **Heat.** *Cool*, *Warm*, *Hot* or *Very hot*, as your Mac reports it.
- **Using Localist now.** Other apps that asked recently, and what they're doing.

## The Localist window

The window has five pages, listed on the left:

- **Cockpit.** The memory dial, speed and heat, the apps using Localist, the models on deck, and
  everything Localist did on its own. Open **Details** for a two-hour memory timeline and the
  technical numbers, and **Copy diagnostics** if you ever need to report a problem.
- **Models.** The models on your Mac and the ones you can download. Choose **Use this one** to
  make a model the one apps get, or **Put to sleep** to free its memory. To remove a model, click
  **…** on its card; it moves to the Trash, and emptying the Trash frees the space. Models under
  **Coming to Localist** haven't been tested yet and can't be downloaded.
- **Chat.** A longer conversation than the menu bar allows. Pick what you need done (everyday
  help, looking at images, thinking it through, quick replies) and Localist picks the model. Drop
  a document or an image onto the window, or click **+**, to ask about it: PDFs, Word and text
  files are read on your Mac. A long one isn't cut off: Localist reads the parts that fit each
  question, so you can ask about any chapter. With **Qwen3 Embedding** downloaded from the Models
  page, on a Mac with 16 GB or more, it finds those parts by what they mean, not only by the words
  they share with your question. The second menu picks a ready-made job, such as
  **Proofread**, **Summarise** or **Explain simply**, or your own standing instructions. With
  **Think it through**, the model's thinking is tucked under the answer; click it to read it.
  Each answer has **Copy** and **Try again**, and your last 30 chats are under the clock button.
- **Connected apps.** Which apps have used Localist, when, and how much.
- **Settings.** Light or dark, Open at Login, waking the model as you start to ask, whether chats
  are kept, updates, how much memory to leave for your other apps, how long an idle model stays
  awake, and how long replies may be.

Closing the window keeps Localist running in the menu bar.

## Updates

Localist checks for a new version once a day. When there is one, it downloads it in the
background and installs it the next time Localist restarts; it may also ask, quietly, whether to
restart now. Every update is checked against the developer's signature first. To check right
away, or to have Localist ask before installing, open **Settings** and look under **Updates**.

## How Localist looks after your memory

AI models need a lot of memory. Localist manages it for you, so you don't have to:

- **It wakes a model when something asks**, and puts it to sleep after five quiet minutes,
  giving the memory back.
- **Your apps come first.** If memory gets tight, the model steps aside. Requests wait and pick
  up again when there's room.
- **It leaves room to spare.** A model never takes more than 60% of your memory, or all but
  6 GB, whichever is smaller. That's a little under 10 GB on a 16 GB Mac.
- **One model at a time.** If an app asks for a different model, Localist finishes the current
  answer, then switches. Switching takes a few seconds.
- **On battery or when your Mac is hot**, work that can wait is paused.

When Localist does something on its own, the menu says why, for example "The model went to
sleep after 5 quiet minutes."

To get the memory back right away, open the menu and choose **Put to sleep**.

## Models

Your models are kept in the **Localist Models** folder in your home folder. To open it, click
the gear in the menu and choose **Show Models Folder**.

Each model is checked file by file after it downloads, so a damaged download is never used.

If a download stops, choose **Resume**. Localist keeps what already arrived and picks up from
there.

## The gear menu

- **Open Localist** opens the window.
- **Settings…** opens the Settings page.
- **Open at Login** starts the menu bar app when you log in. Your apps can still use Localist
  without it, because the background part runs on its own.
- **Restart Localist's Service** restarts the background part. A model in memory is put to
  sleep.
- **Show Models Folder** opens the folder in Finder.
- **Help**, **Privacy**, **Use with Coding Apps** and **For Developers** open these pages.
- **Quit Localist** closes the menu bar app. The background part keeps serving your apps.

## Troubleshooting

**"Allow Localist to run in the background."** Choose **Open Login Items** and turn on
Localist in System Settings ▸ General ▸ Login Items & Extensions.

**"Localist isn't running."** Choose **Try again**. If that doesn't help, choose **Restart
Localist's Service** in the gear menu.

**An app says the model isn't downloaded.** That app asked for something your model can't do,
such as reading images with a text-only model. Pick a model that can.

**Answers are slow.** Check the Heat tile. When your Mac is hot, it slows down to cool off.
Closing heavy apps (video editors, games, lots of browser tabs) also gives the model more room.

## Removing Localist

1. Turn off **Open at Login** in Localist's gear menu. Quit Localist, then turn off its
   background service in System Settings ▸ General ▸ Login Items & Extensions.
2. Drag Localist from Applications to the Trash.
3. To remove downloaded model weights and partial downloads, trash **Localist Models** in your home folder.
4. In Finder choose **Go ▸ Go to Folder**. Trash `~/Library/Application Support/Localist`
   to remove saved chats and their attachments, `~/Library/Caches/Localist` to remove
   conversation caches, and `~/Library/Logs/Localist` to remove the service log. If present,
   also trash `~/Library/Caches/app.localist.mac` and `~/Library/Caches/app.localist.mac.service`
   for app, updater and service request caches.
5. To clear saved preferences, run `defaults delete app.localist.mac` and
   `defaults delete app.localist.mac.service` in Terminal. An absent domain has nothing to remove.
   Empty the Trash when you're ready to permanently remove the files you moved there.
