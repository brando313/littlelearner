# Tot Board

A single-file learning app for a 2-year-old. Four modules: Numbers 0-10, Letters, Colors, Shapes. Each has Teach mode (auto-advancing, spoken, loops forever) and Quiz mode (silent, parent arrows, tap the card to hear the answer).

## Files

- `index.html` — the whole app
- `sw.js` — service worker for offline use
- `manifest.json` — install metadata
- `icon-180.png`, `icon-512.png` — home screen icons

## Deploy to GitHub Pages

1. Create a new repo (for example `totboard`) and put these five files at the repo root.
2. In the repo: Settings > Pages > Source: Deploy from a branch > `main`, folder `/ (root)` > Save.
3. Wait a minute, then open `https://<username>.github.io/totboard/` in Safari on the iPad.
4. Tap the share button > Add to Home Screen.
5. Open it once from the home screen icon while online so the service worker caches everything. After that it works offline.

## Shipping updates

The service worker is cache-first, so an installed iPad will keep serving the old version until the cache name changes. When you push changes, bump the version in `sw.js`:

```js
const CACHE = "totboard-v2";
```

Then close and reopen the app twice on the iPad (first open fetches the new worker, second open runs it).

## Custom audio (replacing the robot voice)

Every spoken line has a key. Record an mp3, save it as `audio/<key>.mp3` in the repo, and add the key to `CUSTOM_AUDIO` near the top of the script in `index.html`:

```js
const CUSTOM_AUDIO = new Set(["num-3-teach", "num-3"]);
```

Keys: each item has `<id>` (short answer, played on card tap in Quiz) and `<id>-teach` (full teaching line). Ids are `num-0`...`num-10`, `letter-a`...`letter-z`, `color-red` through `color-white`, and `shape-circle` through `shape-diamond`. The service worker caches audio after first load, so recordings work offline too. Remember to bump the cache version when you add audio.

## Tuning the voice

The app prefers the Samantha voice, then Ava, Allison, Karen. For a warmer sound, download the enhanced version of the voice on the iPad: Settings > Accessibility > Spoken Content > Voices > English. The phonetic spellings the voice reads for letter sounds live in `LETTER_SOUNDS` near the top of the script; adjust any string that sounds wrong on your device.
