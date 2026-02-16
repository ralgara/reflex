# Reflex Game — Implementation Plan

## Technology Choice

**Single-file HTML/CSS/JS** — no build tools, no dependencies, no framework.
Open the file in a browser and play. Simple to develop, deploy, and modify.

## Game Mechanics

### Positions & Keys
| Position | Key |
|----------|-----|
| Left     | A   |
| Center   | S   |
| Right    | D   |

### Token Lifecycle
1. A **token** (e.g. a colored circle) appears at a random position (left / center / right).
2. A **timer** starts counting from 0 ms.
3. The token remains visible for its **lifetime** (initially 3 000 ms).
4. The player presses A, S, or D before the token disappears.
5. If the token expires without input, it counts as a **miss** (wrong + slow).
6. After resolution, a short **inter-round pause** occurs, then the next token appears.

### Scoring

```
If correct:
  score = + BASE_POINTS × (1 - elapsed / lifetime)

If incorrect (wrong key or miss):
  score = - PENALTY_POINTS × (elapsed / lifetime)
```

- A **fast correct** hit yields close to `+BASE_POINTS`.
- A **slow correct** hit yields close to `+0`.
- A **fast wrong** hit yields close to `-0` (small penalty — at least they were fast).
- A **slow wrong** hit or miss yields close to `-PENALTY_POINTS`.

### Difficulty / Speed Curve

- Each **correct** response reduces the token lifetime by `SPEEDUP_FACTOR`.
- Each **incorrect** response increases the token lifetime by `SLOWDOWN_FACTOR`.
- Lifetime is clamped between `MIN_LIFETIME` and `MAX_LIFETIME`.

Formula:
```
On correct: lifetime = max(MIN_LIFETIME, lifetime × SPEEDUP_FACTOR)
On incorrect: lifetime = min(MAX_LIFETIME, lifetime × SLOWDOWN_FACTOR)
```

### Configurable Parameters (defaults)

| Parameter         | Default   | Description                              |
|-------------------|-----------|------------------------------------------|
| `BASE_POINTS`     | 1000      | Max points for a perfect correct hit     |
| `PENALTY_POINTS`  | 500       | Max penalty for a wrong/missed hit       |
| `INITIAL_LIFETIME`| 3000 ms   | Starting token display duration          |
| `MIN_LIFETIME`    | 400 ms    | Fastest possible token duration          |
| `MAX_LIFETIME`    | 5000 ms   | Slowest possible token duration          |
| `SPEEDUP_FACTOR`  | 0.90      | Multiply lifetime on correct (shrink)    |
| `SLOWDOWN_FACTOR` | 1.20      | Multiply lifetime on incorrect (grow)    |
| `INTER_ROUND_MS`  | 500 ms    | Pause between rounds                     |
| `NUM_ROUNDS`      | 20        | Total rounds per game (0 = endless)      |

## UI Layout

```
┌──────────────────────────────────────────────┐
│              REFLEX                          │
│         Score: 1250   Round: 5/20            │
│         Lifetime: 2430 ms                    │
│                                              │
│    ┌────┐      ┌────┐      ┌────┐            │
│    │    │      │ ●  │      │    │            │
│    │ A  │      │ S  │      │ D  │            │
│    └────┘      └────┘      └────┘            │
│                                              │
│     Last: +820 pts (Correct! 540 ms)         │
└──────────────────────────────────────────────┘
```

- Three boxes labeled A, S, D.
- The active box shows a token (filled circle or similar).
- Above: score, round counter, current lifetime.
- Below: feedback on last response (points earned, correct/wrong, elapsed time).
- A start/restart button and a settings panel to adjust parameters.

## File Structure

```
reflex/
├── LICENSE
├── PLAN.md          ← this file
└── index.html       ← entire game (HTML + CSS + JS in one file)
```

## Implementation Steps

1. **Scaffold HTML/CSS** — layout the three-position grid, score display, start button.
2. **Game engine (JS)** — token generation, timer, keyboard input, scoring logic.
3. **Difficulty curve** — lifetime adjustment after each round.
4. **Settings panel** — let the user tweak all configurable parameters before starting.
5. **Game-over screen** — summary after NUM_ROUNDS (total score, accuracy, avg time).
6. **Polish** — animations, colors for correct/wrong feedback, responsive layout.
