# Week 1 — Data Cleaning Documentation

## Tool Used
Excel Power Query

---

## Original Dataset

| Item | Value |
|------|-------|
| Rows | 4,774 |
| Columns | 19 |
| Source | World Bank — World Development Indicators |
| Format | .xlsx |
| Years Covered | 2000 – 2021 |
| Countries | 217 |

---

## Problems Found in Raw Data

| Problem | Details |
|---------|---------|
| Metadata rows | Last 5 rows contained World Bank notes, not actual data |
| Missing value marker | World Bank uses `..` instead of null |
| Long column names | Full indicator descriptions + codes in one name |
| Wrong data types | Year and numeric columns stored as text |
| High-null columns | 3 columns had 96%+ missing values |
| Extra spaces | 3 column names had leading/trailing spaces |

---

## Cleaning Steps Applied

### Step 1 — Removed Metadata Rows
- Deleted last 5 rows at the bottom
- These contained World Bank database notes, not data
- **Rows affected:** 5 rows removed

---

### Step 2 — Replaced Missing Value Marker
- Replaced all `..` values with `null`
- **Reason:** World Bank uses `..` to indicate no reported data
- **How:** Transform → Replace Values → `..` → empty
- **Applied to:** All columns at once

---

### Step 3 — Fixed Data Types

| Column | Old Type | New Type |
|--------|----------|----------|
| Time (Year) | Text | Whole Number |
| All indicator columns | Text | Decimal Number |
| Country Name | Text | Text |
| Country Code | Text | Text |

---

### Step 4 — Removed High-Null Columns
Columns with more than **90% missing values** were removed:

| Column Removed | Missing % | Reason |
|----------------|-----------|--------|
| Investment in water (private participation) | 96.4% | Not enough data for analysis |
| Mortality rate (unsafe water) | 96.2% | Not enough data for analysis |
| PPP Investment in water | 96.5% | Not enough data for analysis |

> These columns are documented as **data limitations** in the final report

---

### Step 5 — Renamed Columns
Shortened long indicator names for readability:

| Original Name | New Name |
|---------------|----------|
| Annual freshwater withdrawals, agriculture (% of total)... | `Agriculture_Freshwater_PCT` |
| Annual freshwater withdrawals, domestic (% of total)... | `Domestic_Freshwater_PCT` |
| Annual freshwater withdrawals, industry (% of total)... | `Industry_Freshwater_PCT` |
| Annual freshwater withdrawals, total (% of internal resources)... | `Freshwater_Internal_Resources_PCT` |
| Annual freshwater withdrawals, total (billion cubic meters)... | `Total_Withdrawal (billion cubic meters)` |
| Level of water stress... | `Water_Stress_PCT` |
| People using basic drinking water, rural (%)... | `Rural_Basic_Drinking_Water` |
| People using basic drinking water, urban (%)... | `Urban_Basic_Drinking_Water` |
| Renewable internal freshwater resources per capita... | `Freshwater_Resources_Per_Capita` |
| Water productivity, total (constant 2015 USD)... | `Water_Productivity_USD` |

---

### Step 6 — Feature Engineering (New Columns Added)

#### 1. `Water_Stress_Category`
Classifies water stress into readable labels for decision makers:

| Value | Category |
|-------|----------|
| null | null |
| Below 25% | Low |
| 25% – 70% | Medium |
| 70% – 100% | High |
| Above 100% | Critical |

**Power Query Formula:**
```
= if [Water_Stress_PCT] = null then null
  else if [Water_Stress_PCT] < 25 then "Low"
  else if [Water_Stress_PCT] < 70 then "Medium"
  else if [Water_Stress_PCT] < 100 then "High"
  else "Critical"
```

**Why useful:** Decision makers can instantly see which countries need urgent action without reading raw numbers

---

#### 2. `Dominant_Water_Sector`
Identifies which sector (Agriculture / Domestic / Industry) consumes the most water per country:

**Power Query Formula:**
```
= if [Agriculture_Freshwater_PCT] = null
     or [Domestic_Freshwater_PCT] = null
     or [Industry_Freshwater_PCT] = null
  then null
  else if [Agriculture_Freshwater_PCT] > [Domestic_Freshwater_PCT]
     and [Agriculture_Freshwater_PCT] > [Industry_Freshwater_PCT]
  then "Agriculture"
  else if [Domestic_Freshwater_PCT] > [Industry_Freshwater_PCT]
  then "Domestic"
  else "Industry"
```

**Why useful:** Helps identify where efficiency improvements will have the biggest impact

---

#### 3. `Water_Access_Gap`
Measures inequality between urban and rural water access:

**Power Query Formula:**
```
= [Urban_Basic_Drinking_Water] - [Rural_Basic_Drinking_Water]
```

**Why useful:** Reveals which countries serve city people well but neglect rural populations

---

## Final Dataset Summary

| Item | Before Cleaning | After Cleaning |
|------|----------------|----------------|
| Rows | 4,779 | 4,774 |
| Columns | 19 | 17 |
| New columns added | 0 | 3 |
| High-null columns removed | — | 3 removed (96%+) |
| Remaining nulls | — | 16–22% per column |
| Duplicate rows | 0 | 0 |
| Data types correct |

---

## Data Limitations

> Null values ranging from **16% to 22%** were **retained** in the dataset.
> They represent countries with no reported data in the World Bank database.
> These nulls are handled automatically during analysis —
> SQL and Python skip nulls in aggregations like AVG, SUM, COUNT.

> The 3 columns removed (96%+ missing) are noted as limitations
> and excluded from all analysis and visualizations.
