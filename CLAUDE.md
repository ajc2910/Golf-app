# Fairway Score — Golf Performance PWA

## Project Overview

A Progressive Web App (PWA) for Android that tracks golf performance beyond a simple stroke count. Instead of measuring only score relative to par, it evaluates **shot execution quality** across eight categories and combines that with a **hole outcome modifier** to produce a composite performance score.

Installed on a Samsung Galaxy S25 Ultra via Chrome's "Add to Home Screen" and hosted on GitHub Pages at:
`https://ajc2910.github.io/Golf-app`

---

## Scoring System

### Shot Quality (logged per shot, at end of each hole)

Each shot is tagged to a category and rated on a **1–5 scale** (1 = worst, 5 = best).
Composite points are **centred on 3** (neutral), so a "3" neither helps nor hurts the score:

| Rating | Label | Points |
|--------|-------|--------|
| 5      | Great | +2     |
| 4      | Good  | +1     |
| 3      | OK    |  0     |
| 2      | Weak  | -1     |
| 1      | Poor  | -2     |

### Shot Categories

| Category   | Description                          |
|------------|--------------------------------------|
| Driving    | Tee shots with driver                |
| Long Iron  | Long iron / hybrid approaches        |
| Short Iron | Mid-iron approaches                  |
| Wedge      | Wedge approaches and pitches         |
| Bunker     | Sand play                            |
| Short Game | Chipping and bump-and-run            |
| Putting    | All putts                            |
| Recovery   | Punch-outs, unplanned scramble shots |

### Hole Outcome Modifier

Applied once per hole based on score vs par:

| Result    | Modifier |
|-----------|----------|
| Eagle+    | +3       |
| Birdie    | +2       |
| Par       | +1       |
| Bogey     |  0       |
| Double    | -1       |
| Triple+   | -2       |

Bogey is the neutral anchor — a respectable club golf score that neither rewards nor penalises.

### Hole Score Formula

```
Hole Score = Sum of shot ratings + Hole modifier
```

### Round Summary

- **Total Performance Score** = sum of all hole scores
- **Category Quality** = average rating (out of 5) per category
  - e.g. drives rated 5, 4, 3 → average 4.0
- **Hole-by-hole table** with par, strokes, result label, and points
- **Round history & stats** — completed rounds are saved to `localStorage` and shown on a
  History screen with career averages (avg score, avg vs par) and per-category trend bars
- **Save to cloud** — the round exports as CSV via the Web Share API; on Android the share
  sheet offers "Save to Drive" so scorecards accumulate in Google Drive over time

---

## File Structure

```
Golf-app/
├── index.html      # Complete app — HTML + vanilla React (via CDN) + CSS + JS
├── manifest.json   # PWA manifest (name, theme colour, icons, display mode)
├── sw.js           # Service worker — caches assets for offline use
└── README.md       # This file
```

---

## Technical Notes

- **No build step** — React and Babel loaded via CDN (unpkg). Single `index.html` is the entire app.
- **State persistence** — `localStorage` saves the round in progress so phone lock / browser close doesn't lose data.
- **Offline support** — Service worker caches all assets on first load; app works on course without signal.
- **Mobile-optimised** — `inputMode="numeric"` for score entry, `100dvh` for proper Android viewport, `safe-area-inset` padding for notch/gesture bar.
- **No framework dependencies** beyond React 18 + Babel standalone (both CDN).

---

## Hosting

Hosted on **GitHub Pages** (free):

- Repo: `https://github.com/ajc2910/Golf-app`
- Live URL: `https://ajc2910.github.io/Golf-app`
- Branch: `main`, root folder `/`

To update the app: edit `index.html` in the repo and commit. GitHub Pages redeploys automatically within ~60 seconds.

---

## Potential Enhancements

- [x] Round history — save completed rounds to `localStorage` and display a history screen
- [x] Export to CSV — download round data for analysis in Excel / Python
- [ ] Handicap-adjusted par targets per hole
- [ ] Course library — save common courses with pre-filled par values per hole
- [ ] Trend charts — rolling category quality % across last N rounds
- [ ] Weather/conditions notes per round
