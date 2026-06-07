# Data Dictionary — `market_data.csv`

Daily observations, 1 January 2020 – 31 December 2025 (2,192 rows; first row is the
header). One row per calendar day. Cells are blank when the corresponding market
was closed (weekends/holidays) or a value was unavailable. The analysis script
aligns the series to common trading days and works with log returns.

The four **core** variables used in the paper are `BTC`, `SPY`, `XAU/USD`, and
`VIX`. The remaining variables are provided for transparency and robustness and are
not part of the four-asset baseline system.

| # | Column | Description | Unit | Role | Typical source |
|---|--------|-------------|------|------|----------------|
| 1 | `Date` | Calendar date (ISO `YYYY-MM-DD`) | date | key | — |
| 2 | `BTC` | Bitcoin price (USD) | USD | **core** | Crypto market data provider |
| 3 | `SPY` | SPDR S&P 500 ETF price; equity-market proxy | USD | **core** | Equity market data provider |
| 4 | `DJI` | Dow Jones Industrial Average index level | index pts | auxiliary | Equity market data provider |
| 5 | `IXIC` | NASDAQ Composite index level | index pts | auxiliary | Equity market data provider |
| 6 | `NYSE` | NYSE Composite index level | index pts | auxiliary | Equity market data provider |
| 7 | `TNX` | CBOE 10-year U.S. Treasury note yield | percent | auxiliary | Market data provider |
| 8 | `XAU/USD` | Gold spot price per troy ounce (read into R as `XAU.USD`) | USD/oz | **core** | Commodity/FX data provider |
| 9 | `VIX` | CBOE Volatility Index | index pts | **core** | CBOE / market data provider |
| 10 | `IYR` | iShares U.S. Real Estate ETF price | USD | auxiliary | Equity market data provider |
| 11 | `VNQ` | Vanguard Real Estate ETF price | USD | auxiliary | Equity market data provider |
| 12 | `HashDiff` | Bitcoin network mining difficulty | difficulty units | auxiliary (on-chain) | Public blockchain explorer |
| 13 | `Hashrate` | Bitcoin network hashrate | hashes per second | auxiliary (on-chain) | Public blockchain explorer |
| 14 | `Address` | Active Bitcoin addresses | count/day | auxiliary (on-chain) | Public blockchain explorer |
| 15 | `Transaction` | Bitcoin transactions | count/day | auxiliary (on-chain) | Public blockchain explorer |
| 16 | `GreedIndex` | Crypto Fear & Greed Index (0 = extreme fear, 100 = extreme greed) | index 0–100 | auxiliary (sentiment) | Crypto Fear & Greed Index |

## Notes

* **Header peculiarity.** The column header `XAU/USD` contains a slash; `read.csv()`
  in R imports it as `XAU.USD`. The analysis script refers to it accordingly.
* **Encoding.** The file is UTF-8 and begins with a byte-order mark (BOM); standard
  `read.csv()` handles this transparently.
* **Returns.** The script converts price levels to daily log returns and uses the
  return series (not levels) as inputs to the TVP-VAR and DCC-GARCH models. `TNX`
  and the on-chain/sentiment variables are not part of the baseline four-asset
  system.
* **Sample window.** The paper reports results for January 2020 – October 2025. Rows
  after October 2025 are retained in the raw file but excluded from the reported
  estimation window by the script.
* **Provider terms.** Some original providers restrict redistribution of raw feeds;
  this file contains only the derived daily observations needed to reproduce the
  study. Users extending the sample should obtain fresh data directly from the
  providers indicated above.
