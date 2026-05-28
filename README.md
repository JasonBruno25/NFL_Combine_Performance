# NFL_Combine_Performance

# 🏈 Analyzing the NFL Combine's Influence on Draft Position and Team Performance

> A data science project exploring whether NFL Combine performance predicts draft position, and how the #1 overall pick affects team attendance and wins.

**Team Name:** Roger Goodell Non-Fanclub  
**Course:** Data Analytics / Sports Analytics (or appropriate course name)  
**Date:** February 2024 - May 2024

---

## 📌 Overview

The NFL Combine is an annual event where college football prospects perform physical tests in front of scouts and coaches. This project investigates two main questions:

1. **Combine performance vs. draft position** – Which combine metrics (if any) correlate with where a player is drafted? Do combine scores actually predict draft order?
2. **Impact of the #1 overall pick** – How does drafting a player first overall affect the team’s stadium attendance and win‑loss record in the following season?

We analyzed publicly available datasets from Kaggle (2000–2019) using Python (pandas, matplotlib, seaborn, scikit‑learn, statsmodels) to answer these questions.

---

## 📂 Repository Contents

- `notebooks/` – Jupyter notebooks with data cleaning, analysis, and visualizations.
- `data/` – Raw CSV files (Combine, Draft History, Attendance, Standings).
- `images/` – Generated plots (boxplots, correlation bar charts, attendance bar charts).
- `README.md` – This file.

---

## 🔍 Research Questions

1. **Which NFL combine metric is most correlated with draft position?**
2. **How does combine performance (individual metrics and a custom composite score) impact draft position?**
3. **For teams with the first overall pick, how does their record in the upcoming season compare to the prior season?**
4. **Does acquiring the #1 overall pick influence stadium attendance for the upcoming season?**

---

## 📊 Data Sources

All data was obtained from Kaggle (links provided in project report):

| Dataset | Description | Years |
|---------|-------------|-------|
| [NFL Combine Performance](https://www.kaggle.com/datasets/redlineracer/nfl-combine-performance-data-2009-2019) | Player metrics: 40‑yard dash, vertical jump, bench press, broad jump, shuttle, 3‑cone agility, age, BMI, draft status | 2009–2019 |
| [NFL Draft History](https://www.kaggle.com/datasets/dubradave/nfl-draft-history-1990-present) | Every draft pick with team, round, pick number, player name | 2000–2019 |
| [NFL Stadium Attendance](https://www.kaggle.com/datasets/sujaykapadnis/nfl-stadium-attendance-dataset) | Weekly attendance per team, home/away | 2000–2019 |
| [NFL Standings](https://www.kaggle.com/datasets/sujaykapadnis/nfl-stadium-attendance-dataset) | Wins per team per year | 2000–2019 |
| Hall of Fame analysis (reference only) | Used for context, not directly in models | – |

---

## 🧹 Data Cleaning & Preparation

- **Combine data**: Filtered for players who were drafted. Extracted pick number from `Drafted..tm.rnd.yr.` column. Removed irrelevant columns (`School`, `Position_Type`, `Height`, `Weight` – BMI already present). Removed outlier position `DB` (defensive back) that skewed distributions.
- **Attendance data**: Standardized team names and abbreviations (e.g., “Los Angeles Rams” → LAR). Computed average attendance per team per year for home games only.
- **Draft data**: Kept only the #1 overall pick for each year (2000–2019). Merged with attendance data on year and team abbreviation.
- **Previous year attendance**: Added column for attendance in the year before the player was drafted to calculate difference.

---

## 📈 Methodology & Key Findings

### Part 1: Combine Performance vs. Draft Position

#### 1. Distribution of draft picks by position
We visualized draft position (pick number) for each playing position using boxplots.

![Boxplot](images/boxplot_draft_by_position.png)

**Finding:** Positions have different draft ranges (e.g., quarterbacks tend to be drafted higher), but within‑position variance is large.

#### 2. Pearson correlation between individual combine metrics and draft position

| Metric | Correlation with Pick Number |
|--------|------------------------------|
| Age | +0.20 |
| Sprint_40yd | +0.12 |
| Vertical_Jump | −0.11 |
| Bench_Press_Reps | −0.09 |
| Broad_Jump | −0.10 |
| Shuttle | +0.06 |
| Agility_3cone | +0.07 |
| BMI | −0.08 |

**Finding:** No metric has a strong correlation (all |r| < 0.2). Age shows the highest, but still weak.

#### 3. Multiple linear regression (all metrics)

We fit the model:
PickNumber = β₀ + β₁·Age + β₂·Sprint_40 + β₃·Vertical_Jump + β₄·Bench_Press + β₅·Broad_Jump + β₆·Agility_3cone + β₇·Shuttle + β₈·BMI

---


**R² = 0.099** – Combine metrics explain only ~9.9% of the variability in draft position.

**Conclusion:** Combine performance alone is a poor predictor of draft position.

#### 4. Custom composite metric

We created a weighted score giving higher importance to the four most‑correlated metrics (Age, Vertical Jump, Broad Jump). Then we ran a simple linear regression of this custom score against draft position.

**R² = 0.001** – Even worse. The custom metric explains virtually nothing.

**Why?** Most prospects enter the combine with their draft stock already determined by game film and college production. The combine rarely elevates a player’s draft position; it’s more a confirmation for top prospects.

---

### Part 2: Impact of the #1 Overall Pick

#### A. Stadium Attendance

We compared each team’s average home attendance in the year they drafted #1 overall versus the year before.

![Attendance bar chart](images/attendance_compare.png)

**Key observations:**
- Jared Goff (Rams, 2016) saw the largest attendance increase – but this coincided with the team’s relocation to Los Angeles and a new stadium.
- David Carr (Texans, 2002) had no prior year attendance data because the Texans were an expansion team.
- Most teams showed only modest or negative changes in attendance after drafting #1 overall.

**Conclusion:** Drafting first overall does **not** guarantee a boost in attendance. Other factors (team relocation, new stadium, fan base size, overall team performance) dominate.

#### B. Team Performance (Wins)

We created an interactive widget (slider) to display team wins for any year 2000–2019. By selecting consecutive years, you can see how the worst team from one year (which gets the #1 pick) performs the next year.

**Example findings:**
- Sam Bradford (Rams, 2010): Team went from 1 win (2009) to 7 wins (2010) – positive impact.
- Myles Garrett (Browns, 2017): Team went from 1 win (2016) to 0 wins (2017) – no short‑term improvement.

**Conclusion:** The #1 pick can help, but it’s not a guaranteed turnaround. Roster depth, coaching, and injury luck matter more.

---

## 📊 Visualizations

1. **Boxplot** – Distribution of draft position by player position.
2. **Correlation bar chart** – Pearson correlation coefficients for each combine metric.
3. **Double bar chart** – Previous year vs. current year attendance for each #1 pick.
4. **Horizontal bar chart** – Attendance difference (current − previous) by team.
5. **Interactive slider widget** – Team wins by year (allows dynamic exploration).

> All plots are generated in the provided Jupyter notebooks.

---

## 🧑‍🤝‍🧑 Team Contributions

| Team Member | Contribution |
|-------------|--------------|
| **Michael Holman** | Multiple & simple linear regression models; cleaning attendance & draft data; creating `draft_att` dataframe for attendance analysis. |
| **Joey Kozohar** | Boxplot for draft position by position; correlation bar chart; cleaning standings data; slider widget; attendance difference bar chart. |
| **Jason Bruno Terceros** | Data research and selection; document organization and formatting; procedural write‑ups; co‑development of widget; data cleaning assistance. |
| **Christopher Parker** | (As listed in original – add specifics if known) |
| **Steven Bottone** | Double bar plot (prev/current attendance); difference bar plot; code commenting/organization; analysis contributions. |

---

## 🚀 How to Reproduce

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/nfl-combine-analysis.git
   cd nfl-combine-analysis
   ```
   
2. **Set up a Python environment** (recommended: `conda` or `venv` with Python 3.9+) 
   ```bash
   pip install -r requirements.txt
   ```
  Required packages: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, `statsmodels`, `ipywidgets`
  
3. **Download the datasets** from Kaggle (links above) and place CSV files in `data/`

4. **Run the Jupyter notebooks** in order:
     - `01_combine_cleaning.ipynb`
     - `02_combine_analysis.ipynb`
     - `03_attendance_analysis.ipynb`
     - `04_standings_widget.ipynb`

5. **Generate plots** - The notebooks will output figures into `images/` (or you can view inline)

## 📚 Lessons Learned
- **Correlation &ne; causation** – Even if some metrics correlate weakly with draft position, the overall predictive power is negligible
- **External factors matter** – Attendance changes are driven more by relocation, new stadiums, and team popularity than by a single draft pick
- **Data cleaning is critical** – Handling missing values (expansion teams, relocations) and standardizing team names required careful manual adjustments
- **Interactive widgets improve exploration** – The slider widget made it easy to examine team performance year‑by‑year

## 📜 Acknowledgments
- Kaggle users for providing the datasets
- NFL Combine, NFL Draft, and team attendance data sources
- Course instructors for guidance on regression analysis and data visualization

## 👥 Authors
- Michael Holman – **GitHub**
- Joey Kozohar – **GitHub**
- Jason Bruno Terceros – **GitHub**
- Christopher Parker – **GitHub**
- Steven Bottone – **GitHub**








  

