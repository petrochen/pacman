# Pac-Man Test Plan

## How to Test

Open https://pacman.petrochenko.info in browser. Test on desktop (Chrome) and mobile (iPhone Safari).
For level-specific tests, reach the level by playing or use browser console:

```js
level = N;
dots = 0; // triggers level complete
```

---

## TC-1: Start & Controls

| #   | Test Case                  | Steps                                | Expected                                          |
| --- | -------------------------- | ------------------------------------ | ------------------------------------------------- |
| 1.1 | INSERT COIN button         | Open page, click INSERT COIN         | Game starts, overlay hides, READY! shows          |
| 1.2 | Keyboard start             | Open page, press any key             | Game starts                                       |
| 1.3 | Arrow keys                 | During gameplay, press arrows        | Pac-Man changes direction                         |
| 1.4 | WASD keys                  | During gameplay, press WASD          | Same as arrows                                    |
| 1.5 | D-pad on mobile            | Open on phone, tap D-pad buttons     | Pac-Man moves in pressed direction                |
| 1.6 | Swipe on mobile            | Swipe on canvas during gameplay      | Pac-Man changes direction                         |
| 1.7 | D-pad hidden on desktop    | Open on desktop                      | D-pad not visible, "Arrow keys / WASD" hint shown |
| 1.8 | Button doesn't steal focus | Click INSERT COIN, then press arrows | Arrows work immediately (no need to click canvas) |

## TC-2: Pac-Man Movement

| #   | Test Case       | Steps                                             | Expected                                  |
| --- | --------------- | ------------------------------------------------- | ----------------------------------------- |
| 2.1 | Grid movement   | Move Pac-Man                                      | Smooth movement along corridors           |
| 2.2 | Wall collision  | Move toward wall                                  | Pac-Man stops, doesn't pass through       |
| 2.3 | Cornering       | Press next direction before reaching intersection | Direction queued, applied at intersection |
| 2.4 | Tunnel wrap     | Enter left tunnel (row 15)                        | Pac-Man appears on right side             |
| 2.5 | Eating slowdown | Eat dots, observe speed                           | Slight pause on each dot (1 frame)        |
| 2.6 | Pellet slowdown | Eat power pellet                                  | Longer pause (3 frames)                   |

## TC-3: Dots & Scoring

| #   | Test Case            | Steps                       | Expected                                                    |
| --- | -------------------- | --------------------------- | ----------------------------------------------------------- |
| 3.1 | Dot score            | Eat a dot                   | Score +10                                                   |
| 3.2 | Power pellet score   | Eat a power pellet          | Score +50                                                   |
| 3.3 | Ghost chain          | Eat ghosts during frighten  | 200, 400, 800, 1600 in sequence                             |
| 3.4 | Ghost score popup    | Eat a ghost                 | Score value (200/400/...) appears briefly at ghost position |
| 3.5 | High score persist   | Get high score, reload page | High score preserved                                        |
| 3.6 | Extra life at 10k    | Reach 10,000 points         | Lives count increases by 1, sound plays                     |
| 3.7 | Extra life once only | Continue past 10k           | No second extra life                                        |

## TC-4: Ghost Behavior

| #    | Test Case                 | Steps                          | Expected                                     |
| ---- | ------------------------- | ------------------------------ | -------------------------------------------- |
| 4.1  | Blinky starts outside     | Start game                     | Red ghost moves immediately, not in house    |
| 4.2  | Pinky exits immediately   | Start game                     | Pink ghost exits house within ~2 seconds     |
| 4.3  | Inky exits at 30 dots     | Eat 30 dots (level 1)          | Cyan ghost exits house                       |
| 4.4  | Clyde exits at 60 dots    | Eat 60 dots (level 1)          | Orange ghost exits house                     |
| 4.5  | Vertical house exit       | Watch ghost exiting            | Ghost moves to center, then UP through door  |
| 4.6  | Different exit directions | Watch multiple ghosts exit     | They go in alternating left/right directions |
| 4.7  | Scatter mode              | First 7 seconds                | Ghosts move toward corners, not chasing      |
| 4.8  | Chase mode                | After 7 seconds                | Ghosts actively pursue Pac-Man               |
| 4.9  | Mode reversal             | Wait for scatter/chase switch  | Ghosts briefly reverse direction at switch   |
| 4.10 | Blinky chase              | During chase mode              | Red ghost goes directly to Pac-Man           |
| 4.11 | Pinky ambush              | During chase mode              | Pink ghost targets 4 tiles ahead of Pac-Man  |
| 4.12 | Clyde threshold           | Get close to orange ghost      | Clyde retreats to corner when within 8 tiles |
| 4.13 | Tunnel slowdown           | Ghost enters tunnel            | Ghost visibly slows in tunnel area           |
| 4.14 | Backup release timer      | Stop eating dots for 4 seconds | Next ghost exits house anyway                |

## TC-5: Elroy Mode (Blinky Speed-Up)

| #   | Test Case               | Steps                       | Expected                  |
| --- | ----------------------- | --------------------------- | ------------------------- |
| 5.1 | Elroy 1                 | Level 1, 20 dots remaining  | Blinky noticeably faster  |
| 5.2 | Elroy 2                 | Level 1, 10 dots remaining  | Blinky even faster        |
| 5.3 | Higher level thresholds | Level 5+, 50 dots remaining | Elroy 1 activates earlier |

## TC-6: Frightened Mode

| #   | Test Case                 | Steps                       | Expected                                       |
| --- | ------------------------- | --------------------------- | ---------------------------------------------- |
| 6.1 | Frighten activation       | Eat power pellet            | All active ghosts turn blue, reverse direction |
| 6.2 | Frighten duration L1      | Eat pellet on level 1       | Ghosts blue for ~6 seconds                     |
| 6.3 | Flash warning             | Last ~2 seconds of frighten | Ghosts flash white/blue                        |
| 6.4 | Frighten ends             | Timer expires               | Ghosts return to normal colors and behavior    |
| 6.5 | Eaten ghost eyes          | Eat a frightened ghost      | Ghost becomes eyes only, rushes to house       |
| 6.6 | Eyes re-enter house       | Watch eaten ghost           | Eyes reach house, ghost re-enters and exits    |
| 6.7 | Level 19+ no frighten     | Reach level 19, eat pellet  | Score +50 awarded but ghosts do NOT turn blue  |
| 6.8 | Shorter frighten by level | Play levels 1-5             | Duration visibly decreases each level          |

## TC-7: Lives & Death

| #   | Test Case                 | Steps                    | Expected                                           |
| --- | ------------------------- | ------------------------ | -------------------------------------------------- |
| 7.1 | Starting lives            | Start game               | 3 lives shown (2 Pac-Man icons below maze)         |
| 7.2 | Death animation           | Collide with ghost       | Pac-Man shrinks/collapses, sound plays             |
| 7.3 | Respawn                   | Die with lives remaining | After 2s delay: READY! text, new positions, resume |
| 7.4 | Life decrement            | Die                      | Lives display shows one less                       |
| 7.5 | Game over                 | Lose all lives           | GAME OVER overlay with PLAY AGAIN button           |
| 7.6 | Play again                | Click PLAY AGAIN         | Fresh game starts (score 0, 3 lives, level 1)      |
| 7.7 | Ghost release after death | Die and respawn          | Ghosts exit house faster (dot counter credit)      |

## TC-8: Level Progression

| #   | Test Case            | Steps                 | Expected                                           |
| --- | -------------------- | --------------------- | -------------------------------------------------- |
| 8.1 | Level complete       | Eat all dots          | "Won" state triggers                               |
| 8.2 | Flash animation      | Complete a level      | Maze flashes white/blue 4 times                    |
| 8.3 | Next level starts    | After flash animation | New level: all dots reset, level counter +1        |
| 8.4 | Difficulty increases | Play levels 1-5       | Ghost speed increases, frighten duration decreases |
| 8.5 | Level display        | Complete levels       | "LV N" updates correctly below maze                |

## TC-9: Fruit

| #   | Test Case        | Steps                           | Expected                                     |
| --- | ---------------- | ------------------------------- | -------------------------------------------- |
| 9.1 | First fruit      | Eat 70 dots                     | Fruit appears below ghost house              |
| 9.2 | Second fruit     | Eat 170 dots                    | Second fruit appears                         |
| 9.3 | Fruit disappears | Don't eat fruit for ~10 seconds | Fruit disappears                             |
| 9.4 | Fruit scoring    | Eat fruit on level 1            | +100 (cherry), sound plays                   |
| 9.5 | Fruit per level  | Higher levels                   | Different fruit types with increasing values |

## TC-10: Intermission Cutscenes

| #    | Test Case                | Steps                 | Expected                                 |
| ---- | ------------------------ | --------------------- | ---------------------------------------- |
| 10.1 | After level 2            | Complete level 2      | "ACT 1" cutscene plays (~6s)             |
| 10.2 | After level 5            | Complete level 5      | "ACT 2" cutscene plays                   |
| 10.3 | After level 9            | Complete level 9      | "ACT 3" cutscene plays                   |
| 10.4 | Cutscene ends            | Wait through cutscene | Next level starts normally               |
| 10.5 | No cutscene other levels | Complete level 3      | No intermission, level 4 starts directly |

## TC-11: Kill Screen (Level 256)

| #    | Test Case            | Steps                                        | Expected                                  |
| ---- | -------------------- | -------------------------------------------- | ----------------------------------------- |
| 11.1 | Trigger              | Set `level = 255` in console, complete level | Level 256 loads with corrupted right half |
| 11.2 | Left half normal     | Observe left side                            | Normal maze rendering                     |
| 11.3 | Right half corrupted | Observe right side                           | Glitchy colors, random symbols            |
| 11.4 | Unwinnable           | Try to eat all dots                          | Some dots unreachable in corrupted area   |

## TC-12: Audio

| #    | Test Case          | Steps                | Expected                       |
| ---- | ------------------ | -------------------- | ------------------------------ |
| 12.1 | Intro melody       | Start game           | Short melody plays             |
| 12.2 | Waka sound         | Eat dots             | Waka-waka sound on each dot    |
| 12.3 | Power pellet sound | Eat pellet           | Rising tone                    |
| 12.4 | Siren              | During gameplay      | Background siren, pitch varies |
| 12.5 | Frightened siren   | During frighten mode | Lower-pitched siren            |
| 12.6 | Death sound        | Die                  | Descending tone                |
| 12.7 | Eat ghost sound    | Eat frightened ghost | Chirp sound                    |

## TC-13: Responsive Layout

| #    | Test Case      | Steps                   | Expected                              |
| ---- | -------------- | ----------------------- | ------------------------------------- |
| 13.1 | Desktop        | Open on desktop browser | Full maze, no D-pad, keyboard hint    |
| 13.2 | iPhone SE      | Open on small phone     | Smaller D-pad (52px), everything fits |
| 13.3 | iPhone 14      | Open on medium phone    | Medium D-pad (62px), comfortable      |
| 13.4 | iPhone Pro Max | Open on large phone     | Large D-pad (74px)                    |
| 13.5 | iPad           | Open on tablet          | Scales appropriately                  |
| 13.6 | Landscape      | Rotate phone            | Game still visible and playable       |

---

## Quick Smoke Test (2 minutes)

1. Open page -> click INSERT COIN -> game starts
2. Move with arrows -> Pac-Man moves, eats dots (+10)
3. Ghosts exit house one by one
4. Eat power pellet -> ghosts turn blue -> eat ghost -> score popup shows
5. Die -> respawn works -> lives decrease
6. Complete level -> maze flashes -> next level
7. Test on phone -> D-pad works
