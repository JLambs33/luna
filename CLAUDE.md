# Luna — Cycle Tracker

A single-file PWA menstrual cycle tracker. Hosted on GitHub Pages at https://jlambs33.github.io/luna.

## Stack

- Vanilla HTML/CSS/JS — everything in `index.html`
- `manifest.json` for PWA install on iPhone
- All data stored in `localStorage`, no backend

## Git Workflow

### Branch structure

```
main        ← production (GitHub Pages serves this)
staging     ← integration branch, always ahead of or equal to main
feature/*   ← new features, cut from staging
fix/*       ← bug fixes, cut from staging
```

### Rules

- **Never commit directly to `main` or `staging`**
- Cut all feature and fix branches from `staging`:
  ```bash
  git checkout staging && git pull
  git checkout -b feature/your-feature-name
  ```
- Merge back to `staging` via PR on GitHub (`feature/* → staging`)
- Once staging is tested and ready, merge to `main` via a separate PR (`staging → main`)
- GitHub Pages deploys from `main` automatically

### Typical flow

```bash
# Start a new feature
git checkout staging && git pull
git checkout -b feature/add-notes-history

# ... make changes ...

git add index.html
git commit -m "Add notes history view"
git push -u origin feature/add-notes-history

# Open PR: feature/add-notes-history → staging
# Review, merge PR on GitHub
# When ready to ship: open PR staging → main
```

## Architecture

All app logic lives in `index.html`. Key sections (by JS function):

- **State**: `load()`, `save()`, `defaultState()` — localStorage read/write
- **Phase logic**: `getPhaseInfo()`, `getPhaseForDate()` — cycle day and phase calculation
- **Today tab**: `renderToday()`, `getTipCard()` — home screen
- **Calendar**: `renderCalendar()`, `renderStrip()`, `renderMonth()` — week strip + month grid
- **Log tab**: `renderLog()`, `logPeriodStart()`, `logPeriodEnd()`, `saveLog()`
- **Settings**: `renderSettings()`, `exportData()`, `importData()`

## Data model

```json
{
  "mode": "her | partner",
  "cycles": [{ "start": "YYYY-MM-DD", "end": "YYYY-MM-DD | null" }],
  "logs": { "YYYY-MM-DD": { "flow": "string", "symptoms": ["id"], "note": "string" } },
  "avgCycleLen": 28,
  "avgPeriodLen": 5
}
```
