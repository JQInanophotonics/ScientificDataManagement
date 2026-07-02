# 01 — Repository setup

## Naming

One repository per paper, created in the `JQInanophotonics` organization:

```
YYYY-PaperData-ShortName
```

- `YYYY` — year the project/paper effort started (it will not always match the publication year, that's fine).
- `ShortName` — a short CamelCase or underscore tag for the paper topic: `OctaveSelfKIS`, `PDCS_KIS`, `AimComb`, `BuriedHeaters`, …

Existing examples: `2025-PaperData-OctaveSelfKIS`, `2024-PaperData-KisMultiDKS`, `2021-PaperData-AuxPump`.

Keep the repo **private** during preparation and peer review. It becomes **public when the paper is published** — this is what the "Data availability" statement of the paper will point to, so write everything assuming it will be read by strangers.

## Git LFS — set it up at repo creation

The data files themselves are small (processed data, see [03](03-DataFiles.md)), so LFS is not about the CSVs — it is there for the files that don't diff well and can grow (SVG exports, `.ai`, notebooks, `.ftr`). Since `.gitattributes` must exist **before** those files are first added, set it up when creating the repo and forget about it. Install [Git LFS](https://git-lfs.com):

```bash
git lfs install
```

Then create `.gitattributes` at the repo root:

```gitattributes
*.pkl filter=lfs diff=lfs merge=lfs -text
*.svg filter=lfs diff=lfs merge=lfs -text
*.csv filter=lfs diff=lfs merge=lfs -text
*.feather filter=lfs diff=lfs merge=lfs -text
*.ai filter=lfs diff=lfs merge=lfs -text
*.ipynb filter=lfs diff=lfs merge=lfs -text
```

Commit `.gitattributes` in your **first commit**, before any data file. If you added data before setting up LFS, fix it with `git lfs migrate` before pushing.

## .gitignore

Keep it minimal — only local clutter:

```gitignore
*/__pycache__/
.DS_Store
```

## Commits

- Commit as you work, not in one giant dump at submission time.
- Messages are short and name the figure or dataset touched: `Wafer robust data`, `Update data for CEO tracking with RR`, `Rearange and tidy up notebook`.
- The history then doubles as a log of how the paper's figures evolved.

## Tag each submission round

When the manuscript goes out, tag the repo — plain git tags, nothing else needed:

```bash
git tag v1-submitted     # initial submission
git tag v2-revision      # after responding to referees
git tag v3-published     # version of record
```

This way the exact dataset and figures each referee saw stay recoverable forever, even as the repo keeps evolving during review.

Next: [02 — Folder structure](02-FolderStructure.md)
