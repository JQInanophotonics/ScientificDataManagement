# 05 — README template

Every paper-data repo gets a `README.md` built from the template below. Its job is **indexing and explaining the data** — the map between manuscript figures, `Data/` folders, notebook sections, and SVG outputs, plus the conventions needed to interpret the files. It is *not* a tutorial (running the notebook is obvious).

Copy, fill in, delete what doesn't apply:

```markdown
# <Paper title>

<Author list> — <journal>, <status: in preparation / submitted / published + link>

<2–3 sentence abstract, in plain words.>

## Figure index

| Figure | Data folder                   | Notebook section | Output                          |
|--------|-------------------------------|------------------|---------------------------------|
| Fig. 1 | — (schematic, no data)        | —                | Illustrator only                |
| Fig. 2 | `Data/<Fig2Topic>/`           | `# Figure 2`     | `svgs/<...>.svg`                |
| Fig. 3 | `Data/<Fig3Topic>/`           | `# Figure 3`     | `svgs/<...>.svg`                |
| SFig 1 | `Data/Supmat/<SFig1Topic>/`   | `## <SFig1>`     | `svgs/SupMats/<...>.svg`        |

## Data dictionary

Recurring columns across the data files:

| Column | Meaning                              | Units |
|--------|--------------------------------------|-------|
| `freq` | optical frequency                    | Hz    |
| `S`    | optical power spectral density       | dBm   |
| `mu`   | mode index relative to the pump      | —     |
| `frep` | repetition rate                      | Hz    |

Filename convention: `<Type>_<Setup>_<YYYY-MM-DD>_<Device>_<Condition>.csv`

- Measurement prefixes: `OSA`, `RSA`, `PhaseNoise`, `Adev`, `FreqCount`, …
- Device codes: `RW850` = ring width 850 nm; `W2_F11_D4_G450_Lc18_Wg470` =
  wafer / field / die / gap / coupling length / waveguide width (nm).

## Raw data

This repo holds processed, plot-ready data only. Raw acquisition data:
`GoogleDrive: LAB/<project>/<setup>/<dates>/` — folders occasionally move
under `Archive/`; search the path name to relocate them.

## Environment

Python <x.y>; packages pinned in `requirements.txt`; all plotting through
the vendored [`pyprettyplot/`](https://github.com/JQInanophotonics/ScientificGraphicDesign/tree/main/Plotting/pyprettyplot).
```

Notes:

- **Figure index**: one row per figure, including data-less schematic figures (say so explicitly). If the manuscript figure numbering changes during review, update the table — it must always match the current draft.
- **Data dictionary**: only the columns that recur across files; a one-off column can be explained in the notebook section that uses it. Always give units.
- **Raw data**: a Google Drive *path*, never the data itself (that can be GBs). The path name is enough to find the folder again even if it gets moved into an archive.
- **Environment**: `pyprettyplot` always comes from [ScientificGraphicDesign/Plotting/pyprettyplot](https://github.com/JQInanophotonics/ScientificGraphicDesign/tree/main/Plotting/pyprettyplot) — keep the link so readers know where the canonical package lives.
- **At publication**: add a `LICENSE` file (the repo goes public) and update the status line with the journal reference.

Next: the annotated [example walkthrough](Example-OctaveSelfKIS.md).
