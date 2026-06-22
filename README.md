# Institutionalization Without Integration: Bitcoin After the Spot ETF — Replication Package

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![DOI](https://img.shields.io/badge/Zenodo-10.5281%2Fzenodo.20582645-blue.svg)](https://doi.org/10.5281/zenodo.20582645)

This repository contains the **data and code** required to reproduce all empirical
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
├── CITATION.cff                   Machine-readable citation metadata
├── .zenodo.json                   Archival metadata for the Zenodo deposit
├── .gitignore                     Excludes R/OS artifacts and generated output
├── data/
│   ├── market_data.csv            Raw daily market and on-chain data (2020-2025)
│   └── data_dictionary.md         Variable definitions, units, and sources
└── code/
    └── TVP-VAR_ETF_Analysis.Rmd   Single, self-contained analysis script
```

The repository deliberately ships only the inputs needed to reproduce the study:
the raw data and the analysis script. Running the script regenerates every table
and figure reported in the paper; those generated files are not committed (they are
the paper's own exhibits and are listed in Section 5).

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
  common trading days and computes returns before estimation.
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
knitting and save the output with your run.

---

## 4. How to reproduce the results

1. Install R (and, recommended, RStudio).
2. Clone or download this repository:

   ```bash
   git clone https://github.com/Albatrosses-reply/bitcoin-etf-connectedness.git
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
   tables (`.csv`) and figures (`.png`) to the working directory.

**Notes**
* A full run estimates the TVP-VAR, the frequency decomposition, the DCC-GARCH
  benchmark, the changepoint/regime detection, and block-bootstrap procedures
  (10,000 replications); allow several minutes to tens of minutes depending on
  hardware.
* Bootstrap-based confidence intervals and tests use random resampling. The script
  sets a random seed where appropriate; minor numerical differences across platforms
  or package versions do not affect the substantive conclusions.

---

## 5. Outputs produced by the script

Running the script writes self-describing files to the working directory; the main
ones map onto the paper as follows:

| Generated file | Appears in the paper as |
|----------------|-------------------------|
| `Table1_Descriptive_Statistics.csv` | Descriptive statistics |
| `Table2_ADF_Results.csv` | Stationarity (ADF) tests |
| `Table4_TCI_by_Regime.csv`, `Figure3_TCI_Dynamics.png` | Total connectedness dynamics |
| `Table5_NET_Statistics.csv`, `Figure4_NET_Analysis.png` | NET directional connectedness (H3) |
| `Table7_ETF_Hypothesis_Tests_Revised.csv`, `Table7c_Placebo_Tests.csv` | ETF effect and placebo tests (H1, H2) |
| `Table_SafeHaven_*.csv`, `Figure5_Threshold_SafeHaven.png` | Safe-haven / VIX-regime diagnostics (H4) |
| `Table11_Portfolio_Performance.csv`, `Table13_LedoitWolf_Tests.csv` | Supplementary portfolio evidence |
| `Table17_Kappa_Sensitivity_Revised.csv`, `Table18_Cutoff_Sensitivity.csv` | Robustness checks |
| `Figure9_Network.png` | Connectedness network structure |

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

> @article{kang2026institutionalization,
> author  = {Kang, Hojun and Lee, Sang-Gun},
> title   = {Institutionalization without integration: {Bitcoin} after the spot {ETF}},
> journal = {International Review of Economics \& Finance},
> volume  = {110},
> pages   = {105531},
> year    = {2026},
> issn    = {1059-0560},
> doi     = {10.1016/j.iref.2026.105531},
> url     = {https://doi.org/10.1016/j.iref.2026.105531}}
> 
> Kang, H., & Lee, S.-G. (2026). Institutionalization without integration: Bitcoin after the spot ETF. International Review of Economics & Finance, 110, Article 105531. https://doi.org/10.1016/j.iref.2026.105531


A machine-readable citation is provided in `CITATION.cff`.

---

## 8. Updating the archived version (maintainer note)

The repository is archived on Zenodo, which issues a new DOI for each GitHub
release. To publish an updated version, draft a new release on GitHub (e.g. tag
`v1.1`); Zenodo archives it automatically under the same concept DOI. Update the DOI
in this README, `CITATION.cff`, and `.zenodo.json` if you cite a version-specific
DOI.
