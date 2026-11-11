# Square

A minimal calorie + TDEE tracker. Snap your scale, snap your food, and it works out your real maintenance calories from your intake and weight trend.

- **Scale** — photo of your scale → weight is read for you (confirm or edit).
- **Food** — photo(s) of a meal with a **2×2 inch orange square** in frame → calories, protein, carbs, fat.
- **Fix** — tap any entry and say what's off. Corrections are saved and fed into future estimates, so it learns your portions.
- **TDEE** — average intake minus weight change (7,700 kcal/kg, regression over the last 28 days), with a ± range that tightens as data builds.
- **Trends** — weight (raw + smoothed) and daily calories, 2W / 1M / 3M / 1Y. Press and drag to read any day.
- **Backup & settings** — export everything to a `.json` file, restore/merge from one.

## Run it on GitHub Pages

1. Create a new repo and upload every file in this folder (including `.nojekyll`).
2. **Settings → Pages →** Source: *Deploy from a branch*, Branch: `main` / root → Save.
3. Open `https://<you>.github.io/<repo>/` on your phone. In Safari: **Share → Add to Home Screen** for a full-screen app.
4. The first time you tap Scale or Food, paste an Anthropic API key from [console.anthropic.com](https://console.anthropic.com). You can change the model there too (default `claude-sonnet-5-5`).

## Where data lives

- **On GitHub Pages:** in your browser's storage (IndexedDB) on that device. Nothing is sent anywhere except photos/prompts to Anthropic's API. Use **Backup & settings → Save backup file** now and then. Clearing site data or switching phones loses anything not backed up.
- **Inside Claude (artifact):** saved to your Claude account.

To move data between the two (or into a future version), export from one and **Restore from file** in the other. Restores merge in without duplicating entries.

## Backup format (`square-backup`, version 1)

```json
{
  "format": "square-backup",
  "version": 1,
  "exportedAt": "2026-11-11T15:40:00.000Z",
  "settings": { "unit": "lb", "calib": [{ "before": "...", "note": "...", "after": "..." }] },
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
