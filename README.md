# Tachyon reference tracks

Public database of kart track definitions for **Tachyon Analyzer** (start/finish line, splits, reference trace, length).
The app downloads tracks from here (Library › Tracks › Online) and keeps them up to date; nothing is uploaded by the app.

## Files

- `index.json` – list of tracks:

```json
{ "schema": 1, "updated": "2026-11-04", "tracks": [
  { "id": "4f1c2b3e-…", "name": "Kartodromo La Conca", "country": "IT", "place": "Muro Leccese", "variant": "",
    "version": 1, "updated": "2026-11-04", "length_m": 1250, "sha256": "<sha256 of tracks/<id>.json>", "file": "tracks/4f1c2b3e-….json" } ] }
```

- `tracks/<id>.json` – one track (Tachyon Analyzer track export + `id`, `place`, `variant`, `note`): `sfPoint {lat, lon}`,
  `splitPoints [{lat, lon}]`, optional `sf` / `splits` lines, `L` (m), `lineWidth`, `minLap`, `trace` (≤ 400 points, Douglas–Peucker 0.5 m),
  `traceFrom`, `bbox`.

Rules: `id` is a UUID and never changes (the app pairs local copies by it); every change bumps `version` by 1 and
updates `sha256` + `updated`; `index.json` and the track file are committed together.

## How tracks get here

Only the maintainer publishes, with **Tachyon Track Admin** (`tools/track-admin` in the main repo): import from Unipro `.otrack`,
AiM Race Studio `.ztracks`, GPX/KML or the analyzer's JSON export, edit on the map, validate, and commit through the GitHub API
with a fine‑grained token (Contents: read/write, this repo only). Validation: `node tools/tracks/validate.mjs <checkout>` in the
main repo. Pull requests with corrections are welcome – keep the `id`, bump `version`, recompute `sha256`.

## Licence

Track geometry data: CC BY 4.0 (attribution: Firefly Racing Team / contributors). Trademarks of the circuits belong to their owners.
