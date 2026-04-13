# 🏅 Olympic Athletes — Historical Performance Analysis (1896–2016)

> A data-driven exploration of 120 years of Olympic history, uncovering patterns in national dominance, athlete physiology, and gender participation using a dataset of ~270,000 athlete entries.

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Dataset](#-dataset)
- [Data Preparation & Feature Engineering](#-data-preparation--feature-engineering)
- [Exploratory Data Analysis](#-exploratory-data-analysis)
- [Hypothesis 1 — Country Performance](#-hypothesis-1--country-performance)
- [Hypothesis 2 — Physical Attributes & Medal Success](#-hypothesis-2--physical-attributes--medal-success)
- [Hypothesis 3 — Female Participation Growth](#-hypothesis-3--female-participation-growth)
- [Final Summary of Findings](#-final-summary-of-findings)
- [Limitations](#-limitations)
- [Future Work](#-future-work)
- [Tech Stack](#-tech-stack)

---

## 🎯 Project Overview

This project analyzes 120 years of Olympic athlete data to investigate three core hypotheses:

| # | Hypothesis |
|---|-----------|
| 1 | Countries with historically strong Olympic programs show higher and increasing medal counts over time |
| 2 | Athlete physical attributes (height, weight, age) influence medal outcomes depending on the sport |
| 3 | Female participation in the Olympics has grown faster than male participation over time |

---

## 📦 Dataset

| File | Rows | Columns | Description |
|------|------|---------|-------------|
| `athlete_events.csv` | ~270,000 | 15 | Athlete-level Olympic event data (1896–2016) |
| `noc_regions.csv` | 205 | 3 | Mapping of NOC codes to country/region names |

**Dataset Stats after Cleaning:**

| Metric | Count |
|--------|-------|
| Total Unique Athletes | 134,724 |
| Countries Represented | 207 |
| Sports | 66 |
| Olympic Games | 51 |

---

## 🔧 Data Preparation & Feature Engineering

### Cleaning Steps
- **Merged** athlete events with NOC regions via LEFT JOIN on the `team` column
- **Corrected** missing country mappings — boat names, unknown delegations, and the Refugee Olympic Team were manually mapped or removed
- **Imputed** missing `age`, `height`, and `weight` using **sport-level median values** to preserve sport-specific characteristics
- **Replaced** null medal values with `"No Medal"`
- **Removed** duplicate athlete entries
- **Standardized** all column names to lowercase

### Engineered Features

| Feature | Description |
|---------|-------------|
| `medal_flag` | Binary (1 = medal won, 0 = no medal) — enables medal probability and regression modeling |
| `decade` | Groups Olympic years into decades for trend analysis |
| `bmi` | Calculated from height and weight — standardized body composition metric for cross-sport comparisons |

---

## 📊 Exploratory Data Analysis

### Athlete Participation by Decade
Participation grew steadily across Olympic history, with the **2000s decade recording the highest number of athletes (49,357)**. The post-WWII era saw sharp acceleration in global participation.

### Olympic Sports Over Time
The number of sports expanded from **9 in 1896 to a peak of 41**, with the most significant expansion occurring **after the mid-20th century** as new disciplines were added to both Summer and Winter Games.

### Top 10 Most Participated Sports

| Rank | Sport | Athlete Entries |
|------|-------|----------------|
| 1 | Athletics | 38,624 |
| 2 | Gymnastics | 26,707 |
| 3 | Swimming | 23,195 |
| 4 | Shooting | 11,448 |
| 5 | Cycling | 10,827 |
| 6 | Fencing | 10,735 |
| 7 | Rowing | 10,595 |
| 8 | Cross Country Skiing | 9,133 |
| 9 | Alpine Skiing | 8,829 |
| 10 | Wrestling | 7,154 |

> **Athletics** leads by a wide margin, reflecting its presence in virtually every Olympic edition and its large number of track and field events.

---

## 🌍 Hypothesis 1 — Country Performance

### Approach
- Identified the **Top 15 countries** by all-time medal count
- Plotted **medal share percentage** over Olympic history, grouped in sets of 5 for clarity

### Key Findings

**✅ Hypothesis Supported**

| Insight | Detail |
|---------|--------|
| 🇺🇸 US Consistent Dominance | Sustained top medal share across all eras despite rising global competition |
| 🇷🇺 Soviet/Russian Cold War Rise | USSR emerged as a dominant force from the 1950s; medal share peaked during the Cold War era |
| 🇨🇳 China's Modern Surge | Rapid rise from the 1980s onward — one of the steepest growth curves in Olympic history |
| 🌐 Smaller Nations | More volatile medal shares with brief peaks; lack sustained dominance |
| 📉 Globalization Effect | Medal concentration decreases over time as Olympic participation expands globally |

> **Conclusion:** Countries with strong sports infrastructure, training systems, and long-term investment consistently dominate medal tallies. Smaller nations show irregular performance, reflecting resource and infrastructure gaps.

---

## 💪 Hypothesis 2 — Physical Attributes & Medal Success

### Approach
Sports were categorized by the physical trait expected to provide a competitive advantage:

| Category | Sports |
|----------|--------|
| **Height-Advantage** | Basketball, Volleyball, Rowing, Swimming |
| **Weight-Advantage** | Weightlifting, Wrestling, Judo, Boxing |
| **Age-Sensitive** | Gymnastics, Diving, Figure Skating, Swimming |

Medalists and non-medalists were compared on the relevant attribute within each category.

### Results

| Attribute | Medalists (avg) | Non-Medalists (avg) | Difference | p-value | Interpretation |
|-----------|----------------|---------------------|-----------|---------|---------------|
| Height (cm) | 184.28 | 181.45 | +2.83 cm | < 0.001 | Statistically significant ✅ |
| Weight (kg) | 74.56 | 72.86 | +1.70 kg | < 0.001 | Statistically significant ✅ |
| Age (yrs) | 22.10 | 21.86 | +0.24 yrs | < 0.001 | Significant but negligible ⚠️ |

### Key Findings

**⚠️ Hypothesis Partially Supported**

- **Height** provides a measurable advantage in sports involving reach, leverage, or stroke length (basketball, rowing, volleyball)
- **Weight** correlates with competitive success in strength-oriented sports (wrestling, weightlifting, boxing)
- **Age** shows a statistically significant p-value due to large sample size, but the actual difference (~0.24 years) is **practically negligible** — age alone does not reliably predict medal outcomes
- Physical advantages are **sport-specific**, not universal

> **Conclusion:** Height and weight demonstrate statistically significant and meaningful advantages in the appropriate sport categories. Age, while detectable statistically, does not meaningfully differentiate medalists from non-medalists — suggesting training, experience, and technique play a larger role.

---

## 👩 Hypothesis 3 — Female Participation Growth

### Approach
- Plotted total male vs. female athlete counts over Olympic history
- Calculated **participation share (%)** per year to control for overall growth in athlete numbers

### Key Findings

**✅ Hypothesis Strongly Supported**

| Era | Female Share |
|-----|-------------|
| 1896–1920 | ~0% (near zero) |
| 1980s | ~25% |
| Recent Olympics | ~45% |

- Female participation grew from **less than 5%** in early Games to **nearly 45%** in recent editions
- Growth rate of female athletes has **significantly outpaced** male athlete growth in the post-1990 period
- The alternating zig-zag pattern in raw counts after 1992 is explained by the **separation of Summer and Winter Games** — not an actual decline in participation
- Modern Olympics are approaching **near gender parity**

> **Conclusion:** The data strongly confirms that female Olympic participation has expanded at a faster rate than male participation throughout history, reflecting broader global progress toward gender equality in sport.

---

## 📋 Final Summary of Findings

| Hypothesis | Verdict | Key Takeaway |
|------------|---------|-------------|
| H1 — Country Performance | ✅ Supported | USA, Russia, and China dominate consistently; smaller nations show irregular patterns |
| H2 — Physical Attributes | ⚠️ Partially Supported | Height and weight matter in sport-specific contexts; age is not a meaningful predictor |
| H3 — Female Participation | ✅ Strongly Supported | Female participation grew from ~0% to ~45% — far outpacing male growth |

---

## ⚠️ Limitations

- **Missing data imputation** using sport-level medians may introduce minor bias in physical attribute comparisons
- **Team sports** introduce confounding factors (strategy, coaching, team dynamics) not captured by individual physical attributes
- **Sport subset analysis** — physical attribute findings may not generalize to all 66 Olympic sports
- Dataset covers through **2016 only**; trends may have continued to evolve

---

## 🔭 Future Work

- **Predictive Modeling** — Build medal probability models using athlete attributes (height, weight, age, sport, country)
- **Country Efficiency Analysis** — Examine how effectively nations convert athlete participation into medals (medals per athlete sent)
- **Deep Sport-Level Segmentation** — Analyze physical advantage within individual events rather than sport categories
- **Time-Series Forecasting** — Project future participation and medal share trends

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=flat&logo=matplotlib&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-8CAAE6?style=flat&logo=scipy&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat&logo=jupyter&logoColor=white)

- **Data Manipulation:** `pandas`
- **Visualization:** `matplotlib`, `seaborn`
- **Statistical Testing:** `scipy.stats` (independent two-sample t-tests)
- **Environment:** Jupyter Notebook

---

*Dataset source: [120 Years of Olympic History — Kaggle](https://www.kaggle.com/datasets/heesoo37/120-years-of-olympic-history-athletes-and-results)*
