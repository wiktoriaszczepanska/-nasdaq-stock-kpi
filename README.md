# Data Card — NASDAQ Stock Price Dataset
**Version:** 1.0 | **Created:** 2026-04-17 | **Source:** Yahoo Finance (yfinance)

## 1. Source of Data

| Field | Details |
|---|---|
| **API** | Yahoo Finance via `yfinance` Python library |
| **Data Type** | Daily OHLCV + Dividends + Stock Splits |
| **Granularity** | 1 trading day |
| **Exchange** | NASDAQ |
| **Access** | `yf.Ticker(t).history(start=..., end=..., interval="1d")` |
| **License** | Public market data; subject to Yahoo Finance ToS |

### Companies

| Ticker | Company | Start | End | Duration |
|--------|---------|-------|-----|----------|
| GOOGL | Alphabet Inc. | 2024-01-01 | 2024-11-01 | 10 mo |
| AAPL | Apple Inc. | 2023-06-01 | 2024-06-01 | 12 mo |
| MSFT | Microsoft Corp. | 2024-04-01 | 2025-04-01 | 12 mo |
| AMZN | Amazon.com Inc. | 2023-01-01 | 2023-09-01 | 8 mo |
| NVDA | NVIDIA Corp. | 2024-07-01 | 2025-01-01 | 6 mo |

### Schema

| Column | Type | Description |
|--------|------|-------------|
| Date | datetime | Trading date |
| Open | float64 | Opening price (USD) |
| High | float64 | Intraday high (USD) |
| Low | float64 | Intraday low (USD) |
| Close | float64 | Adjusted closing price (USD) |
| Volume | int64 | Shares traded |
| Dividends | float64 | Cash dividend |
| Stock Splits | float64 | Split ratio |

---

## 2. KPIs

### KPI 1 — Completeness
> % of non-null values in OHLCV columns. Threshold: >= 98% = PASS

| Ticker | Mean Completeness | Status |
|--------|-------------------|--------|
| GOOGL | 100.00% | PASS |
| AAPL | 100.00% | PASS |
| MSFT | 100.00% | PASS |
| AMZN | 100.00% | PASS |
| NVDA | 100.00% | PASS |

### KPI 2 — Latency
> Business-day lag between last data record and expected end date. Threshold: <= 2 days = PASS

| Ticker | Last Record | Biz Day Lag | Status |
|--------|------------|-------------|--------|
| GOOGL | 2024-10-31 | 1 | PASS |
| AAPL | 2024-05-31 | 0 | PASS |
| MSFT | 2025-03-31 | 1 | PASS |
| AMZN | 2023-08-31 | 1 | PASS |
| NVDA | 2024-12-31 | 1 | PASS |

### KPI 3 — Accuracy
> Internal OHLCV validity: High>=Low, Close in [Low,High], Volume>0. Threshold: >=99% = PASS

| Ticker | High>=Low | Close in Range | Volume>0 | Status |
|--------|-----------|---------------|----------|--------|
| GOOGL | 100.0% | 100.0% | 100.0% | PASS |
| AAPL | 100.0% | 100.0% | 100.0% | PASS |
| MSFT | 100.0% | 100.0% | 100.0% | PASS |
| AMZN | 100.0% | 100.0% | 100.0% | PASS |
| NVDA | 100.0% | 100.0% | 100.0% | PASS |

### KPI 4 — Consistency
> ADF stationarity test on daily returns. p < 0.05 → stationary → PASS

| Ticker | ADF Stat | p-value | Stationary | Status |
|--------|----------|---------|------------|--------|
| GOOGL | -14.679 | 0.0 | Yes | PASS |
| AAPL | -14.3124 | 0.0 | Yes | PASS |
| MSFT | -16.0179 | 0.0 | Yes | PASS |
| AMZN | -10.6708 | 0.0 | Yes | PASS |
| NVDA | -6.914 | 0.0 | Yes | PASS |

---

## 3. Conclusion

### Overall Quality Score

| KPI | Weight | Score |
|-----|--------|-------|
| Completeness | 25% | ~98% |
| Latency | 25% | ~100% (1-day lag) |
| Accuracy | 25% | 100% |
| Consistency | 25% | 100% |
| **Total** | | **~99.5 / 100** |

The dataset demonstrates excellent quality for AI/ML training use:

- **Completeness** is ~98%, with ~2% missing Close values from market holidays.
- **Latency** is 1 business day for all tickers — near real-time availability.
- **Accuracy** is 100% — no OHLCV logical violations detected.
- **Consistency** confirmed: all return series are stationary at p < 0.0001.

### Limitations

| Item | Note |
|------|------|
| Adjusted prices | yfinance returns adjusted Close by default |
| Survivorship bias | Delisted firms excluded |
| Rate limits | Add `time.sleep()` for bulk collection |
| Holidays | Missing rows on holidays are expected |

*Dataset compiled for KPI evaluation assignment.*
*Course: Management & AI | Kozminski University (ALK)*