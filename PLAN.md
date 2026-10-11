# Newcastle Harbour Pilot — 3D Browser Game Implementation Plan

> **For the implementing agent:** This plan is self-contained. The user is non-technical — do all technical work for them, explain in plain English, and verify each milestone by actually opening the game in a browser before moving on. Do not hand them code snippets to run; deliver working files.

**Goal:** A fun, casual 3D browser game where the player steers a coal ship into Newcastle Harbour (NSW, Australia) past Nobbys Lighthouse without crashing — as a single self-contained HTML file the user can upload to any website.

**Architecture:** One `index.html` file containing all HTML, CSS, and JavaScript. Three.js loaded from CDN (with the exact version pinned). All 3D models built procedurally from Three.js primitives (boxes, cylinders, cones) — no external model files, no textures, no build step. Low-poly stylised look with flat colours.

**Tech Stack:** Three.js (r160+ via CDN import map), vanilla JavaScript, Web Audio API for simple sound effects (no audio files), localStorage for high score.

**Audience & tone:** Everyone / casual. Easy to pick up in 5 seconds, forgiving (bumping things slows you down and dents a "hull health" bar rather than instantly ending the game), charming and slightly cartoonish. Think "lighthouse keeper's postcard," not "simulator."

---

## Game Design Spec

### Setting (real Newcastle geography, stylised)
- Player approaches **Newcastle Harbour entrance from the ocean (east)**, heading roughly west.
- **Nobbys Head + Nobbys Lighthouse** on the southern side of the entrance: a small white lighthouse with a red roof on a grassy headland, connected to shore by the long breakwater.
- **Northern breakwater (Stockton side)** on the right: a long grey rock wall.
- The channel between them is the play corridor (~generous width for casual play).
- Inside the harbour: simple stylised city skyline on the left (south bank — a few mid-rise blocks, **Christ Church Cathedral** silhouette on the hill, **Queens Wharf tower**), cranes and coal loaders on the right (Kooragang/Carrington side), and the **destination berth** — a glowing green dock zone.
- Ocean: animated blue water plane (simple vertex-wave shader or sine-displaced geometry). Sky: gradient blue with a few white clouds (flat sprites or boxes). Warm afternoon light.
- Flourishes: seagulls (simple white V-shapes circling), a couple of moored yachts, a tiny tugboat that putters around, dolphins that occasionally arc out of the water near the entrance.

### Player ship
- Stylised coal ship: long dark-red/black hull, white bridge castle at the stern, a few hatch covers on deck. ~3 boxes + details. Leaves a white foam wake (simple particle trail or fading quads).

### Core loop (one run ≈ 60–90 seconds)
1. Title screen: game name ("Harbour Pilot — Newcastle"), "Tap / Press Space to start," best score shown.
2. Ship starts out at sea, moving forward automatically at a gentle base speed.
3. Player steers left/right and can throttle up/down slightly.
4. Obstacles in the channel: buoys to slalom between (bonus points for passing on the correct side — red/green channel markers), a slow ferry crossing the channel (the Stockton ferry), floating debris, one anchored yacht.
5. Collectables: gold stars or "cargo bonus" floats worth points.
6. Collisions: bump = screen shake, "clunk" sound, lose one of 3 hull points, brief invulnerability. Lose all 3 = "Run aground!" screen with score and instant retry.
7. Reach the green berth zone and slow down inside it = **docked!** Score = time bonus + buoy bonuses + collectables + remaining hull. Confetti/seagull celebration, score screen, "Play again."

### Controls
- **Desktop:** Arrow keys or A/D to steer, W/S (or Up/Down) for throttle. Space to start/restart.
- **Mobile:** Tilt is unreliable — use two large semi-transparent touch zones (left half steers left, right half steers right) OR left/right on-screen buttons; tap with two fingers (or a button) for throttle. Must be tested at mobile viewport size.
- Camera: smooth third-person chase cam behind and above the ship, slight lag for a nice feel.

### Feel checklist (what makes it "casual fun")
- Ship turns smoothly with momentum but never feels like a brick — tune turn rate up until it's fun, not realistic.
- Generous hitboxes in the player's favour (obstacle hitboxes ~80% of visual size).
- Constant gentle forward motion; the player never stalls or gets stuck.
- Score numbers pop up where points are earned (+100 floating text).
- Sound: low engine hum, "ding" for collectables, "clunk" for bumps, seagull cries occasionally, triumphant chime on docking. All generated with Web Audio oscillators/noise — no audio files. Mute button in corner.

---

## Tasks

### Task 1: Project scaffold + ocean + sky
**Files:** Create `~/newcastle-harbour-pilot/index.html`
- HTML skeleton, Three.js via CDN import map (pin exact version, e.g. three@0.160.0), full-window canvas, resize handler.
- Animated water plane, gradient sky, directional sunlight + ambient light, fog for depth.
- **Verify:** Open in browser — animated ocean under a blue sky at 60fps, no console errors.

### Task 2: Player ship + chase camera + controls
- Build the coal ship from primitives. Forward auto-movement, left/right steering with momentum, mild throttle control, gentle bobbing on the waves, banking into turns.
- Chase camera with smoothing.
- **Verify:** Steer around with arrow keys; movement feels smooth, camera follows nicely.

### Task 3: The harbour — Nobbys, breakwaters, skyline, berth
- Southern headland with white/red lighthouse, two breakwater walls forming the channel, city blocks + cathedral silhouette on the southern hill, cranes on the north side, green glowing berth zone at the end (~800–1200 world units from start).
- **Verify:** Drive the full course; geography reads as "entering a harbour past a lighthouse."

### Task 4: Obstacles, collectables, collisions
- Red/green channel marker buoys, crossing ferry, debris, anchored yacht; gold star collectables.
- Sphere/box distance collision checks (hitboxes 80% of visual size). Bump = shake + hull damage + brief invulnerability; 3 hull points as heart/ship icons in the UI.
- **Verify:** Collide on purpose — lose hull points, see shake; collect stars — score increases.

### Task 5: Game states + scoring + UI
- States: TITLE → PLAYING → DOCKED / WRECKED → (restart). HTML/CSS overlay UI: title screen, score + hull display, floating "+100" popups, end screens with score breakdown and best score (localStorage), restart via tap/Space.
- **Verify:** Full loop works — start, play, dock (win) and wreck (lose) both reachable, best score persists after page reload.

### Task 6: Sound (Web Audio, no files)
- Engine hum (filtered noise/oscillator tied to throttle), collect ding, collision clunk, docking chime, occasional seagull. Mute toggle button, state saved in localStorage. Audio context must start on first user interaction (browser requirement).
- **Verify:** Sounds play after first tap/keypress; mute works and persists.

### Task 7: Mobile support
- Touch steering zones or buttons, UI readable at 390×844, `viewport` meta tag, prevent page scroll/zoom on touch, test performance (cap pixel ratio at 2).
- **Verify:** In browser devtools mobile emulation — playable with touch only, no scrolling glitches.

### Task 8: Polish pass
- Seagulls, dolphins, tugboat, wake particles, clouds; tune difficulty so a first-timer docks within ~3 attempts; confetti/celebration on docking; favicon (inline data URI); page `<title>` "Harbour Pilot — Newcastle"; Open Graph meta tags so the link previews nicely when shared on X/Facebook.
- **Verify:** A fresh playtest of 3 full runs feels fun, nothing visually broken.

### Task 9: Delivery
- Final artifact: one `index.html` (should be well under 200 KB).
- Preview it for the user in the Hermes desktop preview pane for final sign-off.
- Give the user two plain-English publishing paths:
  1. **Their website:** hand them the file + one-paragraph instruction for their web person ("upload this file and link to it — it's self-contained").
  2. **Free hosting:** offer to set up Netlify Drop or GitHub Pages for them (agent does the steps; user just confirms the URL works).
- **Verify:** The final file opens and plays correctly when double-clicked from Finder (no server needed) — this proves it will work anywhere.

---

## Risks & notes
- **CDN dependency:** Three.js loads from a CDN, so the game needs internet — fine for a website game. (Optional later: inline Three.js for a fully offline file.)
- **Performance on old phones:** keep total object count modest, reuse geometries/materials, no shadows (or one cheap shadow), cap pixel ratio.
- **Don't over-scope:** no multiplayer, no leaderboard server, no real map data. Charm over realism. YAGNI.
- **Accuracy vs. fun:** Newcastle locals should recognise Nobbys, the breakwaters, the cathedral on the hill — but it's a caricature, not a map.

## Open questions (fine to decide during build)
- Ship name on the hull? (Could be fun — e.g. "MV Novocastrian" — or let the user pick.)
- Day scene only, or a sunset variant later?
- Add the user's website logo/link on the title screen? Ask them at Task 8.
