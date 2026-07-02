# 04 — Figures & notebook

## One notebook to rule them all

Each repo has a single `FigurePlotting.ipynb` at the root that regenerates **every figure of the paper**, main text and supplementary. Not one notebook per figure, not stray scripts scattered around — one entry point.

## Structure: one markdown heading per figure

The notebook is navigated through markdown cells, mirroring the paper:

```
# Figure 2
## Figure 3 Wafer Robustness
# Figure 4: Microwave to Optical
# Figure 5: Optical to microwave
# Supmat
  ## Soliton crystal
  ## Simulations
  ## Different ring comparison
  ## Supmat Noise XCOR
  ## Thermal tuning THz ring
  ## CEO tracking against RR
```

Rules:

- **`#` heading per main-text figure**, `##` subheadings under a `# Supmat` heading for each supplementary figure. Headings, `Data/` folders, and manuscript figures stay in **one-to-one correspondence** — same count, same names, same order.
- Each section starts by defining its own data path and loading its own files, so sections can be run independently after the imports cell:

  ```python
  bpath = pathlib.Path("./Data/OpticalToMicrowave/Noise/")
  ```

- **Relative paths only** (`./Data/...`). The notebook must run from a fresh clone on any machine; absolute paths like `/Users/you/...` are a bug.
- Small derived numbers quoted in the paper text (mean beat frequency, mismatch, conversion factors) are computed in visible cells too — they are results and need to be reproducible like the figures.

## `pyprettyplot` — mandatory

All plotting goes through `pyprettyplot`, the lab plotting package. It bundles the fonts, `matplotlibrc`, colors, and helper functions that make figures come out **ready for SVG export and final assembly in Adobe Illustrator**, consistent across everyone in the group. It will be provided to you — you don't write your own styling, and you don't restyle figures by hand.

- Copy the package folder into the repo root (vendored, so a fresh clone reproduces the figures with no extra install).
- The notebook starts with a single setup cell:

  ```python
  from pyprettyplot import *
  ```

- Every figure in the notebook is made through it.

Keep *processing* helpers (masking, geometry parsing from filenames, colormap builders) defined in the notebook section that uses them.

## Export to `svgs/`

Each figure section ends by exporting its panels as SVG into `svgs/` (main text) or `svgs/SupMats/` (supplementary), with names matching the figure content:

```
svgs/MicrowaveToOptical_main.svg
svgs/PhaseNoise_withProjection.svg
svgs/SupMats/Simulations.svg
```

The SVGs are the handoff point to final figure assembly (labels, layout, schematics) in Illustrator — see [ScientificGraphicDesign](https://github.com/JQInanophotonics/ScientificGraphicDesign). Data-driven content lives in the notebook; cosmetic assembly lives in the `.ai` file. Never hand-edit data curves in Illustrator.

## Before every push, sanity check

1. Restart kernel, **Run All** — no errors, no missing files.
2. Every figure in the current manuscript draft has a corresponding section.
3. No absolute paths, no references to files outside the repo.

Next: the annotated [example walkthrough](Example-OctaveSelfKIS.md).
