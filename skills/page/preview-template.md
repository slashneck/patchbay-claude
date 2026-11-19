# Preview template

Copy everything inside the code block below into a new file named `preview.html` (outside the repo), then fill it in as SKILL.md says.

````html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Patchbay page preview</title>
<style>
  :root { --bg: #0b0f0e; --panel: #121816; --line: #24302c; --text: #e8f1ee; --muted: #93a39e; --mint: #3cf2b5; }
  * { box-sizing: border-box; }
  body { margin: 0; background: var(--bg); color: var(--text); font: 15px/1.5 system-ui, -apple-system, "Segoe UI", sans-serif; }
  .bar { padding: 12px 24px; border-bottom: 1px solid var(--line); color: var(--muted); font-size: 13px; }
  .bar b { color: var(--mint); }
  main { max-width: 1080px; margin: 0 auto; padding: 32px 24px 64px; display: grid; gap: 40px; }
  .label { font: 600 11px/1 ui-monospace, monospace; letter-spacing: .12em; text-transform: uppercase; color: var(--muted); }
  .head { display: flex; gap: 20px; align-items: center; }
  .icon { width: 88px; height: 88px; border-radius: 18px; background: var(--panel); object-fit: cover; border: 1px solid var(--line); }
  h1 { margin: 0; font-size: 34px; }
  .pitch { color: var(--muted); font-size: 18px; margin: 6px 0 0; }
  .tags { display: flex; gap: 8px; flex-wrap: wrap; margin-top: 12px; }
  .tag { border: 1px solid var(--line); padding: 4px 10px; font-size: 12px; color: var(--muted); }
  .tag.on { border-color: var(--mint); color: var(--mint); }
  .dl { display: inline-block; margin-top: 16px; background: var(--mint); color: #04130d; font-weight: 700; padding: 12px 22px; }
  .dlnote { color: var(--muted); font-size: 13px; margin-top: 8px; }
  .shots { display: grid; grid-auto-flow: column; grid-auto-columns: minmax(320px, 46%); gap: 14px; overflow-x: auto; padding-bottom: 8px; }
  .shots img { width: 100%; aspect-ratio: 16/9; object-fit: cover; border: 1px solid var(--line); background: var(--panel); }
  .card { width: 340px; background: var(--panel); border: 1px solid var(--line); }
  .card img.thumb { width: 100%; aspect-ratio: 16/9; object-fit: cover; display: block; background: #000; }
  .card .body { padding: 12px 14px; display: flex; gap: 10px; align-items: center; }
  .card .body img { width: 36px; height: 36px; border-radius: 8px; }
  .card b { display: block; }
  .card span { color: var(--muted); font-size: 13px; }
  .missing { outline: 2px dashed #ffb547; }
  .warn { color: #ffb547; font-size: 13px; }
</style>
</head>
<body>
<div class="bar"><b>Preview</b> · roughly how your page will look on Patchbay. Nothing is published yet.</div>
<main id="root"></main>
<script>
  const page = __PAGE_JSON__;
  const base = "__PICTURES__";
  const CATS = { productivity: "Productivity", creative: "Creative", audio: "Audio & music", video: "Video", security: "Security & privacy", ai: "AI tools", developer: "Developer tools", games: "Games", education: "Learning", utilities: "Utilities" };
  const DL = { auto: "the installer if there is one, otherwise the zip", installer: "the installer", portable: "the portable (zip) version" };
  const esc = (s) => String(s ?? "").replace(/[&<>"]/g, (c) => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;" })[c]);
  const pic = (f) => (f ? base + encodeURIComponent(f) : "");
  const dl = String(page.download || "auto");
  const dlText = dl.startsWith("match:") ? "the file matching " + dl.slice(6) : DL[dl] || dl;
  const warn = [];
  if (!page.name) warn.push("No name.");
  if ((page.pitch || "").length > 160) warn.push("The pitch is longer than 160 characters.");
  if (!CATS[page.category]) warn.push("Unknown category: " + page.category);
  document.getElementById("root").innerHTML = `
    ${warn.length ? `<p class="warn">${warn.map(esc).join("<br>")}</p>` : ""}
    <section>
      <div class="label">Card in Browse and on the front page</div>
      <div class="card" style="margin-top:12px">
        <img class="thumb" src="${pic(page.thumbnail || (page.screenshots || [])[0])}" alt="">
        <div class="body"><img src="${pic(page.icon)}" alt=""><div><b>${esc(page.name)}</b><span>${esc(page.pitch)}</span></div></div>
      </div>
    </section>
    <section>
      <div class="label">App page</div>
      <div class="head" style="margin-top:14px">
        <img class="icon" src="${pic(page.icon)}" alt="">
        <div>
          <h1>${esc(page.name)}</h1>
          <p class="pitch">${esc(page.pitch)}</p>
          <div class="tags"><span class="tag on">${esc(CATS[page.category] || page.category)}</span><span class="tag">Signed / unsigned: checked by Patchbay</span><span class="tag">Open / closed source: from GitHub</span></div>
        </div>
      </div>
      <a class="dl">Download</a>
      <div class="dlnote">Gives ${esc(dlText)} from your newest GitHub release.${page.web_app ? " Also an Open button for " + esc(page.web_app) : ""}</div>
    </section>
    <section>
      <div class="label">Screenshots</div>
      <div class="shots" style="margin-top:12px">${(page.screenshots || []).map((s) => `<img src="${pic(s)}" alt="">`).join("") || "<p class='warn'>No screenshots: Patchbay will use pictures from your README instead.</p>"}</div>
    </section>
    <p class="dlnote">Below this, Patchbay shows your README, your releases as patch notes, ratings and comments, all mirrored from GitHub.</p>`;
  document.querySelectorAll("img").forEach((img) => img.addEventListener("error", () => img.classList.add("missing")));
</script>
</body>
</html>
````
