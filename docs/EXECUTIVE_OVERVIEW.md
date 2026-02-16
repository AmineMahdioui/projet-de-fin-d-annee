# Recession Probability Module - Executive Overview

## Purpose

The Recession Probability Module calculates the likelihood of an economic recession using machine learning models trained on historical macroeconomic data. It provides early warning signals to support investment decision-making.

## What It Calculates

The module generates a **Recession Probability Score** ranging from 0% to 100%, indicating the likelihood that the U.S. economy will enter a recession within the next 3-6 months.

| Score Range | Signal Level | Interpretation |
|-------------|--------------|----------------|
| 0-25% | Low Risk | Economic expansion likely to continue |
| 25-50% | Elevated | Monitor conditions closely |
| 50-75% | High Risk | Defensive positioning recommended |
| 75-100% | Critical | Recession imminent or in progress |

## Why It Works

### Data Foundation
- **120+ macroeconomic indicators** from Federal Reserve Economic Data (FRED)
- Historical coverage from 1959 to present
- Monthly frequency with daily updates where available

### Key Leading Indicators
The model identifies housing and credit market signals as the strongest predictors:

1. Building Permits (PERMIT)
2. Housing Starts by Region (HOUSTS, HOUSTW)
3. Non-Borrowed Reserves (NONBORRES)
4. Credit Spreads (BAAFFM, T1YFFM)
5. Money Supply (M1SL, M2SL)

### Model Architecture
- **CNN-LSTM Hybrid**: Combines convolutional layers for pattern recognition with LSTM layers for temporal dependencies
- **GARCH Component**: Captures volatility clustering in financial variables

## Output Applications

### Recession Probability Timeline
```
Date        | Probability | Signal
------------|-------------|--------
2024-01-15  | 12.3%       | Low Risk
2024-02-15  | 18.7%       | Low Risk
2024-03-15  | 35.2%       | Elevated
2024-04-15  | 62.8%       | High Risk
```

### Sector Rotation Recommendations
Based on the probability score and economic cycle position:

| Economic Phase | Recommended Sectors | Sectors to Avoid |
|----------------|---------------------|------------------|
| Expansion | XLY, XLK, XLI | XLU, XLP |
| Peak | XLE, XLB | XLF, XLRE |
| Contraction | XLU, XLP, XLV | XLY, XLF |
| Recovery | XLF, XLI, XLY | XLU |

## Performance Summary

| Metric | Value |
|--------|-------|
| Historical Accuracy | 88%+ on test data |
| Lead Time | 3-6 months before NBER announcements |
| Coverage | All U.S. recessions since 1960 |

---

*For implementation details, see [Developer Reference](DEVELOPER_REFERENCE.md).*
