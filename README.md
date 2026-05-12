# Rossmann Sales Forecasting

![Python](https://img.shields.io/badge/Python-3776AB.svg?style=for-the-badge&logo=Python&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![Prophet](https://img.shields.io/badge/Prophet-0072C6.svg?style=for-the-badge&logo=meta&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626.svg?style=for-the-badge&logo=Jupyter&logoColor=white)
![GitHub](https://img.shields.io/badge/github-%23121011.svg?style=for-the-badge&logo=github&logoColor=white)

A time-series sales forecasting project for Rossmann drugstore chain using historical sales data, store attributes, and holiday information. Built and run entirely locally using Python and Facebook Prophet.

---

## Project Overview

Rossmann operates over 1,000 stores across Europe. Accurate sales forecasting helps the business optimise inventory, plan promotions, and allocate resources efficiently.

This project explores 1,017,209 historical sales records across 1,115 stores (Jan 2013 – Jul 2015), performs exploratory data analysis to identify key sales drivers, and applies Facebook Prophet to forecast future sales per store.

---

## Dataset

| File | Description | Size |
|------|-------------|------|
| `data.csv` | Historical daily sales per store | 1,017,209 rows × 9 cols |
| `store.csv` | Store metadata (type, assortment, competition) | 1,115 rows × 10 cols |

> Data files are not included in this repo. Download from [Kaggle — Rossmann Store Sales](https://www.kaggle.com/competitions/rossmann-store-sales/data) and place in `src/data/`.

**Key columns:**
- `Sales` — daily revenue per store (target variable)
- `Customers` — daily foot traffic
- `Promo` — whether a short-term promotion was running
- `StateHoliday` — public holiday type (a=public, b=Easter, c=Christmas)
- `StoreType` — store format (a/b/c/d)
- `CompetitionDistance` — distance to nearest competitor (metres)

---

## Key Findings

### What drives sales?

| Feature | Correlation with Sales |
|---------|----------------------|
| DayOfWeek | -0.179 |
| Promo2 (loyalty scheme) | -0.128 |
| CompetitionDistance | -0.036 |
| SchoolHoliday | +0.039 |
| Promo (short-term) | **+0.368** |
| Customers | **+0.803** |

- **Foot traffic (Customers) is the strongest predictor** — more people in store = higher sales (corr 0.80)
- **Short-term promotions (Promo)** are the most effective controllable lever (+0.37)
- **Promo2** (the ongoing loyalty promotion) is *negatively* correlated — it appears to attract lower-spend customers or cannibalise regular sales
- **Day of week** has a clear effect — sales peak mid-week and drop on Mondays

### Open vs Closed
Of 1,017,209 total records, **172,817 (17%) are days stores were closed** (Sales = 0). These were excluded from modelling to avoid skewing predictions.

---

## Modelling — Facebook Prophet

Prophet was applied per-store with two variants:

**1. Basic model** — trend + weekly seasonality
**2. Holiday-aware model** — adds school holidays (477 unique dates) and state holidays (35 unique dates) as regressors

Store 10 was used as the demonstration store, forecasting 60 days beyond the training data (Aug–Sep 2015).

Sample forecast output for Store 10:

| Date | Trend | Lower Bound | Upper Bound |
|------|-------|-------------|-------------|
| 2015-08-01 | 5,910 | 4,181 | 6,329 |
| 2015-08-02 | 5,912 | 4,692 | 6,793 |
| 2015-08-03 | 5,912 | 5,903 | 7,965 |

---

## Getting Started

```bash
git clone https://github.com/yfi0/sales-forecasting
cd sales-forecasting
pip install -r requirements.txt
```

Place `data.csv` and `store.csv` in `src/data/`, then open and run:

```
notebooks/Rossman_Sales_Forecasting.ipynb
```

---

## Project Structure

```
├── README.md
├── requirements.txt
├── notebooks/
│   └── Rossman_Sales_Forecasting.ipynb   <- Main analysis notebook
│   └── rossman_sales_forecasting.py      <- Script version
├── src/
│   └── data/                             <- Place data files here (not tracked)
│   └── model/                            <- Saved models (coming soon)
```

---

## Roadmap

- [ ] Train/test split and RMSPE evaluation metric
- [ ] XGBoost / LightGBM baseline model for comparison
- [ ] Feature engineering (days since last promo, rolling averages)
- [ ] Forecast across all 1,115 stores
- [ ] Save trained models to `src/model/`

---

## License

MIT License
