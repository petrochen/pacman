# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file browser Pac-Man game. No build step, no dependencies. The entire game is `index.html`.

Live: https://pacman.petrochenko.info (Cloudflare Pages, project name `pacman`)

## Deploy

```bash
CLOUDFLARE_API_KEY=<key> CLOUDFLARE_EMAIL=<email> npx wrangler pages deploy . --project-name pacman --branch main --commit-dirty=true
```

## Architecture

Everything lives in `index.html` (~1385 lines). Sections in order:

1. **CSS** (lines 7-86) -- layout, header, canvas, lives row, D-pad mobile controls, version tag
2. **Google Font** -- `Press Start 2P` loaded from Google Fonts CDN
3. **HTML body** (lines 89-128) -- header (score/title/high-score), `#gameCanvas`, lives row, D-pad buttons, version tag `v5`
4. **JS Constants** (lines 131-181) -- `CELL=16`, `COLS=28`, `ROWS=31`, `MAZE_TEMPLATE` (28x31 grid), wall/dot/ghost colors
5. **Game State** (lines 183-196) -- score, lives, level, frighten timer, game state machine (`start`/`ready`/`playing`/`dead`/`won`/`gameover`)
6. **Resize** (lines 198-206) -- scales canvas to fit viewport width (max 448px)
7. **Maze** (lines 208-234) -- init from template, wall/ghost-wall checks; cell types: 0=dot, 1=wall, 2=empty, 3=power pellet, 4=ghost house door
8. **Pac-Man** (lines 236-250) -- entity creation, start position (13.5, 23), mouth animation state
9. **Ghosts** (lines 252-286) -- 4 ghosts (Blinky/Pinky/Inky/Clyde), scatter targets, start positions, release dot thresholds `[0, 0, 30, 60]`
10. **Ghost AI** (lines 311-452) -- per-ghost targeting (Blinky=direct chase, Pinky=4-ahead ambush, Inky=Blinky-pivot trick, Clyde=distance-threshold scatter), frightened random movement, eaten-eyes return to house, house bounce/release logic
11. **Drawing** (lines 454-749) -- maze rendering (walls with edge highlights, dots, blinking power pellets, ghost door), Pac-Man (directional mouth + eye), ghosts (body wave, eyes with directional pupils, frightened blue/flash), start screen (ghost previews, INSERT COIN button), game over screen
12. **Input** (lines 795-902) -- keyboard (arrows + WASD), D-pad touch/pointer buttons, canvas swipe gestures, tap-to-start on canvas and body
13. **Game Logic** (lines 904-1047) -- `initGame`/`initLevel`, Pac-Man movement with grid snapping, tunnel wrap, dot/pellet eating, frighten mode (reverses ghosts), collision detection (eat ghost for doubling score 200/400/800/1600, or die)
14. **Render & Game Loop** (lines 1084-1149) -- fixed timestep at 60fps (`STEP=1000/60`), accumulator pattern, error display on crash
15. **Audio** (lines 1192-1345) -- Web Audio API: waka-waka (square wave), power pellet (rising sine), eat ghost (square chirps), death (descending sawtooth), intro melody (11-note sequence), siren oscillator (modulated square wave, pitch scales with level)
16. **Init** (lines 1347-1384) -- canvas setup, high score from `localStorage`, resize listener, animated start screen loop, high score save on unload

## Game Mechanics

- **Maze**: classic 28x31 Pac-Man layout with side tunnels (row 15)
- **Scoring**: dot=10, power pellet=50, ghost chain=200/400/800/1600
- **Lives**: 3 (displayed as Pac-Man icons)
- **Levels**: frighten duration decreases by 500ms per level (min 2000ms from base 8000ms)
- **Ghost speeds**: normal=1.8, frightened=0.9 (half), Pac-Man=2.0
- **High score**: persisted in `localStorage` key `pacman_hiscore`

## Key Constants

| Constant             | Value             | Purpose                              |
| -------------------- | ----------------- | ------------------------------------ |
| `CELL`               | 16                | Tile size in pixels                  |
| `COLS`               | 28                | Maze width                           |
| `ROWS`               | 31                | Maze height                          |
| `STEP`               | 1000/60           | Fixed timestep (~16.67ms)            |
| `frightenDuration`   | 8000 - level\*500 | Power pellet effect (ms)             |
| `GHOST_RELEASE_DOTS` | [0, 0, 30, 60]    | Dots eaten before ghost leaves house |
