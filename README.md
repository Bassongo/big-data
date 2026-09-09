# Meteorological Impact on European Energy Consumption

[![PySpark](https://img.shields.io/badge/PySpark-3.5.1-orange.svg)](https://spark.apache.org/)
[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)

> Analyzing 590K+ observations across 32 European countries to quantify how weather extremes affect electricity consumption and renewable energy production.

## Overview

This project uses **Apache Spark (PySpark)** to process large-scale meteorological and energy datasets, revealing statistically significant relationships between weather conditions and energy demand across Europe.

**Key finding**: Heatwaves (>25°C) increase average electricity consumption by **+16.8%**, while extreme cold (<0°C) shows a **-12.3%** decrease at the European level — with significant country-level variation (e.g., Germany shows +8.4% during cold spells due to electric heating).

## Key Results

### Weather-Energy Correlations

| Relationship | Pearson r | Interpretation |
|---|---|---|
| Temperature × Consumption | 0.007 (global) | Season-dependent |
| Solar radiation × Solar production | 0.231 | Moderate positive |
| Wind speed × Wind production | 0.262 | Moderate positive |

### Impact of Extreme Temperatures (ANOVA p = 1.48e-21)

| Period | Avg Consumption | Δ vs Normal |
|---|---|---|
| Normal (0–25°C) | 10,955 MW | — |
| Heatwave (>25°C) | 12,794 MW | **+16.8%** |
| Extreme cold (<0°C) | 9,610 MW | **-12.3%** |

All pairwise differences are statistically significant (p < 0.001, t-test).

## Architecture

```
Data Sources (OPSD + Open-Meteo API)
  → Ingestion & Chunking
    → PySpark Transformation (unpivot, aggregation)
      → Feature Engineering (join weather + energy)
        → Statistical Analysis (ANOVA, t-tests, correlations)
          → Visualization (Matplotlib, Seaborn)
```

## Dataset

- **Energy data**: [Open Power System Data (OPSD)](https://data.open-power-system-data.org/) — hourly electricity consumption & renewable production
- **Weather data**: [Open-Meteo Historical API](https://open-meteo.com/) — temperature, wind speed, solar radiation
- **Coverage**: 32 European countries, Jan 2015 – Jun 2020, 590K observations

## Tech Stack

- **Big Data**: Apache Spark 3.5.1, PySpark
- **Analysis**: pandas, NumPy, SciPy (ANOVA, t-tests)
- **Visualization**: Matplotlib, Seaborn
- **Environment**: Google Colab / Jupyter

## Quick Start

```bash
git clone https://github.com/Bassongo/spark-energy-weather-analysis.git
cd spark-energy-weather-analysis
pip install -r requirements.txt
jupyter notebook notebooks/energy_weather_analysis.ipynb
```

## Project Structure

```
├── notebooks/
│   └── energy_weather_analysis.ipynb   # Main analysis notebook
├── requirements.txt
├── LICENSE
└── README.md
```

## Limitations & Future Work

- Correlations ≠ causation; confounding variables (holidays, events) not controlled
- Daily aggregation masks intra-day peaks
- **Next steps**: ML-based demand forecasting, extended time range (2010–2024), per-country deep dives

## Team

Academic project — Big Data & Cloud Computing (BDCC 2025)

| Name | Role |
|---|---|
| Mouhammadou Dia | Statistical Analyst |
| Kouami Emmanuel Dossekou | Statistical Analyst |
| **Marc Mare** | Statistical Analyst |
| Ndeye Salla Toure | Statistical Analyst |

## Author

**Marc Mare** — [GitHub](https://github.com/Bassongo)
ENSAE Dakar | MSc SEP, University of Reims (2026)

## License

MIT License, see [LICENSE](LICENSE) for details.
