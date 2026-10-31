# Magnet Panic

*Aimant panique · Pánico magnético*

Salvage station K-7 is packed with drifting rescue capsules, and you control the
only magnet that can bring them home. The catch: **debris is metal too**. Every
pull that rescues a capsule can sling a wreck into your hull.

## How to play

- **Move the magnet** with the mouse or your finger.
- **Click, tap the round button or press Space** to flip polarity:
  orange **N** attracts, blue **S** repels.
- The force **fades with distance and is capped** — close objects are
  steerable, far ones just drift.
- Capsules must enter the **blue salvage bay gently** (green mouth = safe
  speed). Too fast: they bounce. Way too fast: they are lost.
- Debris slamming the hull or the bay costs **1 of 3 hull points**. Debris
  ramming a capsule destroys it.
- **Save 10 capsules** before the timer runs out.

## Content

- A short interactive **tutorial** (capture, flip, dock).
- **Mission 1 — First Haul**: capsules and drifting debris from the top.
- **Mission 2 — Heavy Cargo**: heavy freight blocks, slow but brutal.
- **Mission 3 — All Vectors**: traffic from three sides.

Interface in **English / Français / Español** (English by default). Progression
and best scores are kept locally. Touch controls included; `P`/`Esc` pauses.

## Run it

Open `index.html` in any browser. No build, no network, no dependencies —
everything is inlined in one file.

## Development notes

- Pure static HTML/JS/CSS, Canvas 2D, synthesized WebAudio sound.
- **Fixed-step 60 Hz physics** with render interpolation: the simulation is
  independent of the display frame rate. Speeds are capped and bodies are
  positionally clamped, so nothing can tunnel through walls or blow up.
- Deterministic seeded RNG per run.
- `?debug=1` shows an FPS/object overlay. A read-only hook
  `window.__aimant` (start/step/pointer/invert/state/paint/screenshot) powers
  automated play sessions.

## Tests

`node --test games/aimant-panique-dev/aimant.test.mjs` (workshop copy) runs 19
tests against the shipped `index.html`: force cap and decay, polarity,
10-minute violent-simulation sanity (no NaN/tunneling/runaway), frame-rate
independence, bay docking thresholds, hull damage and grace period, capsule
kills, endings, capsule supply flux, tutorial progression, i18n parity,
seed determinism, and **winnability**: a simple steering bot clears all three
missions (8/8 seeds each) and still wins when a third of the capsules are
deliberately abandoned.

## Credits

- Concept and design: **Cédric Bernadac**
- Game, art and tests: **Z-Code** (AI agent)
- License: MIT — see `LICENSE`
- Cover generated with a small Pillow script (workshop copy
  `aimant-panique-dev/make_cover.py`).
