# Searching for Work in Germany

**Google Trends signals of labour migration interest and policy effects — with a focus on migrant care workers**

MDSSB-CAP-01 Capstone Project (Fall 2026) · MSc Data Science for Society and Business · Constructor University Bremen
Author: Tayaphon Rodsai ([@TayaphonRodsai](https://github.com/TayaphonRodsai)) · Supervisors: Prof. Erkan Gunes

> **Status: analysis complete (22 Nov 2026).** Done: country selection (01), keywords + quality check (02), gtab (03, 04), RQ1 (04b), RQ2 (05), RQ3 (06). Next: final presentation (24 Nov) and report (21 Dec).

---

## Why this project

Germany has an ageing society and a strong shortage of care workers. To attract foreign workers, Germany changed its rules:
the Skilled Immigration Act reforms (2023–2024), the recognition partnership (March 2024) and the
Opportunity Card / *Chancenkarte* (June 2024).

Official data comes late: work permit numbers are published once a year, often more than a year later.
Google searches happen **before** people apply. They may show early signals of who wants to come, for which jobs, and when.

## Research questions

| | Question |
|---|---|
| **RQ1 – Fix** | How can Google Trends data be made comparable across origin countries and over time? |
| **RQ2 – Check** | Does search interest in working in Germany follow the number of first permits by country of origin? |
| **RQ3 – Compare** | Did interest grow after the 2023–2024 reforms, compared with other destinations (Canada, the UK, Australia)? |

## Countries

Selected with evidence from three official sources (see [Notebook 01](notebooks/01_country_selection.ipynb)).
A country is selected if it has at least 2 of 3 types of evidence: **E1** many care-route permits, **E2** many vocational-training permits, **E3** confirmed care country (Triple Win partner or top care nationality in Germany).

**India · Türkiye · Viet Nam · Morocco · Tunisia · Philippines · Indonesia · Bosnia and Herzegovina · Cameroon**

All 9 have enough Google Trends data (no zero months for the topic "Germany", Jan 2015 – Oct 2026).

## Data

| Source | What | Used for |
|---|---|---|
| [Google Trends](https://trends.google.com) | Monthly relative search interest, Jan 2015 – Oct 2026 | RQ1–RQ3 |
| [Eurostat `migr_resfirst`, `migr_resedu`](https://ec.europa.eu/eurostat/web/migration-asylum/managed-migration/database) | First residence permits issued by Germany, by citizenship and reason (work, education) | Country selection, RQ2 |
| Bundesagentur für Arbeit (2026): *Arbeitsmarktsituation in Pflegeberufen* | Foreign workers in care occupations | Country selection |

## Method: how keywords are chosen

Keywords come from **what people really search**, not from the author's guesses (see [Notebook 02](notebooks/02_keywords.ipynb)):

1. **Round 1** — related queries (top + rising) for the topic *Germany* in each country; keep only queries about work, care, training, language or visa.
2. **Round 2** — search the best round-1 keywords plus a few local seed words; their related queries give more specific keywords.
3. **Check** — native speakers confirm the words are really used (done for Türkiye and Indonesia; India and Tunisia pending). In Google Trends, a term without quotation marks also counts longer searches that contain all its words, so short keywords include the longer phrases people type.
4. **Calibrate** — make series comparable with [`gtab`](https://github.com/epfl-dlab/GoogleTrendsAnchorBank) (anchor banks).

Decisions on keywords are logged in [`docs/keyword_log.csv`](docs/keyword_log.csv).

## Notebooks

| # | Notebook | Status |
|---|---|---|
| 01 | [Country selection](notebooks/01_country_selection.ipynb) | ✅ done |
| 02 | [Keyword discovery](notebooks/02_keywords.ipynb) | ✅ done — 20 final keywords |
| 03 | [gtab test](notebooks/03_gtab_test.ipynb) | ✅ done |
| 04 | [Calibration with `gtab` (RQ1)](notebooks/04_gtab_calibration.ipynb) | ✅ all 9 countries |
| 04b | [Comparing countries (RQ1)](notebooks/04b_rq1_compare_countries.ipynb) | ✅ done (9 countries) |
| 05 | [Searches vs. permits (RQ2)](notebooks/05_rq2_searches_vs_permits.ipynb) | ✅ done |
| 06 | [Germany vs. other destinations (RQ3)](notebooks/06_rq3_destinations.ipynb) | 🔄 first results (care, 4 countries) |

### Main results

**RQ1 – Fix (Notebooks 03, 04, 04b).** Each country's keywords were calibrated with `gtab` and divided by a common reference search (YouTube; the topic *Germany* as a check). The country ranking is almost the same with both references (Spearman 0.78–0.92 for care, training and work), so cross-country comparisons are robust. Comparisons over time need care: YouTube searches fell 43–69% between 2015 and 2021. Keyword check: 18 of 20 final keywords appear in Google's related-query lists or were confirmed by a native speaker.

**RQ2 – Check (Notebook 05).** Yearly searches and first permits (2015–2025) move together: pooled **r = 0.54** over 18 keywords. Robustness tests (yearly changes, year fixed effects, placebo with other countries' permits, a second Google sample) show that much of this is a **trend shared by all countries**. A clear country-specific link appears mainly for **Ausbildung** searches.

**RQ3 – Compare (Notebook 06).** Nursing searches for Germany vs. Canada, the UK, Australia and Japan (Philippines, India, Viet Nam, Indonesia; Jan 2022 – Oct 2026). After the March 2024 reform step, Germany gained relative to the other destinations in the Philippines, India and Indonesia (difference-in-differences +0.34 to +0.51 log points), but not in Viet Nam. The checks show that this cannot be attributed to the reforms: India and Indonesia already had a rising trend before, the Philippine result depends on Canada and the UK falling, and with a time trend no country shows a significant jump.

## Repository structure

```
├── data/
│   ├── raw/             # downloaded data, never edited (Eurostat, Google Trends)
│   └── processed/       # cleaned tables made by the notebooks
├── docs/
│   ├── keyword_log.csv  # every keyword decision and its source
│   └── seed_terms.md    # seed words per country
├── notebooks/           # analysis, step by step
├── reports/figures/     # charts made by the notebooks
├── requirements.txt
└── README.md
```

## How to run

```bash
git clone https://github.com/TayaphonRodsai/care-migration-google-trends.git
cd care-migration-google-trends
python -m venv .venv
.venv\Scripts\activate          # macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
```

Then open the notebooks in VS Code or Jupyter and run them in order (01, 02, …).

**gtab and new Python versions:** `gtab` 0.8 does not run as-is with Python 3.14 / pandas 3. Apply the small fixes in [`patches/gtab_0.8_core.patch`](patches/gtab_0.8_core.patch) to the installed `gtab/core.py`. Google Trends limits the number of requests per day, so anchor banks are built one country per day.
Notebooks read and write data with relative paths (`../data/...`).

## Limits

- Google Trends values are relative (0–100), sampled, and depend on language.
- In some countries job seeking happens mostly on Facebook; Google Trends shows *information seeking*, not the whole job search.
- Permit data is yearly, so there are few data points per country, and we cannot show that searches come *before* permits (monthly data would be needed). Many 2024 values look unusually low (to be checked).
- Google changed its data collection in January 2016 and January 2022.
- Native-speaker checks were possible only for some countries.
- Results show associations, not causes.

## Ethics

Social media is used only to confirm keywords. Only public groups and public ad libraries are viewed.
No names, images or personal data are collected — only search words and the type of source.

## Main references

- Böhme, Gröger & Stöhr (2020). Searching for a better life: Predicting international migration with online search keywords. *Journal of Development Economics*, 142.
- Avramescu & Wiśniowski (2021). Now-casting Romanian migration into the United Kingdom by using Google Search engine data. *Demographic Research*, 45(40).
- West, R. (2020). Calibration of Google Trends time series. *CIKM 2020* (gtab).
