# Harbour Pilot — Newcastle

A free 3D browser game: steer a coal ship past Nobbys Lighthouse, through the breakwaters and into Newcastle Harbour, then ease into the coal berth.

## What's in this folder

| File | What it is |
|---|---|
| `index.html` | **The game.** This one file is the whole thing. |
| `PLAN.md` | The original build plan. |
| `screenshot-title.png`, `screenshot-gameplay.png` | Pictures for sharing or for your website. |
| `README.md` | This note. |

## Play it on your computer

Double-click `index.html`. It opens in your web browser. It needs an internet connection because the 3D engine (Three.js) loads from a free public server the first time.

## Put it on your website

**Option A: your existing website.**
Send `index.html` to whoever manages your site with this message:

> "Please upload this file to a new folder, for example `/harbour-pilot/`, so it's live at `yoursite.com/harbour-pilot/`. It's a single self-contained HTML file with no database, no plugins and no extra files."

On WordPress: upload it through your host's File Manager or FTP. The normal WordPress media uploader blocks HTML files. You can also embed it inside an existing page with:

```html
<iframe src="/harbour-pilot/index.html" style="width:100%;height:80vh;border:0;border-radius:12px" allow="autoplay; fullscreen"></iframe>
```

**Option B: free hosting in two minutes (Netlify Drop).**
1. Go to https://app.netlify.com/drop
2. Drag this whole folder onto the page.
3. You get a live link straight away. Make a free account to keep it permanently and rename it, e.g. `harbour-pilot-newcastle.netlify.app`.

## How the game works

- **Controls:** arrow keys or A/D to steer, ↑/↓ or W/S for speed. On phones, use the on-screen ◀ ▶ FAST SLOW buttons. Press M to mute.
- **Scoring:** stars +25, passing between a red and green marker +50, docking +500, plus a time bonus, 150 for each life ring you still have, and +100 for a gentle docking.
- **Forgiving:** a bump costs one of your 3 life rings, and the ship blinks and passes through boats for a moment so one mistake can't snowball. You only lose when all 3 are gone.
- **High score** is saved on each player's own device.
