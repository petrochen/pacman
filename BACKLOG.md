# Pac-Man Backlog

## Priority 1 — Core Arcade Accuracy

- [x] BUG-1: Fix canMove collision (ceil-1 bug)
- [x] BUG-2: Fix ghost grid alignment (13.5\*CELL)
- [x] BUG-3: Fix ghost movement snap-back loop
- [x] BUG-4: Fix ghost house exit (bounce overwrite)
- [ ] ARC-1: Extra life at 10,000 points
- [ ] ARC-2: Elroy mode (Blinky speeds up when few dots remain)
- [ ] ARC-3: Level 19+ disables frightened mode (pellets don't scare ghosts)
- [ ] ARC-4: Exact frighten durations per level (arcade table, not formula)
- [ ] ARC-5: Pac-Man eating slowdown (-10% speed for 1 frame on dot eat)
- [ ] ARC-6: Global dot counter reset on death (ghost release resets)

## Priority 2 — Visual Polish

- [ ] VIS-1: Level complete animation (maze flashes white/blue 4 times)
- [ ] VIS-2: Improve death animation timing (match arcade 1.5s sequence)
- [ ] VIS-3: Ghost score popup (show 200/400/800/1600 briefly when eating ghost)

## Priority 3 — Nice to Have

- [ ] NTH-1: Intermission cutscenes (after levels 2, 5, 9)
- [ ] NTH-2: Kill screen at level 256
- [ ] NTH-3: Attract mode / demo playback on start screen
