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

Localist doesn't check for updates on its own yet. New versions are posted on the
[releases page](https://github.com/tmanish/localist-releases/releases).

## Who can use Localist

Only apps running on your Mac. Localist can't be reached from other computers on your network,
and the websites you visit can't use it either, not even ones open on this Mac.

For now, any app on your Mac can use Localist without asking first. A way to approve each app
the first time it asks is planned. Until then, the menu bar shows which apps have used
Localist recently, and how.

## What Localist keeps on your Mac

- **Your models**, in the Localist Models folder in your home folder.
- **Your choices**, such as Open at Login.
- **Recent activity**: which apps asked, how quickly the model answered, and what Localist did
  on its own. It's kept in memory only, so it's gone when Localist's service restarts.

Questions you ask in the menu bar are kept until you choose **New question** or quit Localist.
So that follow-ups start faster, the model may also hold a recent conversation in memory for a
while. That's it. None of it is saved to disk.

To remove everything, see "Removing Localist" in Help.

## Questions

If something here is unclear, [open an issue](https://github.com/tmanish/localist-releases/issues).
