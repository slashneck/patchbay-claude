---
name: video
description: Make a store video for this project's app on Patchbay (patchbayapps.com), the app store for apps made with AI, plus its title and description. Use when the user says "make a store video", "make a video for my Patchbay page", "make a trailer / promo / explainer / short for my app", "make a TikTok / Reel / Short for my app". Asks a few questions first (kind of video, song, voice), records the real app with demo content, builds the video, shows it, and only puts it on the page after a yes.
---

# Store video for Patchbay

Patchbay address: `https://patchbayapps.com` (below: `$PB`). The page itself is made by the `page` skill; this skill makes the video for it, and the title and description to post it with.

Talk to the user in plain, friendly words. Many makers are not coders.

## 1. Ask first (one round of questions)

Use your question tool if you have one, otherwise ask in one short message. Offer sensible defaults so the user can just say "go".

1. **What kind of video?**
   - **Explainer** (default): 16:9, 40 to 90 seconds, a voice explains while the real app is shown, with captions. For the Patchbay page, YouTube and X.
   - **Short**: 9:16, 15 to 35 seconds, music only, cuts on the beat, big text. For TikTok, Reels and Shorts.
   - Both.
2. **Music.** Ask for a song file the user has the rights to use, or no music. If they want a "free" song, say plainly that many free libraries only allow personal content: a video that promotes an app usually counts as promotion (NoCopyrightSounds, for example, needs a licence for that). Safe choices: songs under Creative Commons BY (credit the artist), libraries whose licence allows commercial use, or music they made. Never make up a licence. Don't use AI-generated music unless the user asks for it.
3. **Voice** (explainer only):
   - Their own recording (they read the script you write).
   - A text-to-speech voice that runs on their computer, if they have one (Kokoro, Piper, the system voice…). Ask which voice and how fast. Don't send the script to an online voice service without asking.
   - No voice: big on-screen text carries the story instead.
4. **What to show.** Suggest 3 to 6 things the app does best (from the README and the code) and a one-line hook to open with. Let the user change the list and the order.
5. **Anything to keep out of view.** Their own files, accounts, names, keys, private content.

## 2. Write the script and wait for an OK

- Explainer: one short section per feature, 1 to 3 spoken sentences each, plain words, no hype. Open with the hook (the problem in one line, then "Meet <App>."). End with: "<App> is free / open source / for <platform>. Download it on GitHub, or on Patchbay." Only claim what the app really does.
- Short: 5 to 8 beats of 1 to 4 words each, the hook in the first second.
- Next to each line, note what is on screen.
- Show it to the user. Change it until they're happy. Don't record anything before the OK.

## 3. Record the real app, with demo content

- **Never the user's real data.** Run the app in a separate demo profile, folder or account with made-up or properly licensed content. Never show personal files, folders, accounts, names, emails, keys or tokens, and say so if something private could slip in.
- **Web apps:** a headless browser (Playwright or similar) at 1920x1080 and device scale 2, recording frames while a script clicks through the scenes. Serve the app with demo data if needed.
- **Desktop apps:** best is capturing the app's own window from inside the app (a small recorder in a scratch copy of the app that saves its window as frames), next best a capture of only the app window. Never capture the whole desktop.
- Log the cursor position, clicks and named moments ("added", "playing") with timestamps while recording. The video draws its own cursor from that log, which looks smooth at any frame rate and always lands on what was clicked.
- Record at a steady 30 fps. Timers can snap to the system clock and give fewer frames; check the real frame rate from the timestamps.
- Keep recordings, scripts and demo profiles **outside the repo** (a temp or scratch folder). Nothing of this is committed except the final video.
- Look at a few recorded frames before building the video: is the right thing on screen, is anything private visible?

## 4. Build the video

Any tool that renders video from code works. Remotion (React) is a good default: install it in a folder outside the repo (`npx create-video@latest`) and render with `npx remotion render`.

What makes these videos look good:

- **Never a blank frame.** Keep the app window up the whole time. Between sections, crossfade (about 6 to 14 frames). If the voice runs longer than the recording, hold the last frame instead of cutting to black.
- **Cursor:** drawn from the log, slightly smaller when pressed, always on the element that gets clicked.
- **Camera:** for wide apps, zoom in on what matters, smoothed (no jumps), never past the window's edge, and leave room for the captions. Small or tall apps (a phone app, a narrow window): no zoom. Put the app whole on one side and big text on the other.
- **Captions:** what the voice says, in lines of about 8 words, or the whole sentence in big type next to the app with each word lighting up as it's spoken.
- **Music:** quieter while the voice speaks, back up in the gaps. Put the song's drop or first big beat on the reveal ("Meet <App>."). Let it swell after the last spoken word, then fade out. Shorts: cut on the beat; analyse the tempo first.
- **End card:** the app's icon and name, 3 or 4 short tags in square-cornered boxes, then two boxes: GitHub (`github.com/OWNER/REPO`) and Patchbay (`patchbayapps.com`).
- Use the app's real icon, colors and font. Logos of other products only where the app really works with them.

## 5. Check, then show the user

- Render stills at every section and **look at them**: nothing overlapping, nothing cut off at the edges, no text over the thing being shown, nothing private.
- Check the audio: the voice clearly on top, the same loudness all the way through (measure it if you can't listen), no music under the last line.
- Send the user the video file and say what's in it. If they want changes, make them and render again.

## 6. Title and description

Write, show the user, and offer to save them (for example in the project's docs, or just in the chat):

- **Explainer:** a YouTube title (under 70 characters, the hook first, then what it is), two other title options, and a description: one short paragraph, 5 to 8 feature lines starting with an emoji, the platform and price, the download links (GitHub and `$PB/app/<slug>`), the music credit exactly as the song's licence asks, and a few hashtags.
- **Short:** a first line, a YouTube Shorts title (under 100 characters, ends with `#shorts`), a caption with the links "in bio", the music credit, and hashtags.

## 7. Put it on the Patchbay page (after a yes)

1. The page plays the video from the repo. Compress it to **under 20 MB** so it streams fast: `ffmpeg -i video.mp4 -c:v libx264 -preset slow -crf 26 -tune stillimage -pix_fmt yuv420p -c:a aac -b:a 128k -movflags +faststart .patchbay/video.mp4` (raise `-crf` a little if it's still too big; keep the full-quality file for YouTube).
2. Set `"video": "video.mp4"` in `.patchbay/page.json`. A **new file name** (`video-2.mp4`) when replacing an older video, so Patchbay sees the change, and delete the old file.
3. Then follow the `page` skill from its preview step: show what changes, ask before committing and pushing (only `.patchbay/`), push, and tell Patchbay to sync. If the repo has no Patchbay page yet, offer to make one with the `page` skill first.

## Rules

- Show the real app only. Never fake a feature, a number or a review.
- Demo content only, never the user's own data.
- Nothing is committed or pushed without a yes, and only the `.patchbay/` folder.
- Credit the music the way its licence asks, and be honest with the user about what a licence allows.
