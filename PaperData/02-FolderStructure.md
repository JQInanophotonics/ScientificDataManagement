# 02 — Folder structure

The layout of a paper-data repository is always the same:

```
2025-PaperData-ShortName/
├── .gitattributes          # LFS rules (see 01)
├── .gitignore
├── FigurePlotting.ipynb    # ONE notebook regenerating every figure
├── Data/                   # ONE folder per figure — no more, no less
│   ├── <Fig1Topic>/        # data behind main-text figure 1
│   ├── <Fig2Topic>/        # data behind main-text figure 2
│   └── Supmat/             # one subfolder per supplementary figure
│       ├── <SFig1Topic>/
│       └── <SFig2Topic>/
├── svgs/                   # figure outputs exported by the notebook
│   └── SupMats/            # supplementary figure outputs
└── pyprettyplot/           # lab plotting package — provided, mandatory
```

## `Data/` — one folder per figure

The rule is strict: **`Data/` contains exactly as many folders as the paper has figures.** Count the figures in the manuscript, count the folders — the numbers must match. Each folder holds *all* the data its figure needs, and *nothing else*. If a dataset feeds two figures, it lives in the folder of the figure that introduces it.

Folders are named after the **physics of the figure**, not the instrument or the measurement day:

```
Data/ExpSelfKIS/            → Fig. 2 (self-KIS demonstration)
Data/MicrowaveToOptical/    → Fig. 4
Data/OpticalToMicrowave/    → Fig. 5
```

Within a figure folder, subfolders are only for separating logically distinct measurement sets of that same figure when a flat folder would get confusing (`OpticalToMicrowave/Clock/` vs `OpticalToMicrowave/Noise/`; `Non-QuasiHarmonic/` for an alternate regime). They never hold data for another figure.

## `Data/Supmat/` — one folder per supplementary figure

Same rule for the supplementary material — each supplementary figure gets its own directory:

```
Data/Supmat/CEOtuning/
Data/Supmat/Coupling/
Data/Supmat/DifferentGeometries/
Data/Supmat/Simulations/
Data/Supmat/ThermalTuning/
Data/Supmat/WaferRobustness/
Data/Supmat/XCOR/
```

When a section compares several quantities per device (e.g. an optical spectrum and a rep-rate trace for each chip), use parallel subfolders with **identical filenames** so files pair up trivially in code:

```
Data/Supmat/WaferRobustness/OSA/SF_..._W2_F11_D4_....csv
Data/Supmat/WaferRobustness/RepRate/SF_..._W2_F11_D4_....csv   # same name, different quantity
```

## `svgs/`

Every figure the notebook produces is exported as an SVG here (main text at the top level, supplementary in `svgs/SupMats/`). These are the inputs to the final figure assembly in Illustrator — see [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign).

## `pyprettyplot/`

The lab plotting package (fonts, `matplotlibrc`, colors, and helper functions that produce Illustrator-ready figures). The canonical version lives in [ScientificGraphicDesign/Plotting/pyprettyplot](https://github.com/JQInanophotonics/ScientificGraphicDesign/tree/main/Plotting/pyprettyplot) and **using it is mandatory**. Copy the package folder from there directly into the repo root so a clone is self-contained — no "install my other repo first" step for referees. See [04 — Figures & notebook](04-FiguresAndNotebook.md).

Next: [03 — Data files](03-DataFiles.md)
