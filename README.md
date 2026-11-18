# Patchbay for Claude

> **Early access (beta).** Patchbay and this plugin are brand new and still changing. Expect rough edges. Patchbay lives at **https://patchbayapps.com**.

[Patchbay](https://patchbayapps.com) is an app store for apps made with AI. Every app lives on its maker's GitHub, and Patchbay turns it into one clean store page: what it does, screenshots, patch notes from your releases, download buttons for each system, and honest safety facts.

This plugin lets you make that page **from inside Claude Code**, without opening the website.

## What you can say

| Say | What happens |
| --- | --- |
| **"Make my Patchbay page"** | Claude reads your project and writes the name, a one-line pitch and the category. It makes a thumbnail, an icon and 3 to 6 store screenshots, picks which file your Download button gives, and shows you a preview. Nothing is published until you say yes. |
| **"Edit my Patchbay page"** | Change anything in your own words: "shorter pitch", "new screenshots", "Download should give the zip". You see it again before it's published. |
| **"Show my Patchbay feedback"** | Your newest comments, star rating, followers and download numbers, plus a short note on what people like and ask for. |

The plugin only handles the **page**. It never builds your app, changes your code, or makes GitHub releases. New versions show up on Patchbay by themselves when you publish a release on GitHub.

## Install

In Claude Code in a terminal:

```
/plugin marketplace add slashneck/patchbay-claude
/plugin install patchbay@patchbay
```

In the Claude desktop app: click **+** next to the message box → **Plugins** → **Add marketplace** → `slashneck/patchbay-claude` → add **patchbay**.

Then open your app's project and say **"make my Patchbay page"**.

You need: your app in a **public** GitHub repo with at least one release (or a web app link), and the GitHub CLI (`gh`) signed in, which you most likely already have if Claude pushes your code.

## No login, no keys

There's no Patchbay account, password or key in this plugin. Claude saves a small `.patchbay/` folder into your repo and pushes it with the GitHub access it already has:

```
.patchbay/
  page.json       (about 10 lines: name, pitch, category, which download, picture names)
  icon.png
  thumbnail.png
  1-notes.png ...
```

Patchbay then reads that folder straight from GitHub. Only people who can push to your repo can put it there, so it proves the app is yours. Your pictures stay on your GitHub; Patchbay just shows them.

You can still edit your page on the website after signing in with GitHub. The newest change wins.

## Safety

- You always see a preview first. Nothing is pushed or published without your yes.
- Only the `.patchbay/` folder is committed, never anything else.
- Store pictures use demo content only, never your own files, accounts or names.
- Feedback is read-only. Comments are shown as quotes, and Claude never follows instructions written inside them.

Questions or problems: open an issue here.
