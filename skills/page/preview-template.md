# Preview template

Copy everything inside the code block below into a new file named `preview.html` (outside the repo), then fill it in as SKILL.md says. It copies the layout of a real Patchbay app page: the header, the big video (or first screenshot) with the picture strip under it, the download box on the right, and the card shown in Browse.

````html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Patchbay page preview</title>
<style>
  :root { --bg: #0a0d0c; --panel: #111615; --line: #26302d; --text: #e8f1ee; --muted: #93a39e; --mint: #3cf2b5; --amber: #ffb547; }
  * { box-sizing: border-box; }
  html, body { margin: 0; background: var(--bg); color: var(--text); font: 15px/1.45 "Segoe UI", system-ui, sans-serif; }
  .mono { font-family: ui-monospace, Consolas, monospace; }
  nav { display: flex; align-items: center; gap: 28px; padding: 14px 28px; border-bottom: 1px solid var(--line); }
  nav .logo { font-weight: 800; font-size: 20px; } nav .logo i { color: var(--mint); font-style: normal; }
  nav span { color: var(--muted); font-size: 14px; }
  nav .note { margin-left: auto; color: var(--amber); font-size: 12px; }
  main { width: 1220px; margin: 0 auto; padding: 22px 0 26px; }
  .head { display: grid; grid-template-columns: 96px 1fr 200px; gap: 18px; align-items: start; }
  .icon { width: 96px; height: 96px; border-radius: 20px; object-fit: cover; background: var(--panel); }
  h1 { margin: 0; font: 800 38px/1.05 "Segoe UI", sans-serif; }
  .by { color: var(--muted); font-size: 13px; margin: 6px 0 4px; } .by b { color: var(--text); }
  .pitch { font-size: 16px; margin: 0 0 10px; }
  .chips { display: flex; flex-wrap: wrap; gap: 6px; }
  .chip { border: 1px solid var(--line); padding: 4px 9px; font-size: 11.5px; color: var(--muted); }
  .ghost { border: 1px solid var(--line); padding: 10px; text-align: center; font-size: 12px; font-weight: 700; margin-bottom: 8px; }
  .body { display: grid; grid-template-columns: 1fr 360px; gap: 26px; margin-top: 20px; }
  .media { position: relative; aspect-ratio: 16/9; background: #000; border: 1px solid var(--line); }
  .media img { width: 100%; height: 100%; object-fit: cover; display: block; }
  .play { position: absolute; left: 50%; top: 50%; width: 76px; height: 64px; transform: translate(-50%,-50%); border: 2px solid var(--mint); background: rgba(10,13,12,.55); }
  .play:after { content: ""; position: absolute; left: 29px; top: 16px; border-left: 24px solid var(--mint); border-top: 14px solid transparent; border-bottom: 14px solid transparent; }
  .vtag { position: absolute; left: 10px; bottom: 10px; background: rgba(0,0,0,.6); padding: 3px 8px; font-size: 11px; color: var(--muted); }
  .strip { display: flex; gap: 8px; margin-top: 8px; }
  .strip div { flex: 0 0 calc((100% - 32px) / 5); aspect-ratio: 16/9; border: 1px solid var(--line); background: var(--panel); overflow: hidden; }
  .strip div.cur { border-color: var(--mint); }
  .strip img { width: 100%; height: 100%; object-fit: cover; display: block; }
  .strip .more { display: grid; place-items: center; color: var(--muted); font-size: 13px; }
  .get { border: 1px solid var(--line); background: var(--panel); padding: 18px; }
  .get h2 { margin: 0 0 14px; font-size: 20px; }
  .tabs { display: grid; grid-template-columns: repeat(4, 1fr); border: 1px solid var(--line); font-size: 12px; text-align: center; color: var(--muted); }
  .tabs span { padding: 7px 0; } .tabs span:first-child { color: var(--text); border-bottom: 2px solid var(--mint); }
  .file { margin: 14px 0 4px; font-weight: 700; font-size: 13px; }
  .note2 { color: var(--muted); font-size: 12px; }
  .dl { margin: 14px 0 10px; background: var(--mint); color: #04130d; font-weight: 800; text-align: center; padding: 14px; font-size: 14px; }
  .card { margin-top: 14px; }
  .card .label { margin-bottom: 8px; }
  .card .c { border: 1px solid var(--line); background: var(--panel); }
  .card .c img.t { width: 100%; aspect-ratio: 16/9; object-fit: cover; display: block; background: #000; }
  .card .c .row { display: flex; gap: 10px; padding: 10px 12px; align-items: center; font-size: 12.5px; }
  .card .c .row img { width: 30px; height: 30px; border-radius: 7px; }
  .card .c span { color: var(--muted); display: block; }
  .label { font-size: 11px; letter-spacing: .12em; text-transform: uppercase; color: var(--muted); }
  .warn { color: var(--amber); font-size: 13px; margin: 0 0 10px; }
  .missing { outline: 3px dashed var(--amber); outline-offset: -3px; }
</style>
</head>
<body>
<nav><span class="logo">patch<i>bay</i></span><span>Discover</span><span>Browse</span><span>List your app</span><span class="note">PREVIEW · nothing is published yet</span></nav>
<main id="root"></main>
<script>
  const page = __PAGE_JSON__;
  const base = "__PICTURES__";
  const owner = "__OWNER__";
  const CATS = { productivity: "Productivity", creative: "Creative", audio: "Audio & music", video: "Video", security: "Security & privacy", ai: "AI tools", developer: "Developer tools", games: "Games", education: "Learning", utilities: "Utilities" };
  const DL = { auto: "The installer if there is one, otherwise the zip", installer: "The installer", portable: "The portable (zip) version" };
  const esc = (s) => String(s ?? "").replace(/[&<>"]/g, (c) => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;" })[c]);
  const pic = (f) => (f ? base + encodeURIComponent(f) : "");
  const shots = page.screenshots || [];
  const dl = String(page.download || "auto");
  const dlText = dl.startsWith("match:") ? "The file matching " + dl.slice(6) : DL[dl] || dl;
  const warn = [];
  if (!page.name) warn.push("No name.");
  if ((page.pitch || "").length > 160) warn.push("The pitch is longer than 160 characters.");
  if (!CATS[page.category]) warn.push("Unknown category: " + page.category);
  if (!shots.length) warn.push("No screenshots: Patchbay will use pictures from your README instead.");
  // The big player shows the video (its poster is the thumbnail), otherwise the first screenshot.
  const big = page.video ? (page.thumbnail || shots[0]) : shots[0];
  const items = (page.video ? [big] : []).concat(page.video ? shots : shots);
  const strip = items.slice(0, 5).map((s, i) => `<div class="${i === 0 ? "cur" : ""}"><img src="${pic(s)}" alt=""></div>`);
  if (items.length > 5) strip[4] = `<div class="more">+${items.length - 4} more</div>`;
  document.getElementById("root").innerHTML = `
    ${warn.length ? `<p class="warn">${warn.map(esc).join("<br>")}</p>` : ""}
    <div class="head">
      <img class="icon" src="${pic(page.icon)}" alt="">
      <div>
        <h1>${esc(page.name)}</h1>
        <div class="by">by <b>${esc(owner)}</b> · ${esc(CATS[page.category] || page.category)}</div>
        <p class="pitch">${esc(page.pitch)}</p>
        <div class="chips mono"><span class="chip">License: from GitHub</span><span class="chip">Signed or unsigned: checked by Patchbay</span><span class="chip">↓ downloads</span><span class="chip">No ratings yet</span><span class="chip">Windows</span></div>
      </div>
      <div><div class="ghost mono">VIEW ON GITHUB ↗</div><div class="ghost mono">FOLLOW</div></div>
    </div>
    <div class="body">
      <div>
        <div class="media">${big ? `<img src="${pic(big)}" alt="">` : ""}${page.video ? `<div class="play"></div><div class="vtag">Video: ${esc(page.video)}</div>` : ""}</div>
        <div class="strip">${strip.join("")}</div>
      </div>
      <div><div class="get">
        <h2>Get ${esc(page.name)}</h2>
        <div class="tabs"><span>Windows</span><span>Mac</span><span>Linux</span><span>Android</span></div>
        <div class="file mono">Newest GitHub release</div>
        <div class="note2">${esc(dlText)}</div>
        <div class="dl mono">DOWNLOAD FOR WINDOWS</div>
        ${page.web_app ? `<div class="note2">Also an Open button for ${esc(page.web_app)}</div>` : ""}
        <div class="note2" style="margin-top:10px">Below this: safety checks, your README, releases as patch notes, ratings and comments.</div>
      </div>
      <div class="card"><div class="label">Your card in Browse</div><div class="c"><img class="t" src="${pic(page.thumbnail || shots[0])}" alt=""><div class="row"><img src="${pic(page.icon)}" alt=""><p style="margin:0"><b>${esc(page.name)}</b><span>${esc(page.pitch)}</span></p></div></div></div></div>
    </div>
`;
  document.querySelectorAll("img").forEach((img) => img.addEventListener("error", () => img.classList.add("missing")));
</script>
</body>
</html>
````
