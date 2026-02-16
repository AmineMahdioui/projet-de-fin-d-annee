# Recession Probability Prediction

**Machine Learning Platform for Economic Cycle Prediction & Sector Rotation**

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://python.org)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange.svg)](https://tensorflow.org)

---

## Overview

This platform predicts economic recessions using 120+ macroeconomic indicators and provides sector rotation recommendations. The system combines CNN-LSTM deep learning with GARCH volatility modeling.

## Documentation

| Document | Description |
|----------|-------------|
| [Executive Overview](docs/EXECUTIVE_OVERVIEW.md) | High-level logic and methodology |
| [Developer Reference](docs/DEVELOPER_REFERENCE.md) | Technical implementation details |

---

## Quick Start

### Installation

```bash
pip install -r requirements.txt
```

### Environment Setup

```bash
export FRED_API_KEY="your_fred_api_key"
export QUANDL_API_KEY="your_quandl_api_key"
```

### Usage

```python
from tensorflow import keras

model = keras.models.load_model('Model/CNN2-LSTM')
probability = model.predict(input_data)
print(f"Recession Probability: {probability[0][0]*100:.1f}%")
```

---

## Repository Structure

```
├── Data/                           # Data assets
│   ├── Macro Variables.csv         # Variable definitions
│   ├── HistoricalVariables.csv     # Raw data
│   └── Transformed HistoricalVariables.csv
│
├── DataCollection/                 # Data pipeline
│   └── Collection&Transformation.ipynb
│
├── Model/
│   └── CNN2-LSTM/                  # Trained model
│
├── docs/                           # Documentation
│   ├── EXECUTIVE_OVERVIEW.md
│   └── DEVELOPER_REFERENCE.md
│
├── Notebooks/
│   ├── New try LSTM.ipynb          # Main model
│   ├── feature_selection.ipynb     # Feature importance
│   ├── garch.ipynb                 # Volatility modeling
│   └── SectorAnalysis.ipynb        # Sector rotation
│
└── Readme.md
```

---

## Key Features

- **Recession Probability Score**: 0-100% likelihood of recession within 3-6 months
- **Feature Importance Analysis**: Identifies leading economic indicators
- **Sector Rotation Strategy**: ETF allocation recommendations by economic cycle

---

## Model Performance

| Metric | Value |
|--------|-------|
| Test Accuracy | 88%+ |
| Lead Time | 3-6 months |
| Variables | 120+ macroeconomic indicators |

---

## License

This project is available for academic and research purposes. Contact the authors for commercial licensing inquiries.

---

## References

1. Stock, J.H. & Watson, M.W. (2003). "Forecasting Output and Inflation"
2. NBER Business Cycle Dating Committee
3. Federal Reserve Economic Data (FRED)
