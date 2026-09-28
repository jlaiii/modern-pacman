# Modern Pac-Man

A modern, mobile-first Pac-Man in a single HTML file. Six game modes, a discreet cheat panel, four classic ghost personalities, and no dependencies — open `index.html` and play, even offline from `file://`.

**Play:** https://jlaiii.github.io/modern-pacman/

## Modes

- **Classic** — arcade rules: three lives, power pellets, scatter/chase phases.
- **Speed** — ghosts 25% faster, Pac-Man 10% faster. Everything hurries.
- **Zen** — five lives and gentler ghosts; deaths never end the run. Practice.
- **Time Attack** — three minutes on the clock. Score as much as you can.
- **Ghost Rush** — no power pellets, all four ghosts hunt from the first frame.
- **One Life** — a single life at higher speed. No mistakes.

## Features

- **Authentic play** — 28×31 classic maze validated for connectivity and pellet reachability; 282 pellets; scatter/chase phase clock with the classic reversal; per-ghost targeting (Blinky chases, Pinky cuts four tiles ahead, Inky uses the Blinky vector, Clyde retreats when close); frightened mode with a doubling 200/400/800/1600 chain; ghost house with staggered release; eyes returning home; warp tunnel; extra life every 10,000 points.
- **Modes and progression** — six modes as above, two bonus fruits per level, per-mode best scores, top-10 high-score table that tags assisted runs.
- **Controls** — arrow keys/WASD, `P` pause, `R` restart, `M` mute. On touch: a 44px+ pad or swipe anywhere on the maze.
- **Polish** — animated Pac-Man mouth, ghost eyes that track their direction, floating score popups, level banners, haptic taps, generated sound effects, three themes (Neon, Retro, Daylight), respects `prefers-reduced-motion`.
- **Honest scoring** — any cheat marks the run *assisted* and the high-score row is tagged.

## The cheat panel

There is deliberately no cheat button anywhere in the UI. The panel opens on a **triple-tap of the version line** in the menu, or of the **score panel** during a run. It slides up as a bottom sheet and **does not pause the game** — the maze keeps running behind it.

18 cheats in three tabs:

- **Play** — Invincible, Auto-pilot, Pellet magnet, Speed boost, Slow motion, Infinite lives.
- **Ghosts** — Freeze ghosts, Slow ghosts, Ghost release, Ghost-free zone, Fearless ghosts, X-ray vision.
- **Maze & Score** — Score multiplier (1×/2×/5×/10×), Endless power, Stop the clock, Skip level, Sweep pellets, Drop fruit.

Auto-pilot is a real playing agent: it flees ghosts within seven tiles, hunts frightened ones, and otherwise routes to the nearest pellet through a breadth-first search of the maze.

## Debug API

For automated testing, `window.__pac` exposes:

```js
__pac.state()              // full run state: pac, ghosts, targets, cheats, clock
__pac.start('classic')     // start a named mode
__pac.freeze(true)         // stop the live rAF clock so advance() is the only time source
__pac.advance(5000)        // step the simulation deterministically
__pac.place(c, r, 'left')  // teleport Pac-Man in tests
__pac.setGhost(0, c, r, 'chase')
__pac.setPhase(1)          // force scatter/chase phase
__pac.scare(6000)          // trigger frightened mode
__pac.setCheat(id, val) / __pac.runCheatAction(id)
```

`/root/mp-qa/audit.py` drives all of this with Playwright: 191 checks covering boot, the maze data, gameplay, ghost AI, every mode, the hidden panel, all 18 cheats, settings persistence, scoring, and a responsive sweep from 320px to 1280px.

## Notes

- Single file, no build step, no network calls, no dependencies. Works from `file://`.
- Local storage holds settings, cheats, high scores and stats; nothing leaves the device.
- MIT licensed.
