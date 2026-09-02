# Tot Board

A single-file learning app for a 2-year-old. Seven modules:

- **Numbers 0-12, Letters, Colors, Shapes, Animals** — each has Teach mode (auto-advancing, spoken, loops forever) and Quiz mode (silent, parent arrows, tap the card to hear the answer). Every teach loop ends with a recap: all items appear in a grid and each one lights up as it is announced, then the loop starts over.
- **Catherine** — says the name, spells it letter by letter with highlights, says it again, then repeats. Change the `NAME_TEXT` constant near the top of the script to spell a different name.
- **Anthems** — two flags, Canada and USA. Tap one to watch that anthem in an embedded YouTube player. This is the one module that needs an internet connection; offline it shows a friendly message instead. Video ids live in the ANTHEMS constant near the top of the script.
- **Memory Match** — pick 3, 6, or 9 pairs in the top bar (6, 12, or 18 cards), drawn from the full animal set, with a running timer. 3 pairs is the right starting size for age 2. Card positions reshuffle every round and never repeat a face in the same spot as the previous round. Tap the circular arrow in the top bar for a new round anytime.

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

Keys: each item has `<id>` (short answer, played on card tap in Quiz, memory flips, and recaps) and `<id>-teach` (full teaching line). Ids are `num-0`...`num-12`, `letter-a`...`letter-z`, `color-red` through `color-white`, `shape-circle` through `shape-diamond`, and `animal-cow`, `animal-duck`, `animal-pig`, `animal-chicken`, `animal-horse`, `animal-dog`, `animal-cat`, `animal-sheep`, `animal-frog`, `animal-bunny`, `animal-lion`, `animal-elephant`.

One-off keys: `count-intro`, `recap-letters`, `recap-colors`, `recap-shapes`, `recap-animals` (the recap opening lines), `name-catherine` (the spoken name), and `memory-win` (the memory game celebration).

The service worker caches audio after first load, so recordings work offline too. Remember to bump the cache version when you add audio.

## Tuning the voice

The app prefers the Samantha voice, then Ava, Allison, Karen. For a warmer sound, download the enhanced version of the voice on the iPad: Settings > Accessibility > Spoken Content > Voices > English. Letter sounds are spoken as 'The B says buh.' The phonetic spellings live in `LETTER_SOUNDS` near the top of the script; tune any that sound off on your device. For perfect phoneme sounds, record custom audio clips for the letter-x-teach keys.
