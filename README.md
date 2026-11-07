# LYRAVOX — Turn Every Lyric Into a Visual Story.

Independent online lyric motion video studio. Write lyrics, add a song, get beat-timed animated typography and export
MP4 / PNG sequence / transparent PNG — all in the browser. No account, no upload, no API keys.

- Developer: **Prince AR Abdur Rahman** · Publisher: **NexVora Lab's Ofc** · Bangladesh · v1.0.0
- Portfolio: https://prince-ar-abdur-rahman.vercel.app
- Based on **JIZURA** by hakoniwa (MIT) — https://github.com/852wa/JIZURA — see `LICENSE`, `THIRD_PARTY_NOTICES.md`.

## What is in the box
| URL | What |
|---|---|
| `/` | Landing page (EN / বাংলা) |
| `/studio/` | Dashboard: projects, import, audio library, lyrics studio, export history (IndexedDB, this browser only) |
| `/editor/` , `/editor/bn/` | The editor (English, বাংলা). Also `/editor/ja/ zh-hant/ zh-hans/ ko/ id/ vi/` |
| `/about/ /developer/ /credits/ /privacy/ /terms/ /contact/` | Pages, `404.html`, `sitemap.xml`, `robots.txt`, `manifest.webmanifest` |

The editor is the original JIZURA engine (860 motion components, beat detection, tap-sync, timeline, MP4 / PNG export,
LRC import/export, project save/load, aspect ratios, resolution/fps, mobile mode), rebranded, with:
Bangla edition (grapheme-cluster-safe text layout, Bengali fonts, translated UI), Studio library integration (autosave to
the library, export logging), LYRAVOX theme and loading screen.

## Build & deploy
Requires Python 3 only (no npm dependencies).

```bash
python3 build.py                      # writes public/
SITE_URL=https://your-domain.com python3 build.py   # canonical URLs, sitemap and Open Graph use this address
python3 -m http.server 8800 --directory public       # preview at http://localhost:8800/
```

`public/` is committed, so **Vercel needs no build step**: import the GitHub repo, framework "Other", output directory
`public` (already set in `vercel.json`). The default `SITE_URL` is `https://lyravox.vercel.app` — set your real domain and rebuild
before publishing so canonical/OG/sitemap URLs are correct.

## Layout of the source
`src/*.js` engine + UI (concatenated in file order) · `src/02c_bangla.js` Bangla language + cluster packing ·
`src/13_lyravox.js` Studio integration · `app/` editor HTML/CSS, i18n glossaries (`app/bn/*.tsv` = Bangla) ·
`app/store.js` shared project library · `site/` site generator + assets · `build.py` · `tests/` browser tests (Playwright) ·
`dev/smoke_all.py` renders every registered motion expression.

## Tests (Playwright + Chromium)
```bash
python3 build.py && python3 -m http.server 8800 --directory public &
python3 tests/editor_bn.py      # Bangla boot, planning, rendering
python3 tests/export_test.py    # audio analysis, MP4 export, library + export history
python3 tests/png_test.py       # PNG sequence + transparent PNG
python3 tests/studio_test.py    # landing, Studio flows, import/export, mobile layout
python3 build.py --dev && python3 dev/build_test.py all --all-packs   # then: python3 dev/smoke_all.py t_all
```

## Notes
- Fonts load from Google Fonts at runtime (not bundled). Offline, the browser falls back to system fonts.
- MP4 export uses WebCodecs: use current Chrome or Edge (Android Chrome included).
- The "AE" button in the editor still writes the After Effects data file; the After Effects panel itself is not part of this
  repository (it is still JIZURA-branded upstream).
