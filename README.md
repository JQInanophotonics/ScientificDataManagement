# Scientific Data Management

How we store and tidy data in the group — from the day it is measured to the day the paper is published. Two sections: the day-by-day handling of experimental data, and the packaging of data behind submitted/published papers.

## 1. Daily experimental data *(in progress)*

Storing and tidying experimental data **day by day** as it comes off the setups: where it goes, how it is named, how it stays findable months later. This section lives in [`DailyData/`](DailyData/) and is being written — check back soon.

## 2. Paper data — storing & tidying data of published work

Every paper from the group gets its **own GitHub repository** in the `JQInanophotonics` organization, containing the processed data, the notebook that regenerates every figure, and the figure outputs. The goal is simple:

> **Anyone — a referee, a new student, or you in five years — should be able to clone the repo, run the notebook top to bottom, and regenerate every figure of the paper.**

Read the pages in order the first time; use them as a checklist afterwards.

### The rules, in one screen

1. **One repo per paper**, named `YYYY-PaperData-ShortName` (e.g. `2025-PaperData-OctaveSelfKIS`). Private while the paper is in preparation/review; public at publication.
2. **Only pre-processed, light data** — the plot-ready data behind the figures, in a human-readable format anyone can import: `.csv` with named column headers (`pd.read_csv()`). Raw instrument dumps stay on the lab storage.
3. **If a file is genuinely large**, feather (`.ftr`, `pd.read_feather`) is OK to keep the repo light — and if you use binary formats (`.ftr`, and the `.svg`/`.ai`/`.ipynb` outputs), set up Git LFS for them: `.gitattributes` in the first commit.
4. **One `Data/` folder per figure** — main-text figures at the top level, supplementary figures under `Data/Supmat/`. As many folders as the paper has figures, no more, no less.
5. **Filenames are self-describing**: measurement type, date, device geometry, and conditions — never `final_v2.csv`.
6. **One `FigurePlotting.ipynb` at the repo root**, with a markdown heading per figure and only relative paths (`./Data/...`).
7. **All plotting goes through [`pyprettyplot`](https://github.com/JQInanophotonics/ScientificGraphicDesign/tree/main/Plotting/pyprettyplot)**, the lab plotting package (copied from [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign/tree/main/Plotting) and vendored in the repo). Figure outputs land in `svgs/` as Illustrator-ready SVGs.
8. **Commit early and often**, with short messages saying which figure or dataset changed ("Wafer robust data", "Fix dnu by half"). Tag each submission round: `v1-submitted`, `v2-revision`, `v3-published`.
9. **Every repo has a README built from the [template](PaperData/05-ReadmeTemplate.md)**: figure ↔ folder ↔ notebook index, data dictionary with units, and the Google Drive path of the raw data.

### Pages

| Page | What it covers |
|------|----------------|
| [01 — Repository setup](PaperData/01-RepoSetup.md) | Naming the repo, Git LFS, `.gitattributes`, `.gitignore` |
| [02 — Folder structure](PaperData/02-FolderStructure.md) | `Data/`, `svgs/`, the notebook, and how to lay them out |
| [03 — Data files](PaperData/03-DataFiles.md) | Tidy CSVs, self-describing filenames, what (not) to commit |
| [04 — Figures & notebook](PaperData/04-FiguresAndNotebook.md) | One notebook per paper, one section per figure, SVG exports |
| [05 — README template](PaperData/05-ReadmeTemplate.md) | Boilerplate README for each paper repo: figure index, data dictionary, raw-data pointer |
| [Example — OctaveSelfKIS](PaperData/Example-OctaveSelfKIS.md) | Annotated walkthrough of the reference repo |

### What's in a paper-data repo

```
2025-PaperData-ShortName/
├── .gitattributes              # Git LFS rules — first commit, before any data
├── .gitignore                  # */__pycache__/, .DS_Store — nothing else needed
├── FigurePlotting.ipynb        # THE notebook: regenerates every figure
├── pyprettyplot/               # lab plotting package — provided, mandatory
├── Data/
│   ├── Fig1Topic/              # one folder per MAIN-TEXT figure
│   ├── Fig2Topic/              #   named after the physics, e.g. ExpSelfKIS/
│   ├── ...
│   └── Supmat/
│       ├── SFig1Topic/         # one folder per SUPPLEMENTARY figure
│       └── ...
└── svgs/                       # SVG exports from the notebook, one per figure
    └── SupMats/                # SVG exports of the supplementary figures
```

**`Data/`** holds exactly one folder per figure: count the figures in the manuscript, count the folders — the numbers must match. Each folder contains all the data its figure needs and nothing else; a dataset feeding two figures lives with the figure that introduces it. Folders are named after the physics (`MicrowaveToOptical`), not instruments or dates.

**`pyprettyplot/`** is the group's plotting bundle — fonts, `matplotlibrc`, colors, and helpers that produce figures ready for SVG export and final assembly in Adobe Illustrator. It lives in [ScientificGraphicDesign/Plotting/pyprettyplot](https://github.com/JQInanophotonics/ScientificGraphicDesign/tree/main/Plotting/pyprettyplot) — that is the version you use, and **using it is not optional**: copy it from there into the repo root (so a clone is self-contained), start the notebook with `from pyprettyplot import *`, and make every figure through it.

**`FigurePlotting.ipynb`** mirrors the paper: a single imports cell, then one `#` markdown heading per main-text figure in paper order, then a `# Supmat` heading with one `##` per supplementary figure. Each section is self-contained — it sets its own `bpath = pathlib.Path("./Data/FigNTopic/")`, loads its own files, computes any numbers quoted in the paper text, and ends by exporting its SVG. Before every push: restart kernel, Run All, zero errors.

**`svgs/`** collects those exports (main text at the top level, supplementary in `SupMats/`). They are the handoff to Illustrator for labels and layout — never hand-edit data curves there.

## See also

Sibling wikis in the org: [ScientificWriting](https://github.com/JQInanophotonics/ScientificWriting), [ScientificPresentations](https://github.com/JQInanophotonics/ScientificPresentations), [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign).
