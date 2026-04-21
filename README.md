# Water Security Analysis — World Bank Dataset

## Project Overview
A data analysis project examining global water security indicators  
across **217 countries** from **2000 to 2021** using World Bank data.

The goal is to provide **data-driven insights** to help decision makers  
understand water stress, access inequality, and resource efficiency  
at both global and regional levels.

---

##  Project Status

| Week | Topic | Status |
|------|-------|--------|
| Week 1 | Data Cleaning & Preprocessing |  Done |
| Week 2 | Analysis Questions |  In Progress |
| Week 3 | Forecasting |  Not Started |
| Week 4 | Dashboard & Presentation |  Not Started |

---

##  Project Structure

```
water-analysis-worldbank/
│
├── README.md                             ← You are here
│
├── data/
│   └── World_Bank_Water_Dataset.xlsx     ← Cleaned dataset
│
└── week1_cleaning/
    └── cleaning_documentation.md        ← All cleaning steps documented
```

---

##  Dataset

| Item | Details |
|------|---------|
| Source | [World Bank — World Development Indicators](https://databank.worldbank.org) |
| Countries | 217 |
| Years | 2000 – 2021 |
| Original Columns | 19 |
| Final Columns | 17 |
| Total Rows | 4,774 |

---

##  Tools Used

| Tool | Purpose |
|------|---------|
| Excel Power Query | Data Cleaning & Feature Engineering |
| SQL | Data Modeling *(coming in Week 2)* |
| Python (pandas, scikit-learn) | Forecasting *(coming in Week 3)* |
| Tableau | Dashboard & Visualization *(coming in Week 4)* |

---

## ✅ Week 1 — What We Did

### Cleaning Steps:
- Removed metadata rows at the bottom of the dataset
- Replaced World Bank missing value marker `..` with null
- Removed 3 columns with 96%+ missing values
- Fixed data types (Year → Whole Number, indicators → Decimal)
- Renamed long column names to short readable names

### New Columns Added (Feature Engineering):
| Column | Description |
|--------|-------------|
| `Water_Stress_Category` | Classifies stress as Low / Medium / High / Critical |
| `Dominant_Water_Sector` | Which sector uses the most water per country |
| `Water_Access_Gap` | Difference between urban and rural water access % |

---

