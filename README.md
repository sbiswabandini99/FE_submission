# Replication package: Explosive Prices in the Korean AI Rally

ECOM90011 Financial Econometrics Research Project, Sukanya Biswabandini.

## How to run

1. Install R (4.5.1 was used) and RStudio.
2. Open `AI_Bubble_Replication.Rmd` in RStudio. Keep it in this folder, because all paths are relative to it.
3. Click **Knit**. Missing packages are installed automatically.
4. Tables are written to `output/tables/` and figures to `output/figures/`.

Run time is about 15 to 30 minutes, almost all of it the size and power simulation. All critical values are cached in `data/clean/` and load instantly. If the cache is deleted, the Monte Carlo critical values take about 1 hour and the wild bootstrap critical values several hours.

## Folder structure

```
AI_Bubble_Replication.Rmd   code
README.md                   this file
data/raw/                   frozen input data
data/clean/                 cached critical values (.rds)
output/tables/              tables (created by the code)
output/figures/             figures (created by the code)
```

## Data

| File | Source | Period |
|---|---|---|
| `krx_KOSPI_*.csv`, `krx_Samsung_*.csv`, `krx_SK_Hynix_*.csv`, `krx_Banks_*.csv` | Korea Exchange market data service: close, dividend yield, dividend per share | 4 Jan 2021 to 8 Oct 2026, three files per series |
| `Nasdaq.csv` | Yahoo Finance, ^IXIC | 2 Jan 1990 to 31 Dec 2002 |
| `Dow_Jones.csv` | Yahoo Finance, ^DJI | 2 Jan 1992 to 31 Dec 2002 |
| `KOSPI.csv`, `Samsung.csv`, `SK_Hynix.csv` | Yahoo Finance, ^KS11, 005930.KS, 000660.KS | 2021 to 9 Oct 2026 (source check and start-date check) |
| `*_from2019.csv` | Yahoo Finance, same tickers | 2019 to 9 Oct 2026 (start-date check) |

Raw files are frozen. Yahoo is contacted only if a Yahoo file is missing.

## Settings

- Right-tailed ADF regressions with no lags.
- Minimum window floor((0.01 + 1.8/sqrt(T))T): 81 days for Korea, 135 for the Nasdaq, 122 for the Dow.
- Monte Carlo critical values: 2,000 replications, seed 90011.
- Wild bootstrap critical values: 500 replications, seed 90011.
- Episodes are dated only if GSADF rejects, and must last at least round(log T) days.

## Code sections and report

| Code section | Report | Output |
|---|---|---|
| Size and power | Appendix B | `v2_size_power.csv` |
| Yahoo series | Section 3, Section 5 | `stage1_gsadf_yahoo.csv` |
| H1 | Section 4.1, Figure 1 | `h1_main_gsadf.csv`, `h1_main_episodes.csv`, `h1_main_*.png` |
| Wild bootstrap, Korea | Section 4.2, Appendix C | `robust_wild_bootstrap_krx.csv`, `robust_wb_episodes_krx.csv` |
| Wild bootstrap, dot-com | Section 5 | `robust_wild_bootstrap_dotcom.csv` |
| Start dates | Appendix C | `robust_start_dates.csv` |
| H2 | Section 5, Figure 2 | `h2_episode_metrics_krx.csv`, `h2_headline_krx.csv`, `h2_event_overlay_krx.png` |
| H3 | Appendix D | `h3_regimes_krx.csv`, `stage2_gjr_by_regime_krx.csv`, `stage2_h3_compare_krx.csv` |
| R squared | Section 1 | `r_squared_chip_stocks.csv` |

## Key results to check

| Result | Value |
|---|---|
| GSADF, log price: KOSPI, Samsung, SK Hynix, KRX Banks | 3.980, 3.962, 4.242, 3.259 |
| GSADF, log price-dividend ratio: same order | 3.308, 3.490, 2.315, 3.184 |
| 95% Monte Carlo critical value: Korea, Nasdaq, Dow | 2.350, 2.441, 2.428 |
| 95% wild bootstrap critical value, log price: KOSPI, Nasdaq | 4.662, 4.581 |
| Size and power at n = 1,410 | 3.0%, 92% |
| R squared, KOSPI and KRX Banks on the two chip stocks | 0.812, 0.102 |

## Packages

R 4.5.1 on macOS. exuber 1.1.0, rugarch 1.5-4, quantmod 0.4.28, xts 0.14.1, zoo 1.8-14, dplyr 1.2.1, tibble 3.3.1, readr 2.2.0. The full list is in `output/session_info.txt` after knitting.
