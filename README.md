# projectnest-starters

Blank starter files served to [ProjectNest](https://github.com/metalstorm22/ProjectNest) via CloudKit public database. No account or sign-in required for users.

## How it works

ProjectNest queries the `iCloud.ParthGupta.ProjectNest` CloudKit container's public database, shows available packs in **Settings → Placeholder Files → Starter Packs**, and caches downloaded files at:

```
~/Library/Application Support/ProjectNest/StarterFiles/generated/
```

## Packs

| Pack | Files |
|------|-------|
| Sketch | `.sketch` |
| Adobe Creative Suite | `.ai`, `.aep`, `.indd` |
| 3D | `.blend` |

> **Note:** `.indd` is a stub. To replace it, open InDesign → New Document → Save As, then update the `fileData` asset on the CloudKit record.

---

## Maintenance

The [CloudKit Console](https://developer.icloud.com) is your admin panel. Always make changes in the **Development** environment first, test in the app, then deploy.

> **One rule for schema changes** (new record types or fields): go to **Deploy Schema to Production** after testing. Data records in the public database go live immediately — no deploy step needed for data.

---

### Adding a new file to an existing pack

1. Dashboard → Data → Development → **StarterFile** → New Record
2. Fill in the fields:
   - `packID` — must match the pack's `packID` exactly (e.g. `adobe-creative`)
   - `fileExtension` — e.g. `fig`
   - `displayName` — e.g. `Figma File`
   - `fileData` — upload the blank file
   - `sortOrder` — position within the pack (1, 2, 3…)
3. Save — the app picks it up immediately, no app update needed

---

### Adding a whole new pack

1. Dashboard → Data → **StarterPack** → New Record:
   - `packID` — unique ID, no spaces (e.g. `motion`)
   - `name` — display name shown in the app (e.g. `Motion`)
   - `packDescription` — one-line description
   - `sortOrder` — position in the pack list
2. Dashboard → Data → **StarterFile** → New Record for each file in the pack
3. Done

---

### Removing a file or pack

1. Dashboard → Data → find the record → delete it
2. For a whole pack: delete the `StarterPack` record **and** all its `StarterFile` records — CloudKit does not cascade-delete automatically

---

## CloudKit schema

**StarterPack**

| Field | Type | Index |
|-------|------|-------|
| `packID` | String | Queryable |
| `name` | String | — |
| `packDescription` | String | — |
| `sortOrder` | Int(64) | Sortable |
| `recordName` | (metadata) | Queryable |

**StarterFile**

| Field | Type | Index |
|-------|------|-------|
| `packID` | String | Queryable |
| `fileExtension` | String | — |
| `displayName` | String | — |
| `fileData` | Asset | — |
| `sortOrder` | Int(64) | Sortable |
| `recordName` | (metadata) | Queryable |

> `sortOrder` is scoped per pack — two files in different packs can both have `sortOrder = 1` without conflict.
