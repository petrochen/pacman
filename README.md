# PAC-MAN

Browser recreation of the 1980 Namco arcade classic. Single HTML file, no dependencies, no build step.

**Play:** https://pacman.petrochenko.info

## Features

Faithful to the original arcade:

- **Original 28x31 maze** with 240 dots, 4 power pellets, side tunnels
- **4 unique ghost AIs** -- Blinky (chase), Pinky (ambush), Inky (unpredictable), Clyde (shy)
- **Scatter/Chase mode cycling** with arcade-accurate timers per level
- **Elroy mode** -- Blinky speeds up when few dots remain
- **Frightened mode** with per-level durations, flash warning, ghost chain scoring (200/400/800/1600)
- **Level 19+** disables frightened mode (power pellets award points only)
- **Fruit bonuses** -- cherry through key, spawn at 70/170 dots
- **Ghost house** with dot-counter release, 3-phase vertical exit, backup timer
- **Speed scaling** -- Pac-Man, ghosts, tunnel, frightened speeds all vary by level
- **Extra life** at 10,000 points
- **Eating slowdown** -- 1 frame for dots, 3 frames for pellets
- **Intermission cutscenes** after levels 2, 5, and 9
- **Level 256 kill screen** -- right half corrupts (just like the original)

## Controls

| Input | Desktop                     | Mobile          |
| ----- | --------------------------- | --------------- |
| Move  | Arrow keys / WASD           | D-pad buttons   |
| Move  | --                          | Swipe on maze   |
| Start | Click INSERT COIN / any key | Tap INSERT COIN |

## Screenshots

```
 +--------------------------+
 | SCORE          HIGH SCORE |
 | 2840      PAC-MAN   5120 |
 |                          |
 |  . . . . . . . . . . .  |
 |  . +---+ . +---+ . +-+  |
 |  o |   | . |   | . | |  |
 |  . +---+ . +---+ . +-+  |
 |  . . . . . . . . . . .  |
 |        +-+   +-+        |
 |  +---+ | | @ | | +---+  |
 |        +-+   +-+        |
 |     [ GHOST HOUSE ]     |
 |  . . . . . . . . . . .  |
 |  o . . . C < . . . . o  |
 |  . . . . . . . . . . .  |
 +--------------------------+
 | LV 3               x x  |
 +--------------------------+
```

## Tech Stack

- **Rendering:** Canvas 2D API
- **Audio:** Web Audio API synthesizer (waka, siren, death, intro melody)
- **Hosting:** Cloudflare Pages
- **Size:** Single `index.html` (~2000 lines), zero dependencies

## Responsive Layout

Adapts to any screen:

| Device                 | D-pad Size | Notes               |
| ---------------------- | ---------- | ------------------- |
| iPhone SE / small      | 52px       | Compact layout      |
| iPhone 14 / medium     | 62px       | Standard            |
| iPhone Pro Max / large | 74px       | Comfortable         |
| Desktop                | Hidden     | Keyboard hint shown |

## Deploy

```bash
npx wrangler pages deploy . --project-name pacman --branch main --commit-dirty=true
```

## Testing

Open [/test.html](https://pacman.petrochenko.info/test.html) in a browser:

- **72 unit tests** -- state init, movement, scoring, ghost AI, release thresholds, speeds, frighten mode, mode switching, fruit, level progression, intermissions, kill screen
- **Simulation fuzzer** -- 10,000 frames of random gameplay checking 10 invariants per frame (bounds, stuck states, memory leaks, timer underflows)

## Architecture

Everything lives in `index.html`. Sections:

1. CSS -- responsive layout, D-pad, overlay
2. HTML -- canvas, score header, lives row, D-pad, start/gameover overlay
3. Constants -- maze template, ghost colors, speed tables, frighten durations, Elroy thresholds
4. Game state -- score, lives, level, mode timers, fruit
5. Maze -- init, wall checks, kill screen corruption
6. Pac-Man -- creation, movement with grid snapping, eating slowdown
7. Ghosts -- AI targeting, scatter/chase/frighten, house bounce, 3-phase exit, Elroy
8. Mode switching -- scatter/chase timer with arcade durations per level tier
9. Speed helpers -- per-level tables for Pac-Man, ghosts, tunnel, frightened, eaten
10. Fruit -- 8 types, spawn at dot thresholds, timer
11. Drawing -- maze, Pac-Man, ghosts, fruit, eyes, score popups
12. Overlay -- HTML-based start/gameover screens with real button
13. Input -- keyboard, D-pad touch/pointer, canvas swipe
14. Game logic -- collision, death, level complete, flash animation
15. Intermissions -- 3 cutscene acts with sprite animation
16. Audio -- Web Audio synth: waka, power pellet, eat ghost, death, intro, siren
17. Init -- canvas setup, high score from localStorage, resize

## License

For educational purposes. PAC-MAN is a trademark of Bandai Namco Entertainment Inc.
