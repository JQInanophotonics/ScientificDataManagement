# 03 — Data files

## What goes in: processed, plot-ready data

The repo holds the **reduced data behind the figures**, not the raw acquisition output.

- ✅ CSV files with a header row of named columns, loadable with a single `pd.read_csv()`.
- ✅ Simulation outputs reduced to the arrays actually plotted (`Mu_vs_Dint.csv`, `eigval_selfKIS_120525.csv`).
- ❌ Raw instrument dumps, scope binaries, GB-scale sweeps — those stay on the lab storage server. If a referee asks for rawer data, we handle it then.
- ❌ Anything you can't explain: if you don't know what a file is, it doesn't get committed.

Every column has a meaningful name. Examples from the reference repo:

```csv
Lc,G,W,freq,Qc,eta                    # coupling Q: geometry params + extracted values
14,300,460,170000000000000,8403.8971,0.9933744313469753
```

```csv
,S,freq,mu,frep,M                     # comb spectrum: PSD, frequency, mode index, ...
0,-57.3359472,176348504705882.34,-154.8179272937047,736594466792.4681,239.6820727062953
```

Use SI base units (Hz, not THz) in the data; convert for display in the notebook. When a table has natural keys (geometry parameters, mode number), store them as columns so the file can be indexed with `index_col=[...]`.

## Filenames are self-describing

A filename should tell you **what was measured, when, on which device, under which conditions** — without opening the file or asking whoever took it. Build it from these blocks, separated by underscores:

```
<What>_<Setup/Method>_<Date>_<Device>_<Condition>.csv
```

Real examples:

```
SelfKIS_PDCS_2025-09-16_RW850_SpectrumKIS.csv
SelfKIS_PDCS_2025-08-19_RW850_RepRateDisciplining_1550nmPump.csv
PhaseNoise_XCOR_CTL1550Free_CEOlocked780.csv
Adev_OptLock264.csv
RSA_RepRate_1550_CEOlocked_RepRateLocked1060_SelfKIS.csv
```

Conventions used throughout:

- **Measurement type prefix**: `OSA` (optical spectrum), `RSA` (RF spectrum), `PhaseNoise`, `Adev`/`Mdev` (Allan/modified deviation), `FreqCount`, `Spec`, `Dispersion`, `KISmap`, …
- **Date** in ISO format `YYYY-MM-DD` when a measurement day matters.
- **Device geometry** encoded compactly: `RW850` (ring width 850 nm), and for wafer-scale data the full fabrication ID `W1_F11_D24_RW860_G450_Lc17_Wg460` (wafer, field, die, ring width, gap, coupling length, waveguide width).
- **Locking/pump conditions** spelled out: `CEOlocked`, `RepRateLocked1060`, `1550nmPump`, `FreeRuning`.
- **Parameter sweeps** keep one file per point with the swept value in the name: `..._RR23_2.4V.csv`, `..._RR22.85.csv`.

Never: `data.csv`, `test2.csv`, `final_FINAL.csv`, spaces in filenames.

## One file = one trace/table

Keep each CSV a single coherent table. For sweeps, prefer many small files that `glob` cleanly over one mega-file with cryptic multi-index columns — the wafer-robustness example loads its whole dataset with:

```python
files = glob("./Data/Supmat/WaferRobustness/OSA/*.csv")
```

Next: [04 — Figures & notebook](04-FiguresAndNotebook.md)
