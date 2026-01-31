# DS-BIOS: Drug Shortage Bio-Intelligence System

**DS-BIOS** is an AI-driven early warning system designed to identify U.S. prescription drugs at elevated risk of entering shortage. It integrates publicly available federal datasets to produce an interpretable, ranked watchlist that supports proactive planning by regulators, hospitals, and public health stakeholders.

This repository contains the full data pipeline, modeling workflow, and visualization artifacts used in the Presidential Artificial Intelligence Challenge (Track 2).

---

## Project Overview

Drug shortages are a persistent national healthcare challenge that can delay treatment, increase medical errors, and disproportionately affect vulnerable populations. Current responses are largely reactive, often identifying shortages only after supply disruptions occur.

DS-BIOS addresses this gap by:
- Aggregating **public FDA and CMS data**
- Learning structural patterns associated with future shortages
- Producing a **relative risk ranking** (not absolute probabilities)
- Presenting results in a **policy-friendly, interpretable format**

The system is designed as a **decision-support tool**, not an automated enforcement mechanism.

---

## Data Sources (Public & Non-Identifiable)

All data used in this project are publicly available and contain no patient-level or personally identifiable information.

1. **FDA OpenFDA – Drug Shortages**
   - Historical and current drug shortage events
   - JSON download
   - https://open.fda.gov/apis/drug/drugshortages/

2. **FDA Orange Book – Products File**
   - Manufacturer diversity and supply-side structure
   - ZIP file containing `products.txt`
   - https://www.fda.gov/drugs/drug-approvals-and-databases/orange-book-data-files

3. **CMS Medicare Part D – Drug Spending by Drug**
   - National drug demand and utilization trends
   - CSV download
   - https://data.cms.gov

---

## Modeling Approach

- **Problem Type:** Rare-event risk ranking (drug shortages)
- **Model:** Gradient Boosted Decision Trees (LightGBM)
- **Why LightGBM:**
  - Strong performance on structured tabular data
  - Handles class imbalance effectively
  - Interpretable feature importance

### Key Design Choices
- **Time-based splits** to prevent data leakage  
  - Train: 2019–2021  
  - Validation: 2022  
  - Test / deployment year: 2023
- **Ranking-based evaluation** (AUC, Precision@K) instead of raw accuracy
- **Relative risk percentiles and tiers**, not literal probabilities

---

## Key Features Used

- Prior shortage recurrence (OpenFDA)
- Manufacturer diversity (Orange Book)
- Demand level and demand growth (Medicare Part D)
- Temporal features (year-over-year changes)

---

## Outputs

- `model_table.csv` — unified modeling dataset
- `watchlist_top50.csv` — highest-risk drugs for the most recent year
- `watchlist_top50_presentable.csv` — risk percentiles and tiers
- `figures/` — publication-quality figures used in the final PDF:
  - Top-20 ranked watchlist table
  - Ranking diagnostics
  - ROC and Precision–Recall curves
  - Feature importance plots

---

## Responsible AI Considerations

- Uses **only public, aggregated data**
- No demographic, geographic, or provider-level features
- No patient data of any kind
- Transparent drivers for each flagged drug
- Designed for **human-in-the-loop decision making**

AI-assisted tools (including ChatGPT) were used only for conceptual guidance and debugging support. All data processing, modeling, evaluation, and interpretation were performed by the student.

---

## Environment & Tools

- **Platform:** Google Colaboratory
- **Language:** Python
- **Key Libraries:**
  - pandas, numpy
  - LightGBM
  - scikit-learn
  - matplotlib, plotly

---

## Intended Use

DS-BIOS is a prototype early warning system intended to:
- Support FDA monitoring efforts
- Help hospitals prioritize contingency planning
- Enable proactive supply-chain interventions

It is not a diagnostic tool and does not make automated regulatory decisions.

---

## Author

**Louis Gui**  
The Bronx High School of Science  
Presidential Artificial Intelligence Challenge — Track 2

---

## License

This project is provided for educational and research purposes only. All data sources are governed by their respective public-use terms.
