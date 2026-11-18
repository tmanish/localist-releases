# Localist Privacy

Localist runs AI models on your Mac so that what you write stays with you. This page describes
exactly what it does with your information, and what it doesn't do.

## What stays on your Mac

Everything you type into Localist stays on your Mac. So does every answer, and anything other
apps send it, images included. None of it leaves.

There's no account, no analytics and no crash reporting. Nothing about you or how you use
Localist is collected.

## When Localist goes online

Only to download the models you choose. They come from Hugging Face, where their makers publish
them. The download asks for the model's files by name and identifies itself as Localist. As
with any download, Hugging Face can see your Mac's internet address.

And once a day, to see whether there's a new version of Localist. It reads a small list of
releases from GitHub, where Localist is published, and downloads a new version from there when
there is one. The check sends nothing about you or your Mac beyond what any download does:
GitHub sees your Mac's internet address and that it's Localist asking. Every update is checked
against the developer's signature before it's installed. You can turn off automatic installs in
Settings. New versions are also posted on the
[releases page](https://github.com/tmanish/localist-releases/releases).

## Who can use Localist

Only apps running on your Mac. Localist can't be reached from other computers on your network,
and the websites you visit can't use it either, not even ones open on this Mac.

For now, any app on your Mac can ask Localist questions without asking you first. Only
the signed Localist app can change settings or manage model downloads through the bundled
service. macOS checks the signatures of the app and service before they exchange a private
connection. Connected apps shows the names other apps supply with their requests. Approval
for individual apps is planned.

## What Localist keeps on your Mac

- **Your models**, in the Localist Models folder in your home folder.
- **Your choices**, such as Open at Login and the settings in Settings.
- **Your chats** from the Chat page, the last 30, so they're still there after a restart. They're
  in `~/Library/Application Support/Localist/Chats`, with your user account's file permissions. Turn off **Keep my chats** in
  Settings to close the current chat and delete saved transcripts and their attachments.
  Localist reports an error if it cannot remove the files.
- **A short log** of what Localist's service did: models woken and put to sleep, downloads,
  errors. It never contains what you or your apps wrote, or any answer.
- **A cache of other apps' recent conversations.** When a model that reads text only
  (Ornith or Granite) goes to sleep, what it had worked out from an app's conversation is saved,
  so the app's next message doesn't make it read everything again. Each file holds the words of
  that conversation in the model's own numbering. They're in Localist's Caches folder in your
  Library, accessible to processes with your user account's file permissions. The service
  reuses an entry for up to 24 hours and removes it when read. At startup and after writing a
  cache, it removes expired entries and trims the oldest files to a 2 GB total. An idle or
  stopped service can leave expired files on disk until the next cleanup. Localist's own menu bar and Chat
  page use memory for this cache.
- **Recent activity**: which apps asked, how quickly the model answered, and what Localist did
  on its own. It's kept in memory only, so it's gone when Localist's service restarts.

Questions you ask in the menu bar are kept until you choose **New question** or quit Localist,
and are never saved to disk. So that follow-ups start faster, the model may also hold a recent
conversation in memory for a while.

To remove everything, see "Removing Localist" in Help.

## Questions

If something here is unclear, [open an issue](https://github.com/tmanish/localist-releases/issues).
