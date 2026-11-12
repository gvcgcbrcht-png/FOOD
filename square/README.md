# Square

A minimal calorie + TDEE tracker. Snap your scale, snap your food, and it works out your real maintenance calories from your intake and weight trend.

- **Scale** — photo of your scale → weight is read for you (confirm or edit).
- **Food** — photo(s) of a meal with a **2×2 inch orange square** in frame → calories, protein, carbs, fat.
- **Fix** — tap any entry and say what's off. Corrections are saved and fed into future estimates, so it learns your portions.
- **TDEE** — average intake minus weight change (7,700 kcal/kg, regression over the last 28 days), with a ± range that tightens as data builds.
- **Goals** — tap the TDEE card: set weight gain/loss per week and protein per lb (default 0.9). Today shows how many more calories you need (TDEE + surplus) and protein eaten vs. target.
- **Trends** — separate tab: weight (raw + smoothed) and daily calories, 2W / 1M / 3M / 1Y. Press and drag to read any day.
- **GitHub backup** — every input (weights, meals, notes, corrections, goals, meal photos) is saved to a private repo. The dot at top right shows status; tap it for setup.

## Run it on GitHub Pages

1. Create a new repo and upload every file in this folder (including `.nojekyll`).
2. **Settings → Pages →** Source: *Deploy from a branch*, Branch: `main` / root → Save.
3. Open `https://<you>.github.io/<repo>/` on your phone. In Safari: **Share → Add to Home Screen** for a full-screen app.
4. The first time you tap Scale or Food, paste an Anthropic API key from [console.anthropic.com](https://console.anthropic.com). You can change the model there too (default `claude-sonnet-5-5`).

## Save your data to GitHub

The first time you tap Scale or Food, Setup asks for your Anthropic key and GitHub details (later: tap the dot at top right):

1. **Repo** — a *private* repo just for data, e.g. `gvcgcbrcht-png/FOOD-data` (create it on GitHub first; it can be empty). Don't use the public Pages repo — your weight and meals would be public.
2. **Token** — GitHub → Settings → Developer settings → **Fine-grained tokens** → Generate. Repository access: *Only select repositories* → your data repo. Permissions: **Contents → Read and write**. Paste the `github_pat_…` value.

Every change is committed to that file a few seconds later. When the app opens (or you switch back to it) it pulls the latest, so it works across devices. If two devices edit at once, entries are merged. Files in the repo: `square-data.json` (all numbers, notes, corrections, goals) and `square-data-photos/YYYY-MM.json` (meal photo thumbnails by entry id).

The file uses the backup format below, so any future version of the app can read it straight from the repo.

## Where data lives

- **On GitHub Pages:** on the device (IndexedDB) and in your GitHub data repo. Photos are sent to Anthropic's API for estimates; small thumbnails are kept in the repo.
- **Inside Claude (artifact):** saved to your Claude account.

A future version of the app can load everything straight from `square-data.json` in the repo.

## Data format (`square-backup`, version 1)

```json
{
  "format": "square-backup",
  "version": 1,
  "exportedAt": "2026-11-11T15:40:00.000Z",
  "settings": { "unit": "lb", "goalKgWk": 0.23, "protPerLb": 0.9, "calib": [{ "before": "...", "note": "...", "after": "..." }] },
  "weights": { "2026-11-11": { "date": "2026-11-11", "kg": 81.6, "unit": "lb" } },
  "days": {
    "2026-11-11": {
      "date": "2026-11-11",
      "items": [{
        "id": "e…", "ts": 1794411289000, "name": "Eggs and toast",
        "kcal": 420, "p": 24, "c": 30, "f": 22,
        "items": [{ "food": "egg", "grams": 100, "kcal": 155, "p": 13, "c": 1, "f": 11 }],
        "thumb": "data:image/jpeg;base64,…", "note": "", "sq": true
      }]
    }
  }
}
```

Weights are always stored in kg. `p`/`c`/`f` are grams. `calib` holds your past corrections.

## Note on the API key

The key is stored only in your browser's localStorage and sent only to `api.anthropic.com`. Since the site is public, keep the key off any shared device, and consider a spend limit on it in the Anthropic console.
