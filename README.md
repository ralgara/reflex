# Reflex

A minimalist reaction time game with contemplative sound design. Test your reflexes as golden tokens appear in three positions — hit the correct key before time runs out. The game adapts to your skill level, getting faster as you improve and slowing down when you struggle.

![Game Preview](https://img.shields.io/badge/status-playable-brightgreen) ![License](https://img.shields.io/badge/license-MIT-blue) ![No Dependencies](https://img.shields.io/badge/dependencies-none-success)

## Features

- **Adaptive Difficulty**: Game speed automatically adjusts based on your performance
- **Streak Rewards**: Build combos for exponential score bonuses with particle effects
- **Contemplative Audio**: Hand-crafted Web Audio API soundscape with pentatonic melodies
- **Endless Mode**: Play until you stop — no arbitrary round limits
- **Zero Dependencies**: Single HTML file, no build tools, no frameworks
- **Fully Customizable**: Adjust timing, scoring, and difficulty parameters

## How to Play

### Installation

No installation required. Download `index.html` and open it in any modern web browser.

```bash
# Clone the repository
git clone https://github.com/ralgara/reflex.git
cd reflex

# Open in browser (macOS)
open index.html

# Or just double-click index.html in your file explorer
```

### Controls

Three positions, three keys:

| Position | Key |
|----------|-----|
| Left     | **A** |
| Center   | **S** |
| Right    | **D** |

**Additional Controls:**
- Press any position key to auto-start a new game
- **Escape**: End current game or close overlays
- **Settings**: Click to adjust game parameters (only available when not playing)

### Gameplay

1. A golden token appears at one of three positions
2. Press the matching key (A, S, or D) before the timer expires
3. Earn points based on reaction speed — faster responses score higher
4. Build streaks for massive bonus points and visual celebrations
5. The game speeds up when you succeed and slows down when you miss

**Scoring:**
- Correct hit: `+1000 × (1 - reaction_time / time_limit)` points
- Wrong key/miss: `-500 × (elapsed / time_limit)` points
- Streak bonuses: Exponential rewards at 3, 5, 8, 12, 17, 23, and 30+ combos

**Game Ends When:**
- You press **Escape** to stop voluntarily
- You miss 3 consecutive tokens (idle detection)

## Technical Details

### Architecture

Reflex is built as a single self-contained HTML file containing:
- Semantic HTML structure
- Custom CSS with responsive design using `clamp()` for fluid scaling
- Vanilla JavaScript game engine with no external dependencies

### Sound Design

All audio is procedurally generated using the Web Audio API:

- **Soft water-drop tones** when tokens appear (descending pitch sweep)
- **Muted bell chimes** for correct hits (C5 + G5 harmonic fifth)
- **Deep ripple sounds** for mistakes (descending low-frequency tones)
- **Ascending pentatonic melody** on game start (A minor pentatonic scale)
- **Descending wind-chimes** on game over (contemplative closure)
- **Streak celebration chimes** that intensify with combo length

Each sound includes custom reverb using feedback delay networks for spatial depth.

### Visual Effects

- **Adaptive timer bar** that changes color when time is running out
- **Particle system** using HTML5 Canvas for streak celebrations
- **Animated streak banners** with dynamic text scaling
- **Border flash feedback** with glow effects for hits and misses
- **Responsive UI** that scales seamlessly from mobile to desktop

### Performance Features

- **Idle detection**: Automatically ends game after 3 consecutive misses
- **Escape-to-quit**: Press Escape any time to gracefully end your session
- **Dynamic difficulty**: Token lifetime adjusts between 400ms and 5000ms
- **Clamp-based scaling**: All UI elements scale smoothly across viewport sizes

## Configuration

Click the **Settings** button before starting a game to customize:

| Parameter | Default | Description |
|-----------|---------|-------------|
| Base Points | 1000 | Maximum points for a perfect hit |
| Penalty Points | 500 | Maximum penalty for wrong/missed hits |
| Initial Lifetime | 3000 ms | Starting token display duration |
| Min Lifetime | 400 ms | Fastest possible token duration |
| Max Lifetime | 5000 ms | Slowest possible token duration |
| Speedup Factor | 0.90 | Multiply lifetime on correct (makes it harder) |
| Slowdown Factor | 1.20 | Multiply lifetime on incorrect (makes it easier) |
| Inter-round Pause | 500 ms | Delay between rounds |
| Max Consecutive Misses | 3 | Idle detection threshold |

Settings persist for your current session and reset on page reload.

## Development

### Project Structure

```
reflex/
├── LICENSE          # MIT License
├── PLAN.md          # Original design document
├── README.md        # This file
└── index.html       # Complete game (HTML + CSS + JS)
```

### Making Changes

Simply edit `index.html` in any text editor. The file is organized into clearly commented sections:

- **Lines 7-302**: CSS styles with responsive design rules
- **Lines 304-380**: HTML structure and UI elements
- **Lines 381-1065**: JavaScript game engine, audio system, and particle effects

No build process needed — save and refresh your browser to see changes.

### Key Algorithms

**Adaptive Difficulty:**
```javascript
// On correct hit: speed up
lifetime = max(minLifetime, lifetime × 0.90)

// On miss/wrong: slow down
lifetime = min(maxLifetime, lifetime × 1.20)
```

**Dynamic Scoring:**
```javascript
// Correct: reward speed
score = basePoints × (1 - reactionTime / lifetime)

// Wrong: penalize slowness
score = -penaltyPoints × (elapsedTime / lifetime)

// Streak bonus (quadratic scaling)
bonus = streakLength² × 10
```

## Browser Compatibility

Works in all modern browsers supporting:
- ES6 JavaScript (arrow functions, template literals, const/let)
- Web Audio API
- HTML5 Canvas
- CSS Custom Properties (variables)
- CSS clamp() function

**Tested on:**
- Chrome/Edge 90+
- Firefox 88+
- Safari 14+

## Credits

**Author**: Rafael Algara
**License**: MIT
**Built with**: Vanilla JavaScript, Web Audio API, HTML5 Canvas

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

**Tip**: Start with default settings to learn the mechanics, then crank up the difficulty by lowering the minimum lifetime to 200ms for an extreme challenge.
