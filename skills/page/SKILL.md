---
name: page
description: Make or edit this project's page on Patchbay (patchbayapps.com), the app store for apps made with AI. Use when the user says "make my Patchbay page", "put this on Patchbay", "list/upload this to Patchbay", "edit/update my Patchbay page", "change my pitch/screenshots/category on Patchbay", or "make store pictures for Patchbay". Writes .patchbay/page.json and the store pictures into the repo, shows a preview, pushes after a yes, then tells Patchbay to update the page. It never builds the app, changes app code, or makes GitHub releases.
---

# Patchbay page

Patchbay is an app store for apps made with AI. Every app lives on its maker's GitHub; Patchbay shows it as a clean store page with downloads from the repo's GitHub Releases.

Your job here is only the **page**: its words, its pictures, and which file the Download button gives. You do NOT build the app, bump versions, change app code, or create GitHub releases. If the user wants that, it's a separate task. A store video is made by the `video` skill; when the user asks for one, use that.

**How it works (no login):** you write a small `.patchbay/` folder into the repo (`page.json` + pictures), push it, then ask Patchbay to read it. Patchbay fetches the file from GitHub itself. Only people who can push to the repo can change it, so that is the proof of ownership. Never ask the user for a Patchbay login, password or key: there is none.

Patchbay address: `https://patchbayapps.com` (below: `$PB`). Patchbay is in early access (beta). If it stops answering, tell the user to update the plugin (`/plugin marketplace update patchbay`).

Talk to the user in plain, friendly words. Many makers are not coders.

## 1. Check the repo

Run `gh repo view --json nameWithOwner,isPrivate,isFork,defaultBranchRef,url` in the project.

- Not a git repo / no GitHub repo / `gh` not signed in: explain that the app has to be on GitHub first, and stop.
- `isPrivate: true`: Patchbay only lists public repos. Say so and stop (don't change visibility yourself).
- `isFork: true`: forks can't be listed. Stop.
- Check for a release with app files: `gh release list --limit 5` and `gh release view --json tagName,assets` (newest non-prerelease). If there is no release with an installer/zip/app file and the app isn't a web app with a link, explain: "Patchbay's Download button gives files from your GitHub Releases, and this repo has none yet. Publish a release with your built app first, then come back." Stop.

If `.patchbay/page.json` already exists, this is an **edit**: read it, change only what the user asked for, and skip to step 4 (make new pictures only if asked).

## 2. Write the page details

Read the README, the package/manifest files and the code enough to understand what the app really does. Then write `.patchbay/page.json`:

```json
{
  "name": "My App",
  "pitch": "Turns voice memos into tidy notes.",
  "category": "productivity",
  "download": "installer",
  "web_app": "",
  "signing": "",
  "icon": "icon.png",
  "thumbnail": "thumbnail.png",
  "screenshots": ["1-notes.png", "2-export.png", "3-settings.png"],
  "video": "video.mp4"
}
```

Rules (Patchbay refuses the page otherwise):
- `name`: 1 to 60 characters.
- `pitch`: one sentence, at most 160 characters. Say what it does for the user, no hype, no claims that aren't true.
- `category`: exactly one of `productivity`, `creative`, `audio`, `video`, `security`, `ai`, `developer`, `games`, `education`, `utilities`.
- `download`: what the Download button gives, from the newest release's files:
  - `auto`: the installer if there is one, otherwise the zip (fine for most apps)
  - `installer`: setup files first (.exe, .msi, .dmg, .pkg, .apk)
  - `portable`: the zip / unpack-and-run version first
  - `match:<pattern>`: one specific file, with `*` for the parts that change per version, e.g. `match:MyApp-Setup-*.exe`. Use this when the release has several files and the right one isn't obvious.
  Look at the actual release file names and pick the one a normal person should get. If unsure, ask the user.
- `web_app`: only if the app also runs in the browser; must start with `https://`. Otherwise leave empty.
- `signing`: only if the user tells you how the app is code-signed (shown as "Maker says"). Never guess. Usually empty.
- `icon`, `thumbnail`, `screenshots`: plain file names inside `.patchbay/` (letters, numbers, `.`, `-`, `_`), `.png`, `.jpg` or `.webp`. Up to 8 screenshots, best first. No links to other websites.
- `video`: optional, one per app. Either a YouTube link, or the file name of an `.mp4` or `.webm` inside `.patchbay/` (at most 50 MB; Patchbay plays it from GitHub with its own player). Compress a bigger video first, for example: `ffmpeg -i in.mp4 -vf scale=-2:1080 -c:v libx264 -crf 26 -preset slow -c:a aac -b:a 128k -movflags +faststart .patchbay/video.mp4`. Leave the key out to keep the page's current video; set it to `""` to remove it.

## 3. Make the pictures

Save them in `.patchbay/`. Keep each file under about 1.5 MB (PNG for sharp UI, or WebP/JPG for photos).

- **Screenshots (3 to 6)**, 1920x1080, all in the same style:
  - Dark background with a soft glow in the app's main color.
  - Left side: the app icon and name, a big two-line headline (second line in the app's color), a short two-line description, then three short feature tags. Tags are outlined boxes with **square corners**, never rounded pill or capsule shapes.
  - Right side: a real capture of the app window with realistic demo content (not an empty screen), slightly rounded corners and a soft shadow.
  - Each one shows one thing the app does. Name them `1-<what>.png`, `2-<what>.png`, …
- **Thumbnail**: 1280x720. Dark background, the icon on the left, the app name large on the right and one short line underneath. Keep everything inside the middle 80%. No screenshots, no tiny text, no pill-shaped tags.
- **Icon**: 512x512, the app's icon filling most of the square. Use the app's real icon if it has one.

How you capture the app is up to you (run it and screenshot it, or render its UI in a headless browser). Then compose the store images (for example an HTML layout rendered to PNG with a headless browser).

**Privacy, always:** demo or sample content only. Never show the user's own files, folders, accounts, names, emails, keys or anything private. Show the real app; never fake features it doesn't have.

## 4. Show a preview and wait for a yes

1. Make the preview outside the repo: copy the HTML from `preview-template.md` in this skill's folder into `preview.html` in a new temp folder, and replace `__PAGE_JSON__` with the contents of page.json and `__PICTURES__` with the full `file:///` path of the repo's `.patchbay/` folder (forward slashes, ending in `/`).
2. Turn it into a picture, because many app windows and preview panes can't open local files. Take a **full-page** screenshot of `preview.html` into `preview.png` in the same temp folder:
   - with a browser tool you have that can take full-page screenshots, or
   - with headless Chrome or Edge: `<chrome or msedge> --headless=new --disable-gpu --hide-scrollbars --virtual-time-budget=8000 --window-size=1280,1400 --screenshot="<temp>/preview.png" "file:///<temp>/preview.html"` (on Windows Edge is usually `C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe`). If the bottom is cut off, take it again taller.
3. **Look at the picture yourself** first: every picture loads, no broken images, no cut-off text. Fix and retake if needed. Then show it to the user (send or attach the image file, or give its full path). If no screenshot works, describe the page instead.
4. Tell the user, briefly: name, pitch, category, which file the Download button will give (the actual file name from the newest release), and list the pictures.
5. Ask: "Shall I put this on Patchbay? This commits the .patchbay folder to your repo and pushes it." **Do nothing more until they say yes.** If they want changes, make them and preview again.

## 5. Push and publish

1. The page is read from the repo's **default branch**. If the current branch isn't the default branch (`defaultBranchRef.name`), tell the user and ask how they want to get it there (switching branches can disturb their work, so don't do it on your own).
2. Commit **only** the `.patchbay/` folder: `git add .patchbay` then `git commit -m "Patchbay page"` (or "Update Patchbay page"), then `git push`. Don't stage anything else, and never commit secrets.
3. Tell Patchbay:
   ```
   curl -s -X POST "$PB/api/sync" -H "Content-Type: application/json" -d '{"repo":"OWNER/NAME"}'
   ```
   The answer is JSON:
   - `{"ok":true,"result":"created"|"updated"|"unchanged","url":"..."}`: done. Give the user the `url`.
   - `{"ok":false,"error":"..."}`: explain the error in plain words and fix it if you can (for example a field that's too long), then push and try again. "Just synced" means wait 15 seconds. GitHub can take a minute to show a fresh push; if it says no page.json right after pushing, wait a moment and retry once.
4. Patchbay only re-reads the page when `page.json` itself changed, and picture links point at the exact commit. So when you replace a picture, **give the new file a new name** (e.g. `1-notes-v2.png`), update `page.json`, and delete the old file. Otherwise Patchbay answers "unchanged" and keeps the old picture.

## Good to know (tell the user when it's relevant)

- The page goes live right away; there's no approval step. The community can report pages, and pages reported by several people get hidden.
- Editing: say "edit my Patchbay page" anytime. They can also edit on the Patchbay website after signing in with GitHub. The newest change wins: the file only overrides the website when the file itself changes.
- Removing the `.patchbay` folder doesn't delete the page. To remove it, sign in on the website and use "Unlist" on Your apps.
- New versions of the app need nothing here: Patchbay picks up new GitHub Releases on its own.
