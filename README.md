# Institutionalization Without Integration: Bitcoin After the Spot ETF — Replication Package

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![DOI](https://img.shields.io/badge/Zenodo-10.5281%2Fzenodo.XXXXXXX-blue.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)

> Replace `XXXXXXX` with the real Zenodo DOI once the archived release is created
> (see Section 8, "Publishing and archiving"). The same DOI is cited in the paper's
> *Data and Code Availability* section.

This repository contains the data and code required to reproduce all empirical
results, tables, and figures in:

> **Institutionalization Without Integration: Bitcoin After the Spot ETF**
> Manuscript IREF-D-26-02471, *International Review of Economics and Finance*.

The study uses a frequency-dependent TVP-VAR connectedness framework to examine how
the U.S. spot Bitcoin ETF era (approval date: 10 January 2024) is associated with
changes in Bitcoin's connectedness with the S&P 500, Gold, and the VIX, and whether
Bitcoin functions as a safe asset **before and after** the ETF.

---

## 1. Repository structure

```
.
├── README.md                      This file
├── LICENSE.txt                    CC BY 4.0 license (data and code)
├── CITATION.cff                   Machine-readable citation metadata (GitHub)
├── .zenodo.json                   Archival metadata for the Zenodo deposit
├── .gitignore                     Excludes R/LaTeX/OS artifacts
├── data/
│   ├── market_data.csv            Raw daily market and on-chain data (2020-2025)
│   └── data_dictionary.md         Variable definitions, units, and sources
├── code/
│   └── TVP-VAR_ETF_Analysis.Rmd   Single, self-contained analysis script
└── results/
    ├── tables/                    Reference output tables (CSV) reported in the paper
    └── figures/                   Reference output figures (PNG) reported in the paper
```

The `results/` folder contains the exact output the script produces, provided so
that readers can verify the reported numbers without re-running the analysis.
Re-running the script regenerates these files.

---

## 2. Data

* **File:** `data/market_data.csv`
* **Coverage:** daily, 1 January 2020 – 31 December 2025 (2,192 rows). The analysis
  sample reported in the paper covers January 2020 – October 2025; later rows are
  retained in the raw file for completeness.
* **Core variables used in the paper:** `BTC` (Bitcoin price), `SPY` (SPDR S&P 500
  ETF, the equity proxy), `XAU/USD` (Gold spot price), and `VIX` (CBOE Volatility
  Index). Additional market and Bitcoin on-chain variables are included for
  transparency and robustness checks.
* **Missing values:** equity, gold, and volatility series are blank on days when
  those markets are closed (weekends and holidays); the script aligns the series to
  common trading days and computes log returns before estimation.
* See `data/data_dictionary.md` for the full variable list, units, and sources.

### Data sources

The daily series were collected from public market-data providers (Bitcoin, equity
indices, Treasury yields, gold, and the VIX) and from public Bitcoin on-chain
sources (hash difficulty, hashrate, active addresses, transactions) together with
the Crypto Fear & Greed Index. The redistributed file contains only derived daily
observations used as model inputs. Users who wish to extend the sample should
consult the original providers listed in `data_dictionary.md`.

---

## 3. Software environment

* **Language:** R (developed and tested under R 4.3+; R ≥ 4.2 recommended).
* **Report format:** R Markdown (`.Rmd`); knit with **RStudio** or
  `rmarkdown::render()`.

### Required R packages

The script installs any missing packages automatically from CRAN on first run.
The core packages are:

| Purpose                       | Packages |
|-------------------------------|----------|
| TVP-VAR & connectedness       | `ConnectednessApproach` |
| Econometrics / unit roots     | `vars`, `tseries`, `urca`, `lmtest`, `sandwich` |
| DCC-GARCH benchmark           | `rmgarch`, `rugarch` |
| Changepoint / regime detection| `changepoint`, `strucchange` |
| Data manipulation             | `dplyr`, `tidyr`, `lubridate`, `zoo`, `xts`, `tibble`, `reshape2`, `purrr` |
| Bootstrap & statistics        | `boot`, `moments` |
| Visualization                 | `ggplot2`, `patchwork`, `scales`, `viridis`, `ggtext`, `ggthemes`, `ggridges`, `ggrepel`, `corrplot`, `RColorBrewer` |
| Network plots                 | `igraph`, `ggraph`, `tidygraph` |
| Tables / export               | `gt`, `gtsummary`, `knitr`, `kableExtra`, `openxlsx`, `writexl` |
| Utilities                     | `rlang`, `glue`, `progress` |

The script prints the installed versions of the key packages
(`ConnectednessApproach`, `rmgarch`, `changepoint`, `ggplot2`) near the end of the
run. For an exact archival record of your environment, run `sessionInfo()` after
knitting and save the output alongside the results.

---

## 4. How to reproduce the results

1. Install R (and, recommended, RStudio).
2. Clone or download this repository:

   ```bash
   git clone https://github.com/<user>/bitcoin-etf-connectedness.git
   cd bitcoin-etf-connectedness
   ```

3. Open `code/TVP-VAR_ETF_Analysis.Rmd` and knit it (RStudio "Knit" button), or run
   from the `code/` folder:

   ```r
   rmarkdown::render("TVP-VAR_ETF_Analysis.Rmd")
   ```

   The script automatically reads the data from `../data/market_data.csv` (this
   repository's layout). If you instead place a copy of `market_data.csv` in the
   working directory, that copy is used.

4. The run installs any missing packages, performs the full analysis, and writes the
   tables (`.csv`) and figures (`.png`) to the working directory. These outputs
   correspond to the reference files in `results/tables/` and `results/figures/`.

**Notes**
* A full run estimates the TVP-VAR, the frequency decomposition, the DCC-GARCH
  benchmark, the changepoint/regime detection, and block-bootstrap procedures
  (10,000 replications); allow several minutes to tens of minutes depending on
  hardware.
* Bootstrap-based confidence intervals and tests use random resampling. The script
  sets a random seed where appropriate; minor numerical differences across platforms
  or package versions do not affect the substantive conclusions.

---

## 5. Mapping of outputs to the paper

The output file names are self-describing and map onto the paper as follows
(representative items):

| Output file | Used for |
|-------------|----------|
| `Table1_Descriptive_Statistics.csv` | Descriptive statistics |
| `Table2_ADF_Results.csv` | Stationarity (ADF) tests |
| `Table4_TCI_by_Regime.csv`, `Figure3_TCI_Dynamics.png` | Total connectedness dynamics |
| `Table5_NET_Statistics.csv`, `Figure4_NET_Analysis.png` | NET directional connectedness (H3) |
| `Table7_ETF_Hypothesis_Tests_Revised.csv`, `Table7c_Placebo_Tests.csv` | ETF effect & placebo tests (H1, H2) |
| `Table_SafeHaven_*.csv`, `Figure5_Threshold_SafeHaven.png` | Safe-haven / VIX-regime diagnostics (H4) |
| `Table11_Portfolio_Performance.csv`, `Table13_LedoitWolf_Tests.csv` | Supplementary portfolio evidence |
| `Table17_Kappa_Sensitivity_Revised.csv`, `Table18_Cutoff_Sensitivity.csv` | Robustness checks |
| `Figure9_Network.png` | Connectedness network structure |

Where two versions of a table exist (e.g., a base and a `_Revised`/`_FIXED`
variant), the revised/corrected version corresponds to the numbers reported in the
final manuscript.

---

## 6. License

All materials in this repository are released under the **Creative Commons
Attribution 4.0 International (CC BY 4.0)** license. You are free to share and adapt
the material for any purpose, provided you give appropriate credit by citing the
paper and this repository. See `LICENSE.txt`.

---

## 7. How to cite

If you use these data or code, please cite both the paper and the archived
repository:

> [Authors] (2026). *Institutionalization Without Integration: Bitcoin After the
> Spot ETF.* International Review of Economics and Finance (manuscript
> IREF-D-26-02471).

> [Authors] (2026). *Replication package for "Institutionalization Without
> Integration: Bitcoin After the Spot ETF"* (vX.Y) [Data set]. Zenodo.
> https://doi.org/10.5281/zenodo.XXXXXXX

A machine-readable citation is provided in `CITATION.cff`.

---

## 8. Publishing and archiving (maintainer notes)

These one-time steps create the public GitHub repository and the citable Zenodo
archive referenced by the paper. **Do not run `git init` inside a cloud-synced
folder (Google Drive / Dropbox / iCloud): a live `.git` directory can be corrupted
by background sync. Copy this folder to a local, non-synced location first.**

```bash
# 1. Copy out of the cloud-synced drive to a local path
cp -R "<this Replication_Package folder>" ~/bitcoin-etf-connectedness
cd ~/bitcoin-etf-connectedness

# 2. Initialize and commit
git init
git add .
git commit -m "Replication package for IREF-D-26-02471"

# 3. Authenticate (one-time) and create the GitHub repository, then push
gh auth login
gh repo create bitcoin-etf-connectedness --public --source=. --remote=origin --push
```

Then archive on Zenodo to obtain the citable DOI:

1. Sign in at https://zenodo.org with your GitHub account.
2. Under **Settings → GitHub**, toggle ON the `bitcoin-etf-connectedness` repository.
3. On GitHub, create a release (e.g., tag `v1.0`): *Releases → Draft a new release*.
4. Zenodo automatically archives that release and issues a DOI of the form
   `10.5281/zenodo.XXXXXXX`.
5. Replace the placeholder DOI in (i) this README's badge and citation, (ii)
   `CITATION.cff`, (iii) `.zenodo.json`, and (iv) the manuscript's *Data and Code
   Availability* section, then push the update.

> Tip: Zenodo also issues a permanent "concept DOI" that always resolves to the
> latest version; cite the version-specific DOI in the paper for exact
> reproducibility.
