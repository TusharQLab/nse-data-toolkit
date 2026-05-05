# NSE Data Toolkit 🛠️

Convert free NSE stock market data to Parquet format — clean, validated, and ready for backtesting, analysis, or ML.

> **Data comes from** 👉 [market-data repo](https://github.com/TusharQLab/market-data)
> **This repo gives you** the Colab notebook to convert + validate that data in minutes.

---

## How it works
Download CSV from market-data repo  →  Drag into Colab  →  Run notebook  →  Download .parquet

---

## Step 1 — Get your data

Go to the **market-data repo** and download your CSV:

### Option A — Artifacts zip (recommended, gets everything at once)
1. Go to [market-data repo](https://github.com/TusharQLab/market-data) → **Actions** tab
2. Click the latest run with a green tick ✅
3. Scroll to bottom → click **market-data-XXX**
4. Zip downloads with all CSV files inside

> ⚠️ Artifacts expire after 30 days. Use Option B for older data.

### Option B — Download individual files
1. Go to [market-data repo](https://github.com/TusharQLab/market-data) → `data/` folder
2. Choose your timeframe: `1m/` or `5m/`
3. Click any CSV file → click **"Download raw file"** (top right)

---

## What data is available

| Timeframe | File structure | Example file |
|-----------|---------------|--------------|
| 1-minute  | One file per month | `master_1m_2026_04.csv` |
| 5-minute  | One file per year  | `master_5m_2026.csv` |
| 1-hour    | Single file always | `master_1h.csv` |

### Data columns (all timeframes)

| Column | Example | Description |
|--------|---------|-------------|
| `datetime` | 2026-03-28 09:15:00 | Candle open time (IST) |
| `ticker` | RELIANCE | NSE stock symbol |
| `open` | 1423.50 | Opening price |
| `high` | 1431.20 | Highest price |
| `low` | 1419.80 | Lowest price |
| `close` | 1428.75 | Closing price |
| `volume` | 284500 | Shares traded |

---

## Step 2 — Convert + Validate in Colab

**Open `Parquet_converter.ipynb`** from the notebooks folder above.

### What the notebook does in one run:
- ✅ Inspects your CSV — row count, columns, file size
- ✅ Reads in chunks — no RAM crash even on 100MB+ files
- ✅ Auto-optimizes data types — saves 60–70% disk space
- ✅ Validates row count before and after — zero data loss guaranteed
- ✅ Compresses with Snappy — loads in seconds every time
- ✅ Downloads the `.parquet` file directly to your PC

### How to run:
1. Open [Google Colab](https://colab.research.google.com) → **File → Upload notebook** → select `Parquet_converter.ipynb`
2. Drag and drop your downloaded CSV into the Colab files panel (left sidebar)
3. Update the filename at the top of the notebook if needed
```python
   CSV_FILE = "master_1m_2026_04.csv"   # ← change to your filename
```
4. **Runtime → Run all** → parquet file downloads to your PC automatically

---

## Step 3 — Reload anytime for backtesting

Save your `.parquet` file on your PC. Next time just drag it back into Colab:

```python
import pyarrow.parquet as pq

# Load full data
df = pq.read_table("1m_2026_04.parquet").to_pandas()

# Load specific columns only (faster)
df = pq.read_table("1m_2026_04.parquet",
                   columns=["datetime", "ticker", "close", "volume"]).to_pandas()
```

No re-converting. No waiting. Ready in ~2 seconds.

---

## Why Parquet over CSV?

| | CSV | Parquet |
|--|-----|---------|
| File size | 97 MB | ~20 MB |
| Load time | ~30 sec | ~2 sec |
| Query speed | Slow | Fast |
| Column filtering | Read everything | Read only what you need |
| Ready for analysis | ❌ | ✅ |

---

## Repo structure
nse-data-toolkit/
├── README.md
└── notebooks/
└── Parquet_converter.ipynb    ← convert + validate in one run

---

## Related repos

| Repo | What it does |
|------|-------------|
| [market-data](https://github.com/TusharQLab/market-data) | Auto-collects NSE data daily via GitHub Actions + yfinance |
| nse-data-toolkit ← you are here | Converts + validates that data for use in Python |

---

## Questions or issues?

Open an issue or connect on [LinkedIn](https://www.linkedin.com/in/tusharqlab-~-2a96b1253/)
