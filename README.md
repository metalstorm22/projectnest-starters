# projectnest-starters

Blank starter files downloaded by [ProjectNest](https://github.com/metalstorm22/ProjectNest) when a template includes creative file types.

## How it works

ProjectNest fetches `manifest.json` from the latest release, shows available packs in Settings → Placeholder Files → Starter Packs, and caches downloaded files at `~/Library/Application Support/ProjectNest/StarterFiles/generated/`.

## Adding or replacing files

1. Create a blank document in the target app (File → New → Save)
2. Replace the corresponding `blank.*` file in this repo
3. Update `sizeBytes` in `manifest.json`
4. Create a new GitHub Release — ProjectNest always fetches from `/releases/latest/`

## Packs

| Pack | Files |
|------|-------|
| Sketch | `.sketch` |
| Adobe Creative Suite | `.ai`, `.aep`, `.indd` |
| 3D | `.blend` |

> **Note:** `.indd` is a 0-byte stub. To get a real blank InDesign document, open InDesign → New Document → Save As, then replace `blank.indd` and update `sizeBytes`.
