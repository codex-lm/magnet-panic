# Delivery note — Magnet Panic 1.0.0

One-off mission for the Z Code workshop (2026-10-31, evening). Concept and
specification by Cédric Bernadac; implementation, art, tests and verification
by Z Code.

## What is in the box

- `index.html` — the whole game, self-contained (no network, no dependencies).
- `cover.png` — cover art (Pillow script, workshop copy `aimant-panique-dev/make_cover.py`).
- `README.md`, `LICENSE`, `LOB7-GAME.json`, `LOB7-ATTRIBUTION.json`, this note.

## Interpretation choices

- **"Enter gently"**: a capsule touching the bay at ≤130 px/s is docked
  (saved); faster it bounces off with a "too fast" cue; above 240 px/s it is
  destroyed. The bay mouth glows green/orange/red with the speed of the
  closest incoming object, and the tutorial shows a per-capsule speed ring.
- **Danger thresholds** were calibrated to the actual dynamics of the field:
  the capped, distance-falloff magnet can give a free-floating debris about
  175 px/s (112 for heavy blocks) over its full reach, so damage/kill
  thresholds sit at 200/120 (hull) and 170/110 relative (capsule kills).
  Below that range hazards never happen; much above they are unreachable.
- **Supply**: mission timers offer capsules every 6–7 s (per-mission caps on
  simultaneous bodies). A perfect docking run wins mission 1 at ~65% of the
  clock; a simple bot wins 8/8 seeds on every mission (87–103 s), and still
  wins when a third of the capsules are abandoned.
- Station damage has a 1 s grace window; one careless slingshot costs at most
  one hull point.

## Tests (all against the shipped index.html)

19/19 `node --test`: force cap + monotone decay, polarity, 10-minute violent
simulation (no NaN, nothing outside the arena, speeds capped), frame-rate
independence (irregular step batching == regular), bay docking tiers, hull
damage + grace + heavy threshold, capsule kills, wall loss, the three
endings, per-mission capsule flux, idle-arena non-autoplay, tutorial
progression, EN/FR/ES key parity, DOM i18n coverage, seed determinism,
winnability (bot, 3 missions × 8 seeds), sabotage run.

## Real browser play (local, in-app browser panel)

The panel throttles rAF when occluded, so the game was driven through its own
input handlers (real `PointerEvent`s on the canvas, real DOM button clicks)
with the documented `window.__aimant` QA hook advancing fixed steps when the
render loop was frozen. Verified live: home screen with locked mission cards,
tutorial all three steps (capture → real tap flipping polarity → gentle
docking), a complete mission 1 win (10/10 capsules, hull 3/3, score 1500,
~84 s), defeat by timeout, a real hull damage from a slamspeed debris
(299 px/s → integrity 2, on-screen warning), pause, help, and FR/ES texts.
Five screenshots in the workshop's `evidence/aimant-panique/` were checked
with a vision model; two defects found that way (UI overlays leaking through
`hidden`, immortal particles under hook-driven play) were fixed before
delivery.

## Known limits

- Not tested on a physical phone; touch handling uses standard PointerEvents
  and a large dedicated polarity button.
- Audio is synthesized WebAudio, unlocked on first gesture; no per-ear mixing
  pass was done.
- Balance was validated by bot runs, not by a large sample of human players.
- The QA hook `window.__aimant` is intentionally left enabled; it cannot
  modify game rules, only drive inputs and read state.
