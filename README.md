# 🏥 OpenFDA API Performance & Drug Safety Analytics Dashboard

> A Tableau dashboard that monitors OpenFDA API health metrics while surfacing actionable drug safety insights — built to demonstrate end-to-end data analytics capability in a healthcare context.

---

## 📌 Project Overview

This project combines **API performance monitoring** with **healthcare data analytics** using the [OpenFDA API](https://open.fda.gov/apis/) — a publicly available U.S. government dataset covering drug adverse events, recalls, and labeling.

The dashboard has two layers:
- **Technical Layer** — tracks API response times, error rates, throughput, and availability
- **Analytical Layer** — uncovers patterns in drug adverse events, patient demographics, serious reaction rates, and geographic distribution

---

## 🎯 Why OpenFDA?

| Factor | Details |
|---|---|
| **Relevance** | Real-world U.S. FDA drug safety data |
| **Access** | Free, public, no authentication required |
| **Richness** | Millions of adverse event records dating back decades |
| **Healthcare Fit** | Directly applicable to health analytics, pharmacovigilance, and risk modeling |

---

## 📊 Dashboard Panels

### Panel 1 — API Health Monitor
| Metric | Description |
|---|---|
| Avg Response Time (ms) | Mean latency per API call |
| P95 Response Time | 95th percentile latency — catches outlier slowdowns |
| Error Rate (%) | Share of 4xx/5xx responses |
| Request Volume Over Time | Call frequency trend (line/area chart) |
| Status Code Breakdown | Distribution of 200, 400, 404, 500 responses |

### Panel 2 — Drug Adverse Event Insights
- Top 10 drugs by adverse event report count
- Serious vs. non-serious reaction ratio (stacked bar)
- Adverse event trend over time (monthly)
- Most common reaction types (word cloud / bar)

### Panel 3 — Patient Demographics
- Age group distribution of adverse event reporters
- Sex-wise breakdown of serious reactions
- Reporter type (consumer, physician, pharmacist)

### Panel 4 — Geographic Analysis
- Country-wise adverse event report volume (map)
- Top 5 countries by serious event count

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| Python 3.x | API data collection script |
| `requests` library | HTTP calls to OpenFDA endpoints |
| `pandas` | Data cleaning and transformation |
| CSV / Excel | Intermediate data storage |
| Tableau Desktop / Public | Dashboard visualization |

---

## 📁 Project Structure

```
openfda-dashboard/
│
├── data_collection/
│   ├── fetch_adverse_events.py     # Hits drug/event endpoint, logs performance + content
│   ├── fetch_recalls.py            # Hits drug/recall endpoint
│   └── output/
│       └── api_performance_log.csv # Generated data file for Tableau
│
├── tableau/
│   └── openfda_dashboard.twbx      # Packaged Tableau workbook
│
├── docs/
│   └── dashboard_preview.png       # Screenshot of the final dashboard
│
├── requirements.txt
└── README.md
```

---

## ⚙️ Setup & Usage

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/openfda-dashboard.git
cd openfda-dashboard
```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Run the Data Collection Script
```bash
python data_collection/fetch_adverse_events.py
```
This will:
- Make repeated calls to the OpenFDA `drug/event` endpoint
- Log API performance metrics (response time, status code, etc.)
- Extract drug safety fields from the response body
- Save everything to `output/api_performance_log.csv`

### 4. Load CSV into Tableau
- Open Tableau Desktop
- Connect → Text File → select `api_performance_log.csv`
- Set data types (timestamp → Date & Time, response_time_ms → Number)
- Build charts as described in the Dashboard Panels section above

---

## 📋 Data Schema

The output CSV contains the following columns:

| Column | Type | Description |
|---|---|---|
| `timestamp` | DateTime | When the API call was made |
| `endpoint` | String | Which OpenFDA endpoint was called |
| `status_code` | Integer | HTTP response code |
| `response_time_ms` | Float | Latency in milliseconds |
| `records_returned` | Integer | Number of results in the response |
| `drug_name` | String | Name of the drug in the report |
| `reaction_type` | String | Reported adverse reaction |
| `patient_age_group` | String | Age group of the patient |
| `serious` | Integer | 1 = serious event, 0 = non-serious |
| `country` | String | Country where event was reported |
| `report_date` | Date | Date of the original FDA report |

---

## 💡 Key Insights the Dashboard Surfaces

- **Latency spikes** at certain call frequencies → helps identify API rate limit thresholds
- **Drugs with disproportionately high serious event rates** → signals pharmacovigilance risk
- **P95 vs median gap** → exposes tail latency issues invisible in averages
- **Age group patterns** → older patient cohorts show higher serious reaction rates
- **Geographic concentration** → majority of reports originate from the US, with a secondary cluster in EU countries

---

## 🔗 API Reference

- Base URL: `https://api.fda.gov/`
- Endpoints used:
  - `drug/event.json` — Adverse drug event reports
  - `drug/recall.json` — Drug recall data
- Docs: [https://open.fda.gov/apis/](https://open.fda.gov/apis/)
- Rate limit: 1,000 requests/day (unauthenticated), 120,000/day (with API key)

---

## 📄 License

This project uses publicly available government data from the U.S. Food & Drug Administration via the OpenFDA platform. Data is provided under the [Creative Commons CC0 1.0 Universal Public Domain Dedication](https://creativecommons.org/publicdomain/zero/1.0/).

---
