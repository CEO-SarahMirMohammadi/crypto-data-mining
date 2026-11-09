# BTCUSDT Data Mining

## Project Overview

This project currently covers data understanding and preprocessing of BTCUSDT 15-minute candles. The raw CSV contains 229,993 rows with `datetime`, `open`, `high`, `low`, `close`, and `volume`. Its available data ends on 2024-03-14.

## Current Project Status

Preprocessing is completed. No machine learning or prediction has been implemented.

## Dataset Information

- Source: `data/raw/BTCUSDT_M15.csv`
- Timeframe: 15-minute candles
- Original columns: `datetime`, `open`, `high`, `low`, `close`, `volume`
- Original rows: 229,993
- Source coverage ends: 2024-03-14

## Data Preprocessing

The notebook interprets source timestamps as UTC, converts them to `America/New_York`, then filters local dates from 2020-01-01 inclusive to 2025-01-01 exclusive and the New York session from 09:30 inclusive to 16:00 exclusive.

It checks missing values, duplicate rows and timestamps, validates OHLCV rules, and detects missing or irregular 15-minute intervals. The raw file has no missing values or duplicates; all processed OHLCV checks passed. Four initial rows were removed because percentage-change and four-candle rolling features need prior observations. Feature warm-up NaNs were dropped rather than imputed.

Features use current and past observations only:

- `return`: percentage change in close.
- `candle_range`: high minus low.
- `candle_body`: absolute close-open difference.
- `volatility`: rolling standard deviation of returns over four candles.
- `SMA_4`: four-candle moving average of close.
- `volume_change`: percentage change in volume.
- `hour`: local New York hour.
- `day_of_week`: local weekday, Monday = 0.

## Output Files

The processed dataset is saved to `data/processed/BTCUSDT_NY_session_cleaned.csv`. It contains 39,876 rows and the six original columns plus the eight features above.

## Installation and Execution

From the project root in Windows PowerShell, activate the existing virtual environment and install its requirements:

```powershell
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
code .\notebooks\preprocessing.ipynb
```

In VS Code, select the `.venv` Python kernel if prompted, then choose **Run All**. The VS Code Jupyter extension is required.

## Limitations

Source coverage is incomplete for 2024 and ends on 2024-03-14; no later data is available in the source. The notebook reports 30 missing expected session candles. Missing candles were not artificially generated or filled.

## Project Structure As Far As Now - Might Change My Mind;)

```text
crypto-data-mining/
├── .venv/
├── data/
│   ├── raw/
│   │   ├── BTCUSDT_D1.csv
│   │   ├── BTCUSDT_H4.csv
│   │   ├── BTCUSDT_M15.csv
│   │   └── BTCUSDT_W1.csv
│   └── processed/
│       └── BTCUSDT_NY_session_cleaned.csv
├── notebooks/
│   ├── preprocessing.ipynb
│   └── tempCodeRunnerFile.ipynb
├── reports/
├── README.md
└── requirements.txt
```
