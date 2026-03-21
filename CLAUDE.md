# Slots - Gesture-Controlled Slot Machine

## Overview
A browser-based slot machine game controlled by hand gestures via webcam. Built as a single HTML file with no build step.

**Live**: https://myrondoesnotcode.github.io/slots/
**Repo**: https://github.com/myrondoesnotcode/slots

## Tech Stack
- **HTML5** single-file app (`index.html`)
- **Tailwind CSS** via CDN
- **Vanilla JavaScript** (no framework)
- **MediaPipe Hands** for hand tracking (21-landmark detection)
- **Web Audio API** for procedurally generated sounds (no audio files)

## File Structure
```
index.html          — The entire app (HTML + CSS + JS)
oldoldindex.html    — Archived v18 (TensorFlow-based)
oldindex.html       — Earlier version
lastindex.html      — Earlier version
.claude/launch.json — Dev server config (python3 HTTP on port 8765)
```

## How It Works

### Hand Tracking & Lever Interaction
1. **Hover**: Hand within 150px of lever knob -> knob turns GREEN
2. **Grab**: Make a FIST (3+ fingers curled) -> grabs the lever
3. **Pull**: Drag hand down while fist closed -> lever rotates (max 130deg). At 100deg knob turns YELLOW ("cocked")
4. **Release**: Open hand -> lever springs back, spin triggers if pulled past threshold

### Game Mechanics
- **Symbols**: cherry, lemon, grapes, diamond, 7, bell, clover (7 total)
- **Cost**: 10 credits per spin, starting credits: 1000
- **Jackpot**: 3 matching -> +500 credits
- **Match**: 2 matching -> +50 credits
- **Reels**: 3 reels stop staggered at 1000ms, 1600ms, 2200ms

### Audio (procedural, no files)
- `spin`: Square wave 200->50Hz sweep
- `stop`: Sine 600Hz quick decay
- `win`: 4-note ascending chord (440, 554, 659, 880 Hz)
- `click`: Sine 800Hz, 50ms
- `cocked`: Sawtooth 100Hz, 200ms

### Confetti
100 particles on win, gravity + drift + rotation, auto-clears after 3s.

## Responsive Breakpoints
- **Desktop**: Full scale, 1.1x transform on slot machine
- **Mobile portrait** (`max-width: 480px`): Scaled-down reels (60x85px), smaller lever/fonts
- **Mobile landscape** (`max-height: 500px AND max-width: 900px`): Further compressed for limited vertical space

## Camera & Performance
- Adaptive resolution: 640x360 mobile, 1280x720 desktop
- Adaptive model complexity: lighter on mobile
- Camera timeout: 12s mobile, 20s desktop with "Play Without Camera" fallback
- Loading screen hides after first successful hand detection

## Development
```bash
# Run locally
python3 -m http.server 8765

# Or use Claude launch config
# Defined in .claude/launch.json, serves on port 8765
```

No build step. Push to `main` and enable GitHub Pages to deploy.

## Key State Variables
`isSpinning`, `isHandClosed`, `isGrabbingLever`, `readyToSpin`, `grabStartY` — all globals managing interaction state. Lever angle maps hand Y-delta: `Math.min(Math.max((deltaY * 0.6), 0), 130)`.
