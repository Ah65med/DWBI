# Global Health Data Warehouse

A Data Warehouse and Business Intelligence project built on global health
statistics. An ELT pipeline extracts data from a CSV dataset and a live
COVID-19 API, transforms and validates it, then loads it into a
star-schema SQLite data warehouse. Results are visualised in a Power BI
dashboard.

Built as a Data Warehousing and Business Intelligence (DWBI) course project.

## Pipeline Overview

The ELT pipeline (`elt.ipynb`) runs in five stages:

**1. Extraction**
Loads `global-health-statistics.csv` from the local `data/` folder and
fetches live country-level COVID-19 data from the
[disease.sh API](https://disease.sh/v3/covid-19/countries).

**2. Data Quality Assessment**
Profiles the raw dataset — missing values, duplicates, data types,
numeric summaries, and categorical distributions — and logs a full
quality report.

**3. Transformation**
Cleans column names, removes duplicates, fills missing numeric values
with column medians, and engineers derived fields.

**4. Warehouse Loading (Star Schema)**
Builds a SQLite data warehouse with dimension and fact tables:

| Table | Description |
|---|---|
| `DimCountry` | Country with socioeconomic attributes (income, education, urbanisation) |
| `DimDisease` | Disease name, category, treatment type, vaccine availability |
| Fact table | Core health metrics joined across dimensions |

**5. Export**
Exports warehouse tables to CSV for use in Power BI and other BI tools.

## Project Structure
dwh/

├── elt.ipynb                        # Full ELT pipeline notebook

├── data/

│   └── global-health-statistics.csv # Source dataset (not included, see below)

├── docs/

│   ├── dashboard.pbix               # Power BI dashboard

│   └── report.docx                  # Project report

└── bi/

├── presentation.pptx            # Project presentation

└── global_health_bi_dashboard.csv # Exported BI data (not included, see below)

## Getting Started

### Prerequisites

```bash
pip install pandas numpy requests jupyter
```

### Dataset

The source dataset is not included due to its size. Download
**Global Health Statistics** from Kaggle and place it at:
dwh/data/global-health-statistics.csv

### Running the Pipeline

```bash
jupyter notebook dwh/elt.ipynb
```

Run all cells in order. The pipeline will:
- Load and validate the CSV
- Fetch live COVID-19 data from the API
- Transform and clean the data
- Build the SQLite warehouse at `data_warehouse/global_health_datawarehouse.db`
- Export BI-ready CSVs to `data_warehouse/exports/`

### Power BI Dashboard

Open `docs/dashboard.pbix` in Power BI Desktop. If prompted to refresh
the data source, point it to the exported CSVs in
`data_warehouse/exports/` after running the pipeline.
