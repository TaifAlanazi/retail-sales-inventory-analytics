# Retail Sales & Inventory Performance Dashboard

An end-to-end data analytics project exploring the relationship between sales growth, discount strategy, profitability, and inventory efficiency across a four-year retail transaction dataset.

---

## Business Problem

High sales volume does not consistently translate into profitability. Aggressive discounting drives demand but erodes margins, leading to inefficient inventory allocation and a significant share of unprofitable transactions despite overall revenue growth.

**Core finding:** $12.6M in revenue · $1.5M profit · 25% of orders unprofitable · profit margin flat at 11.6% for four consecutive years.

---

## Project Structure

```
retail-sales-inventory-analytics/
│
├── data/
│   └── superstore_orders.csv          # Raw dataset (Superstore — Tech, Office, Furniture)
│
├── notebooks/
│   └── retail_eda.ipynb               # Python exploratory data analysis
│
├── dashboard/
│   └── retail_dashboard.pbix          # Power BI dashboard file
│
├── exports/
│   └── dashboard_overview.pdf         # PDF export of all dashboard pages
│
└── README.md
```

---

## Tools & Stack

| Tool | Purpose |
|---|---|
| Python (pandas, matplotlib, seaborn) | Exploratory data analysis, data cleaning, statistical findings |
| Jupyter Notebook | EDA documentation and visualization |
| Power BI Desktop | Interactive dashboard design and DAX measures |
| Power Query (M) | Data transformation and column engineering |
| DAX | Custom measures — profit margin, discount correlation, stocking action logic |

---

## Dataset

**Source:** Superstore Sales Dataset (public domain)  
**Period:** 2011 – 2014  
**Categories:** Technology · Furniture · Office Supplies  
**Records:** ~10,000 orders  
**Key fields:** order_date · product_name · category · sub_category · sales · profit · discount · quantity

---

## Dashboard Pages

| Page | Title | Key Visual |
|---|---|---|
| 1 | Title Page | Business problem statement |
| 2 | Executive Overview | KPI cards · trend line · category donut |
| 3 | Discount Strategy Is Eroding Profitability | Heatmap · area chart · KPI cards |
| 4 | Inventory Strategy | Risk matrix scatter · profit per unit · reorder table |
| 5 | Product Profitability Analysis | Quadrant scatter · bottom 10 bar · profit trend matrix |
| 6 | Strategic Recommendations | 4 recommendation cards · impact targets · strategic conclusion |

---

## Key Findings

- **25%** of all orders are unprofitable despite strong revenue growth
- Discounts above 50% generate an average loss of **−$98.89 per order**
- **9.46%** of orders apply damaging discounts above 50%
- Technology earns **$18.87 profit per unit** — 3.9× more than Office Supplies at $4.79
- Phones lose **$207 per order** when discounted above 50%
- Canon imageCLASS Copier: **$25K+ profit from only 20 units** — highest profit-per-unit in the dataset
- Total portfolio losses from loss-making products grew from **−$42K (2011) to −$81K (2014)** — nearly doubled in 4 years
- Discount–profit correlation: **−0.32** — more discount consistently means less profit

---

## Strategic Recommendations

| Priority | Recommendation | Data Basis |
|---|---|---|
| High | Cap all discounts at 40% | Above 50% averages −$98.89 loss per order |
| Medium | Prioritise Technology in inventory and marketing | $18.87/unit vs $4.79/unit for Office Supplies |
| Medium | Align inventory decisions with profit per unit | 41% of orders at Reorder threshold — not all profitable |
| Ongoing | Review and discontinue structural loss-makers | Losses grew from −$42K to −$81K over 4 years |

---

## DAX Measures Built

```dax
-- Profit Margin %
Profit Margin % = DIVIDE(SUM(profit), SUM(sales), 0)

-- Profit Per Unit
Profit Per Unit = DIVIDE(SUM(profit), SUM(quantity), 0)

-- % Unprofitable Orders
% Unprofitable Orders =
DIVIDE(
    COUNTROWS(FILTER(SuperStoreOrders_afterEDA, profit < 0)),
    COUNTROWS(SuperStoreOrders_afterEDA),
    0
)

-- Stocking Action (Measure)
Stocking Action =
VAR CurrentAvgProfit = [Avg Profit]
VAR CurrentStatus = MAX(SuperStoreOrders_afterEDA[Stock Status])
RETURN
SWITCH(TRUE(),
    CurrentStatus = "Reorder"   && CurrentAvgProfit >= 0, "✓ Restock",
    CurrentStatus = "Reorder"   && CurrentAvgProfit <  0, "⚠ Review first",
    CurrentStatus = "In stock"  && CurrentAvgProfit >= 0, "● Monitor",
    CurrentStatus = "In stock"  && CurrentAvgProfit <  0, "⚠ Check discounts",
    CurrentStatus = "Overstock" && CurrentAvgProfit >= 0, "↓ Reduce stock",
    CurrentStatus = "Overstock" && CurrentAvgProfit <  0, "⚠ Urgent",
    "—"
)
```

---

## Python EDA Highlights

```python
# Average profit by discount band
df.groupby('discount_band')['profit'].mean()

# Discount–profit correlation
df[['discount', 'profit']].corr()

# Top products by profit per unit
df.groupby('product_name').apply(
    lambda x: x['profit'].sum() / x['quantity'].sum()
).sort_values(ascending=False).head(10)
```

---

## Author

**Taif S. Alanazi**  
Data Analyst · Python · Power BI · SQL  
[LinkedIn]((https://www.linkedin.com/in/taifsamran/)) · [GitHub](https://github.com/TaifAlanazi))

---

## License

Dataset: Superstore Sales (public domain)  
Analysis and dashboard: © 2026 Taif S. Alanazi
