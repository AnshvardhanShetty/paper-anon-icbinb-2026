# Companion code

Companion code and analysis artefacts for the ICBINB-BIO @ NeurIPS 2026 paper
*"When Healthy EMG Pretraining Cannot Reach Stroke Intent: Pathology-Matched
Cross-Patient Transfer at 2{,}600× Less Data."*

This repository is anonymised for double-blind review.

---

## What is in this repo

Everything needed to reproduce every number, figure, and table in the paper.

```
paper-anon/
├── paper/                    LaTeX source, bib, figures, and the frozen
│                             writing artefacts (FINAL_NUMBERS.md, APPENDIX.md,
│                             PREREGISTRATION.md).
├── ml/                       Feature engineering + classifier training.
├── analysis/
│   ├── seed.py               Global RNG seed used throughout.
│   ├── revision/             Every leakage-free re-run and all CSV/parquet
│   │                         outputs cited by the main text and appendix.
│   ├── plots/                Figure-generation scripts (F1, F2, F3).
│   ├── lucchetti/            External replication (Lucchetti n=10 cohort).
│   ├── emgbench/             Public dataset adapters (UCI-EMG, CapgMyo).
│   ├── physiomio/            PhysioMio evaluation, calibration sweeps.
│   └── system/               BOM, latency benchmark, cost itemisation
│                             (deployment / system-characterisation claims).
├── firmware/
│   └── teensy_emg.ino        Deployed Teensy firmware for the peak-to-peak
│                             envelope pipeline (Appendix B).
├── data/
│   ├── physiomio_channel_picks.csv    Per-patient 4-channel selection
│   │                                  (Cohen's-d picks on healthy sessions).
│   └── DATASET_INSTRUCTIONS.md        Where to download the three public
│                                      datasets (PhysioMio, GrabMyo, Lucchetti).
└── requirements.txt          Python dependencies.
```

**Not in this repo:** raw dataset files (they are public and re-hosted at the
original sources, see `data/DATASET_INSTRUCTIONS.md`), trained model
artefacts (regenerable from the scripts here), the shipped web-app deployment
(a separate line of work not covered by the paper).

---

## Reproducing the paper's numbers

### 1. Set up the environment

Python 3.10 or newer, then:

```bash
pip install -r requirements.txt
```

### 2. Point at the datasets

Download the three public datasets per `data/DATASET_INSTRUCTIONS.md`, then
either place them under `./data/{physiomio,grabmyo,lucchetti}/` (the paths
the scripts assume by default) or export environment variables:

```bash
export PHYSIOMIO_ROOT=/path/to/your/physiomio
export GRABMYO_ROOT=/path/to/your/grabmyo
export LUCCHETTI_ROOT=/path/to/your/lucchetti
```

### 3. Rerun the analyses

The full end-to-end pipeline lives in `analysis/revision/`. To reproduce every
leakage-free number that appears in the main text and appendix:

```bash
bash analysis/revision/run_all_sequential.sh
```

Each recompute writes a per-patient CSV to `analysis/revision/results/`. The
frozen (calibration, test) split for every patient lives in
`analysis/revision/frozen_splits.parquet` and is loaded, never regenerated.

### 4. Regenerate the figures

```bash
python3 analysis/plots/fig1_system_and_placement.py
python3 analysis/revision/pathology_dominates_figure.py
python3 analysis/revision/plot_dose_response.py
```

Output PNGs land in `analysis/revision/results/` and `analysis/plots/figures/`;
copy the final ones into `paper/figures/` before recompiling the paper.

### 5. Compile the paper

```bash
cd paper
pdflatex icbinb_paper.tex
bibtex icbinb_paper
pdflatex icbinb_paper.tex
pdflatex icbinb_paper.tex
```

Requires the workshop's official `neurips_2026.sty` (included in `paper/`)
plus a standard TeX Live distribution with `natbib`, `booktabs`, `hyperref`,
`nicefrac`, and `microtype`.

---

## Provenance map

Every table in the paper is regenerable from a single source file, listed in
Appendix G (`Provenance summary`). If a claim in the paper is not traceable
via that table, it is a bug, please flag it.

---

## Pre-registration

Every decision rule was recorded before result-reading began. The commit is
dated 2026-08-21, eight days before the analyses were re-read against those
rules. The full pre-registration document is reproduced verbatim as
Appendix G, and also lives at `paper/PREREGISTRATION.md`.

---

## Anonymity

This repository is a fresh export with no git history and no author-identifying
content. If you find any residual leak (an author name, an institutional URL,
a personal email address, an identifying dataset trace), please treat it as a
bug and flag it to the review chairs so it can be scrubbed before camera-ready.
