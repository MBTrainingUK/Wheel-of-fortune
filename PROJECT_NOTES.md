# Mercedes-Benz Randomiser — Project Notes
**Last updated:** 29 April 2026  
**File:** `index.html` (single self-contained file, no dependencies or build step)  
**Open in:** Any browser — just double-click `index.html`

---

## What it does

A wheel-of-fortune style randomiser for use as a training icebreaker.

1. Facilitator adds up to **20 participant names** in the right-hand panel
2. Press **Spin the Wheel** — the wheel spins and lands on a random name
3. The selected name appears in a banner below the wheel
4. Press **Remove & Spin Again** — that person is removed and the wheel respins the remaining names
5. This continues until everyone has been selected
6. A **Session Complete** screen is shown at the end with a confetti burst
7. **Start New Session** resets everything

---

## Layout behaviour

| Phase | Layout |
|---|---|
| **Setup** (before first spin) | Participants panel on the right, wheel in centre |
| **Spin mode** (after first spin) | Participants panel collapses/disappears, wheel grows, **Called Out** results list appears on the left |
| **Session Complete** | Wheel hides, full-screen end card shown |

The layout switch is triggered by toggling the `.spinning` class on `<main id="main-layout">`.

---

## Branding

Follows official **Mercedes-Benz Digital Brand Guidelines**:

| Token | Value | Usage |
|---|---|---|
| `--mb-black` | `#000000` | Page background, segment fills |
| `--mb-white` | `#ffffff` | Body text, star logo |
| `--mb-blue` | `#008DFC` | Buttons, pointer, winner banner border, segment fills |
| `--mb-blue-cta` | `#0078D6` | Alternate blue (darker segments) |
| `--mb-grey-1..5` | `#1a1a1a` → `#a0a0a0` | Surfaces, borders, secondary text |

- **Wheel segments** alternate Deep Black and Pure Blue shades
- **Star logo** uses the official Adobe Illustrator SVG path (all three instances)
- **Typography:** Inter (Google Fonts) with wide letter-spacing

---

## Key JS state variables

```js
let names         // string[] — names currently on the wheel
let results       // string[] — ordered list of winners (populated during spin mode)
let currentWinner // string|null — name selected in last spin, waiting to be removed
let isSpinning    // bool — true while wheel animation is running
let currentAngle  // number (radians) — cumulative wheel rotation
let spinMode      // bool — true once first spin has been pressed
```

---

## Key functions

| Function | What it does |
|---|---|
| `spin()` | Picks a random winner, animates wheel, calls `enterSpinMode()` on first run |
| `enterSpinMode()` | Adds `.spinning` to `<main>`, hides participants panel, shows results panel |
| `exitSpinMode()` | Removes `.spinning`, resets `results`, resets results panel HTML |
| `updateResultsList()` | Rebuilds the Called Out ordered list from the `results` array |
| `drawWheel(angle)` | Redraws the canvas wheel at given rotation (called every animation frame) |
| `addName(n)` | Adds a name to `names[]`, updates UI |
| `removeName(n)` | Removes a name by value |
| `clearAll()` | Resets all state and UI back to setup mode |
| `showAllDone()` | Hides wheel elements, shows Session Complete card |
| `launchConfetti(count)` | Spawns CSS-animated confetti pieces |

---

## Things to potentially improve next session

- [ ] **Sound effects** — tick during spin (Web Audio API), chime on reveal
- [ ] **LocalStorage persistence** — save names so a page refresh doesn't wipe the list
- [ ] **Bulk import** — paste a comma/newline separated list instead of typing one by one
- [ ] **Idle animation** — slow rotation of wheel when no spin is in progress
- [ ] **Winner name animation** — letter-by-letter reveal or silver shimmer on the banner
- [ ] **Spin history timestamp** — record time each person was picked in the results list
- [ ] **Mobile layout** — larger touch targets, stacked layout for tablet/phone use

---

## Known quirks

- The canvas is drawn at a fixed **480×480 internal resolution** (hi-DPI scaled by `devicePixelRatio`). CSS `width: 100%; height: 100%` scales it visually when the container grows in spin mode.
- `currentAngle` accumulates across spins (never normalised to 0–2π) — harmless in practice but could theoretically drift over hundreds of spins.
- Google Fonts (Inter) requires an internet connection; offline fallback is generic `sans-serif`.
