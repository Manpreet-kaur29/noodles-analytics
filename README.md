# Noodles Crypto Analytics Platform
**GitHub Repository:** https://github.com/Manpreet-kaur29/noodles-analytics

![Executive Dashboard](Screenshot%20(25).png)
![Executive Dashboard](Screenshot%20(24).png)
![Executive Dashboard](Screenshot%20(23).png)
![Executive Dashboard](Screenshot%20(26).png)

---

## Project Overview

Production-ready currency analytics platform processing 1M+ rows of market data
daily using Python, MySQL, and Power BI.

**Key features:**

- Automated Python ETL pipeline (5-minute runtime)
- Star schema data warehouse design
- Interactive Power BI dashboards
- Comprehensive data quality validation
- Scheduled daily refresh

---

## Architecture

```
JSON files  →  Python ETL  →  MySQL (noodles_dw)  →  Power BI Dashboards
```

Six layers — source, staging, core warehouse, aggregation, presentation, and
orchestration — run top to bottom. The full diagram lives in
[`docs/architecture-diagram.png`](docs/architecture-diagram.png).

---

## Technology Stack

| Layer | Technology |
|---|---|
| ETL | Python 3.9, pandas, SQLAlchemy |
| Data Warehouse | MySQL 8.x |
| Visualization | Power BI Desktop, DAX |
| Orchestration | Jupyter notebooks (`05` and `06`) |

---

## Project Structure

```
noodles-analytics/
├── 01_my_first_analysis.ipynb
├── 02_twitter_analysis.ipynb
├── 03_reddit_analysis.ipynb
├── 04_combined_dashboard.ipynb
├── 05_data_warehouse_design.ipynb
├── 06_powerbi_prep.ipynb
├── reports/                        (Tasks 7–8 .pbix files)
├── docs/
│   ├── architecture-diagram.png    ← §1
│   ├── data-dictionary.xlsx        ← §2
│   ├── technical-runbook.md        ← §3
│   ├── user-guide.md               ← §4
│   ├── demo-presentation.pptx      ← §5
│   ├── demo-video.mp4              ← §6
│   └── final-checklist.md          ← §7
└── README.md
```

---

## Data Model

**Dimensions:** `DimCurrency`, `DimDate`, `DimPlatform`
**Facts:** `FactSocialEngagement`
**Grain:** one row per engagement event

The full column-level dictionary is in
[`docs/data-dictionary.xlsx`](docs/data-dictionary.xlsx).

---

## Key Insights

- **Bitcoin Dominance:** 45% of total market capitalisation
- **Volume Leaders:** top 20 currencies = 90% of trading volume
- **Social Correlation:** engagement predicts 30% of price movements
- **Volatility:** average daily price change of ±8%

---

## Achievements

- Processed 1M+ rows of currency data
- Built an automated ETL pipeline
- Delivered interactive Power BI dashboards
- Referential integrity maintained in the star schema

---

## Documentation

| Document | Path |
|---|---|
| Technical Runbook | [`docs/technical-runbook.md`](docs/technical-runbook.md) |
| User Guide | [`docs/user-guide.md`](docs/user-guide.md) |
| Architecture Diagram | [`docs/architecture-diagram.png`](docs/architecture-diagram.png) |
| Data Dictionary | [`docs/data-dictionary.xlsx`](docs/data-dictionary.xlsx) |
| Demo Presentation | [`docs/demo-presentation.pptx`](docs/demo-presentation.pptx) |
| Final Checklist | [`docs/final-checklist.md`](docs/final-checklist.md) |

### Related documents from earlier tasks

- Task 5 — `data_warehouse_schema.md` (schema export, not the architecture PNG)
- Task 8 — `reports/executive-dashboard-guide.md`
- Task 8 — `reports/dax-measures-reference.md`

---

## Getting Started

**Prerequisites:** Python 3.9+, MySQL 8.x, Power BI Desktop, Anaconda or a
Python environment with Jupyter.

```bash
# 1. Clone the repository
git clone https://github.com/YOUR-USERNAME/noodles-analytics.git
cd noodles-analytics

# 2. Install dependencies
pip install pandas sqlalchemy pymysql python-dotenv matplotlib seaborn

# 3. Configure your database connection in .env
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=yourpassword
DB_NAME=noodles_dw

# 4. Build the warehouse, then the aggregation tables
jupyter nbconvert --execute --to notebook \
  --output-dir logs/executed \
  05_data_warehouse_design.ipynb 06_powerbi_prep.ipynb
```

Then open `reports/*.pbix` in Power BI Desktop and refresh.

> **Never commit your `.env` file.** It holds your database password.

---

## Demo

**Demo video:** https://www.loom.com/share/17041c80c6324838a785d087821aef51

**Presentation:** [`docs/demo-presentation.pptx`](docs/demo-presentation.pptx)

---

## Contact

**Manpreet Kaur** | [your.email@example.com]

---

## Documentation Index

| Deliverable | Link |
|---|---|
| Architecture Diagram | [Architecture Diagram](docs/architecture-diagram.png) |
| Data Dictionary | [Data Dictionary](docs/data-dictionary.xlsx) |
| Technical Runbook | [Technical Runbook](docs/technical-runbook.md) |
| User Guide | [User Guide](docs/user-guide.md) |
| Presentation slides | [Presentation slides](docs/demo-presentation.pptx) |

> Paths above resolve on GitHub once the files are pushed. `docs/screenshots/executive-dashboard.png`
> is optional — add it when you have a screenshot from Task 8.
