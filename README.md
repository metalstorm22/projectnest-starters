# projectnest-starters

Blank starter files downloaded by [ProjectNest](https://github.com/metalstorm22/ProjectNest) when a template includes creative file types.

## How it works

ProjectNest fetches `manifest.json` from the latest release, shows available packs in **Settings → Placeholder Files → Starter Packs**, and caches downloaded files at:

```
~/Library/Application Support/ProjectNest/StarterFiles/generated/
```

## Packs

| Pack | Files |
|------|-------|
| Sketch | `.sketch` |
| Adobe Creative Suite | `.ai`, `.aep`, `.indd` |
| 3D | `.blend` |

> **Note:** `.indd` is a 1-byte stub. To get a real blank InDesign document, open InDesign → New Document → Save As, then follow the update workflow below.

---

## Maintenance

> **First time:** Clone this repo to a permanent location — don't work from `/tmp`.
> ```bash
> git clone https://github.com/metalstorm22/projectnest-starters.git ~/Desktop/Projects/projectnest-starters
> ```

### Replacing an existing file

For example, replacing `blank.aep` with a real one saved from After Effects:

```bash
cp ~/Downloads/blank.aep starters/blank.aep
git add . && git commit -m "Better blank AEP" && git push
gh release upload v1.0.0 starters/blank.aep --clobber
```

Also update `sizeBytes` in `manifest.json` if the file size changed, then:

```bash
gh release upload v1.0.0 manifest.json --clobber
```

### Adding a new file type to an existing pack

1. Copy your file into `starters/`
2. Open `manifest.json` and add an entry to the relevant pack:
   ```json
   {
     "extension": "fig",
     "displayName": "Figma File",
     "downloadURL": "https://github.com/metalstorm22/projectnest-starters/releases/latest/download/blank.fig",
     "sizeBytes": 1234
   }
   ```
3. Commit, push, and upload:
   ```bash
   git add . && git commit -m "Add blank Figma file" && git push
   gh release upload v1.0.0 starters/blank.fig manifest.json --clobber
   ```

### Adding a whole new pack

Same as above — add your files to `starters/`, then add a new pack object to `manifest.json`:

```json
{
  "id": "my-new-pack",
  "name": "My New Pack",
  "description": "Description shown in ProjectNest",
  "files": [
    {
      "extension": "ext",
      "displayName": "Display Name",
      "downloadURL": "https://github.com/metalstorm22/projectnest-starters/releases/latest/download/blank.ext",
      "sizeBytes": 1234
    }
  ]
}
```

Then commit, push, and upload the new file + updated manifest:

```bash
git add . && git commit -m "Add new pack" && git push
gh release upload v1.0.0 starters/blank.ext manifest.json --clobber
```

### Creating a new versioned release (optional)

If you prefer clean version history instead of updating `v1.0.0` in place:

```bash
gh release create v1.1.0 manifest.json starters/blank.* \
  --title "v1.1.0 — Added ..." \
  --notes "What changed"
```

GitHub's `/releases/latest/` automatically points to the newest release, so the app picks it up with no changes needed.

---

## Key rule

`manifest.json` in the release is what the app reads first. Any time you change which files are available — always re-upload it with `--clobber`.
