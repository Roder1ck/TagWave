# TagWave — Music Metadata Tagger

> Vibe Coded AI slop to manage audio files.

Drop in your MP3s, FLACs, and WAVs — TagWave fingerprints them against Shazam's catalog and fills in title, artist, album, genre, BPM, and cover art automatically. Remix detection catches edits, bootlegs, and VIP cuts by scanning track titles. Songs get sorted into broad genre folders (House, Techno, DnB, Bass, Trance…) using a fully editable genre map, so your library stays organized without being over-categorized. Export a metadata JSON and run the included Python script to write tags directly to your files.

---

## Features

- **Drag & drop** MP3, FLAC, WAV, M4A, OGG, AIFF files into the browser
- **Shazam-powered recognition** — no API key required, uses the public fingerprint endpoint
- **Remix detection** — auto-flags remixes, edits, bootlegs, flips, VIPs, and club mixes
- **Editable metadata** — title, artist, album, year, genre, BPM, label, remixed-by
- **Cover art** pulled automatically from Shazam
- **Genre → folder mapping** — broad categories, not exact genre tags (Deep House → `House/`, Hard Techno → `Techno/`, etc.) — fully editable in the UI
- **Folder override** — manually reassign any track to a different folder
- **Export JSON** — download all recognized metadata to pass into the Python tagger
- **Python tag writer** — `tagger.py` writes ID3/FLAC tags and sorts files on disk

---

## How to use

### Browser app (tagging + recognition)

1. Open the app (see [Deploy](#deploy-to-github-pages) below or just open `index.html` locally)
2. Drag audio files into the queue
3. Click **Recognize all** — TagWave fingerprints each file against Shazam
4. Select any file to review and edit its metadata in the editor
5. Adjust genre folder mappings in the **Genre map** tab if needed
6. Click **Export metadata JSON** to download `metadata.json`

### Python tag writer (writing tags to disk)

The browser can't write ID3/FLAC tags directly — use `tagger.py` for that.

```bash
pip install shazamio mutagen
python tagger.py
```

Point `INPUT_DIR` at your audio folder and `META_JSON` at the exported `metadata.json`. The script writes tags to each file and copies it into the sorted folder structure under `OUTPUT_DIR`.

---

## Deploy to GitHub Pages

1. **Create a repo** — github.com → New repository → initialize with a README
2. **Upload files** — add `index.html` (and optionally `tagger.py` and this `README.md`) to the root
3. **Enable Pages** — Settings → Pages → Source: Deploy from branch → Branch: `main` → Folder: `/ (root)` → Save

Your app goes live at:
```
https://yourusername.github.io/your-repo-name
```

No build step, no dependencies, no server.

---

## Genre folder map (defaults)

Shazam returns specific genre tags. TagWave maps them to broader folders so your library doesn't end up with 40 subfolders.

| Shazam genre | Folder |
|---|---|
| Deep House, Tech House, Progressive House, Afro House | `House/` |
| Melodic Techno, Hard Techno, Detroit Techno, Industrial | `Techno/` |
| Drum and Bass, Liquid DnB, Jungle, Neurofunk | `DnB/` |
| Dubstep, Riddim, Trap, UK Bass, Future Bass | `Bass/` |
| Trance, Psytrance, Uplifting Trance | `Trance/` |
| Hip-Hop, Rap, Boom Bap, Drill | `Hip-Hop/` |
| Electronic, EDM, Electro | `Electronic/` |
| Ambient | `Ambient/` |
| Pop, Indie Pop | `Pop/` |
| R&B, Soul | `R&B/` |
| Jazz, Nu-Jazz | `Jazz/` |
| Afrobeats, Latin, Reggaeton | `World/` |
| Everything else | `Unsorted/` |

All mappings are editable in the **Genre map** tab — changes apply immediately to the current session.

---

## Stack

| Layer | Tool |
|---|---|
| Frontend | Vanilla HTML / CSS / JS — zero dependencies |
| Recognition | Shazam public fingerprint API (via corsproxy.io) |
| Python tagger | [shazamio](https://github.com/shazamio/ShazamIO) + [mutagen](https://mutagen.readthedocs.io/) |
| Hosting | GitHub Pages |

---

## Limitations

- **CORS proxy** — the browser version routes through `corsproxy.io` to reach Shazam. For bulk runs (100+ files) or production use, run `tagger.py` directly — it calls Shazam natively with no proxy.
- **Browser tag writing** — browsers cannot write ID3 or FLAC tags to files on disk. Use the exported JSON + `tagger.py` to actually tag and sort your files.
- **Shazam match rate** — works best on commercially released tracks. Unreleased white labels, acapellas, and heavily processed edits may not match.
- **No persistence** — the browser app holds state in memory only. Refresh = start over. Export your JSON before closing.

---

## License

MIT — do whatever you want with it.