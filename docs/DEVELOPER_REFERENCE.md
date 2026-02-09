# Recession Probability Module - Developer Reference

## Overview

Technical documentation for implementing and maintaining the recession probability prediction system.

## Input Parameters

### Data Sources

| Source | Purpose | Configuration |
|--------|---------|---------------|
| FRED API | Macroeconomic indicators | Requires `FRED_API_KEY` environment variable |
| Quandl | S&P metrics, yield data | Requires `QUANDL_API_KEY` environment variable |
| Yahoo Finance | Market data, sector ETFs | No authentication required |

### Environment Variables

```bash
# Required API credentials
export FRED_API_KEY="your_fred_api_key"
export QUANDL_API_KEY="your_quandl_api_key"
```

**Note**: API keys should be stored as environment variables or in a secure configuration file. Never commit credentials to version control.

### Macroeconomic Variables

The model uses 120+ variables defined in `Data/Macro Variables.csv`. Key categories:

| Category | Variables | Examples |
|----------|-----------|----------|
| Housing | 12 | PERMIT, HOUST, HOUSTNE, HOUSTS, HOUSTW |
| Employment | 15 | PAYEMS, UNRATE, ICSA, CE16OV |
| Financial | 20 | FEDFUNDS, TB3MS, GS10, BAA, AAA |
| Production | 18 | INDPRO, IPMAN, IPCONGD |
| Money Supply | 8 | M1SL, M2SL, AMBSL, TOTRESNS |
| Prices | 15 | CPIAUCSL, PCEPI, PPICMM |
| Consumer | 10 | UMCSENT, RSAFS, DSPIC96 |

### Data Transformations

Variables are transformed according to the `Transformation` column in `Macro Variables.csv`:

| Code | Transformation | Formula |
|------|----------------|---------|
| 1 | No transformation | x |
| 2 | First difference | Δx |
| 3 | Second difference | Δ²x |
| 4 | Log | ln(x) |
| 5 | Log first difference | Δln(x) |
| 6 | Log second difference | Δ²ln(x) |

## Model Parameters

### CNN-LSTM Architecture

```python
# Model configuration
SEQUENCE_LENGTH = 12        # 12-month lookback window
FEATURES = 120              # Number of input variables
CNN_FILTERS = 64            # Convolutional filters
LSTM_UNITS = 50             # LSTM hidden units
DROPOUT_RATE = 0.2          # Dropout for regularization
```

### Training Configuration

```python
EPOCHS = 28                 # Training epochs (see epoch.txt)
BATCH_SIZE = 32             # Mini-batch size
LEARNING_RATE = 0.001       # Adam optimizer learning rate
VALIDATION_SPLIT = 0.2      # Train/validation split
```

### Output Format

```python
# Model output
probability: float  # Range [0.0, 1.0]
# Convert to percentage: probability * 100
```

## File Structure

```
├── Data/
│   ├── Macro Variables.csv          # Variable definitions
│   ├── HistoricalVariables.csv      # Raw data
│   └── Transformed HistoricalVariables.csv  # Processed data
│
├── Model/
│   └── CNN2-LSTM/                   # Saved model
│       ├── saved_model.pb
│       ├── keras_metadata.pb
│       └── variables/
│
├── DataCollection/
│   ├── Collection&Transformation.ipynb  # Data pipeline
│   └── MarketCap.ipynb
│
└── Notebooks/
    ├── New try LSTM.ipynb           # Main model
    ├── feature_selection.ipynb      # Feature importance
    ├── garch.ipynb                  # Volatility modeling
    └── SectorAnalysis.ipynb         # Sector rotation
```

## Usage

### Loading the Model

```python
from tensorflow import keras
import pandas as pd

# Load pre-trained model
model = keras.models.load_model('Model/CNN2-LSTM')

# Prepare input data (shape: [1, 12, 120])
# 1 sample, 12 months, 120 features
input_data = prepare_features(latest_data)

# Get prediction
probability = model.predict(input_data)
print(f"Recession Probability: {probability[0][0]*100:.1f}%")
```

### Data Preparation

```python
import os
from fredapi import Fred

# Initialize API client
fred = Fred(api_key=os.environ.get('FRED_API_KEY'))

# Fetch indicator
data = fred.get_series('PERMIT')
```

## Technical Constraints

| Constraint | Value | Notes |
|------------|-------|-------|
| Minimum data points | 12 months | Required for sequence input |
| Update frequency | Monthly | Aligns with FRED release schedule |
| Memory requirement | ~2GB | For model inference |
| Python version | 3.8+ | TensorFlow compatibility |

## Dependencies

See `requirements.txt` for full list. Core dependencies:

```
tensorflow>=2.10.0
pandas>=1.5.0
numpy>=1.23.0
fredapi>=0.5.0
quandl>=3.7.0
yfinance>=0.2.0
arch>=5.3.0
scikit-learn>=1.2.0
```

---

*For high-level overview, see [Executive Overview](EXECUTIVE_OVERVIEW.md).*
