# Pokemon Data Analysis

A complete end-to-end data analysis project exploring 801 Pokemon across Generations 1–7.
Covers data cleaning, exploratory analysis, static and interactive visualizations,
machine learning clustering, and competitive role identification.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Setup & Installation](#setup--installation)
- [How to Run](#how-to-run)
- [Scripts Breakdown](#scripts-breakdown)
- [Key Insights](#key-insights)
- [Charts Gallery](#charts-gallery)
- [Machine Learning Results](#machine-learning-results)
- [Technologies Used](#technologies-used)

---

## Project Overview

This project performs a thorough analysis of the Pokemon dataset to uncover patterns in
base stats, type distributions, legendary status, evolution power gains, competitive roles,
and more. It is structured as a pipeline of 5 scripts that build on each other, starting
from raw data and ending with machine learning models and competitive team-building insights.

---

## Dataset

| Property | Value |
|----------|-------|
| Source | `pokemon.csv` |
| Pokemon | 801 (Generations 1–7) |
| Columns | 41 original + 10 engineered features |
| Legendary | 70 (8.7%) |
| Dual-type | 417 (52.1%) |
| Types covered | 18 primary types |
| BST range | 180 (Sunkern) → 780 (Mewtwo / Rayquaza) |
| Avg BST | 428.4 |

### Columns include:
- **Stats:** `hp`, `attack`, `defense`, `sp_attack`, `sp_defense`, `speed`, `base_total`
- **Type:** `type1`, `type2`
- **Meta:** `generation`, `is_legendary`, `capture_rate`, `base_happiness`, `base_egg_steps`
- **Physical:** `height_m`, `weight_kg`, `percentage_male`
- **Type matchups:** 18 `against_*` columns (damage multipliers)
- **Engineered:** `offensive_score`, `defensive_score`, `survivability`, `stat_variance`,
  `speed_tier`, `catch_tier`, `gender_cat`, `is_dual_type`, `phys_ratio`, `density`

---

---

## Setup & Installation

### Requirements

- Python 3.8+
- pip

### Install dependencies

```bash
pip install pandas numpy matplotlib seaborn scikit-learn plotly jupyter
```

### Clone the repository

```bash
git clone https://github.com/srdave3/pokemon-analysis.git
cd pokemon-analysis
```

---

## How to Run

> Scripts must be run **in order** — each one depends on the output of the previous.

```bash
# Step 1 — Clean the data and engineer features (generates pokemon_clean.csv)
python 01_data_cleaning.py

# Step 2 — Run exploratory analysis (prints 20 insight sections to console)
python 02_eda.py

# Step 3 — Generate 13 static charts (saved to charts/ folder)
python 03_visualizations.py

# Step 4 — Generate 5 interactive HTML charts (requires plotly, opens in browser)
python 04_interactive.py

# Step 5 — Run ML clustering, PCA, and Random Forest classification
python 05_machine_learning.py
```

To run the full analysis in one place, open the Jupyter notebook:

```bash
jupyter notebook pokemon_analysis.ipynb
```

Then run cells from top to bottom using **Shift + Enter**.

---

## Scripts Breakdown

### `01_data_cleaning.py`
Loads `pokemon.csv`, handles all missing values, and engineers new features.

**Cleaning steps:**
- `type2` — 384 nulls filled with `"None"` (single-type Pokemon)
- `capture_rate` — converted from string to numeric
- `height_m` / `weight_kg` — 20 nulls each filled with median
- `percentage_male` — 98 nulls (genderless Pokemon) filled with `-1` sentinel

**Engineered features added:**

| Feature | Description |
|---------|-------------|
| `offensive_score` | Average of attack, sp_attack, speed |
| `defensive_score` | Average of hp, defense, sp_defense |
| `survivability` | Geometric mean of hp, defense, sp_defense |
| `stat_variance` | Variance across 6 stats (0 = perfectly balanced) |
| `speed_tier` | Categorical: Very Slow / Slow / Medium / Fast / Very Fast / Extreme |
| `catch_tier` | Categorical: Easy / Medium / Hard / Legendary-tier |
| `gender_cat` | Genderless / Male-only / Female-only / Mixed |
| `is_dual_type` | 1 if Pokemon has a secondary type, else 0 |
| `phys_ratio` | Attack / (Attack + Sp. Attack) — physical vs special leaning |
| `density` | Weight / Height² — body density proxy |

---

### `02_eda.py`
Prints 20 analytical sections to the console covering:

1. Dataset overview
2. Overall stat summary
3. Avg BST by generation
4. Avg BST by primary type
5. Legendary vs non-legendary stat comparison
6. Top 15 Pokemon by BST
7. Most common type combinations
8. Capture rate analysis
9. Dual-type rate by generation
10. Speed tier distribution
11. Catch difficulty vs avg BST
12. Biggest BST gains on evolution
13. Hidden gems (high BST, easy to catch)
14. Top offensive pressure (non-legendary)
15. Top defensive survivability (non-legendary)
16. Most balanced vs most specialized Pokemon
17. Avg BST by gender category
18. Strongest dual-type combinations
19. Stat record holders (highest/lowest per stat)
20. Experience growth type vs avg BST

---

### `03_visualizations.py`
Generates 13 PNG charts saved to `charts/`:

| # | Chart | Description |
|---|-------|-------------|
| 01 | BST distribution | Histogram + KDE by legendary status |
| 02 | BST by generation | Bar chart with overall average line |
| 03 | BST by type | Horizontal bar, color-coded by strength |
| 04 | Legendary radar | Polar chart comparing all 6 stats |
| 05 | Type counts | Pokemon count per primary type |
| 06 | Type matchup heatmap | 18×18 damage multiplier matrix |
| 07 | Correlation heatmap | Stat + meta feature correlations |
| 08 | BST boxplot | Distribution spread per generation |
| 09 | Evolution jumps | Top 12 BST gains on evolution |
| 10 | Offensive vs defensive | Scatter plot with quadrant lines |
| 11 | Speed tiers | Count + avg BST per speed tier |
| 12 | Catch difficulty | Boxplot of BST per catch tier |
| 13 | Stat specialization | Variance vs total power scatter |

---

### `04_interactive.py`
Generates 5 interactive HTML files saved to `charts/interactive/`:

| # | Chart | Description |
|---|-------|-------------|
| 01 | Attack scatter | Attack vs Sp. Attack, hover reveals full stats |
| 02 | BST by generation | Grouped boxplot, toggle legendary status |
| 03 | Type radar | Overlaid radar for all 18 types |
| 04 | Parallel coordinates | All 6 stats as parallel axes, colored by type |
| 05 | Defense vs speed | Scatter with median quadrant lines |

> Requires `pip install plotly`

---

### `05_machine_learning.py`

**Part A — KMeans Clustering (k=4)**

Groups Pokemon by stat profile using the elbow method to select k.

| Cluster | Name | Avg BST | Dominant trait |
|---------|------|---------|----------------|
| 0 | Elite Powerhouses | 589 | High across all stats |
| 1 | Defensive Tanks | 470 | High defense, low speed |
| 2 | Speedsters | 469 | High speed, balanced offence |
| 3 | Balanced / Weak | 300 | Low across all stats |

**Part B — PCA Visualization**

Reduces 6 stat dimensions to 2 principal components.
PC1 explains 44.8% of variance, PC2 explains 18.2%.

**Part C — Random Forest Classifier**

Predicts legendary status from stats and meta features.

- Test accuracy: **100%** on held-out set
- 5-fold CV F1: **0.949 ± 0.054**
- Top predictors: `base_egg_steps`, `base_total`, `capture_rate`

**Part D — Enriched Cluster Profiles**

Each cluster profiled with offensive score, defensive score, survivability,
legendary %, dual-type %, dominant type, and most common speed tier.

**Part E — Competitive Role Identification**

| Role | Criteria |
|------|----------|
| Glass Cannon | Offensive score > 120, defensive score < 85 |
| Wall / Tank | Defensive score > 115, offensive score < 75 |
| Balanced Fighter | All 6 individual stats > 80 |
| Physical Sweeper | Attack ≥ 120, Speed ≥ 90 |
| Special Sweeper | Sp. Attack ≥ 110, Speed ≥ 90 |

---

## Key Insights

- **Dragon/Flying** is the strongest dual-type combination with avg BST of 667.5
- **Magikarp → Gyarados** is the biggest evolution BST jump in the game at +440
- **Capture rate is a power proxy** — each difficulty tier differs by ~90 BST on average
- **Genderless Pokemon** average 552 BST vs 408 for mixed-gender; 64% are legendary
- **36% of all Pokemon** (286/801) have speed below 50 — speed is the most differentiating stat
- **Gen 4** introduced the most Pokemon with BST ≥ 500 (44 Pokemon, avg 566)
- **Shuckle** holds both the highest defense (230) and sp_defense (230) AND the lowest speed (5) — the most extreme specialist in the game
- **Blissey** has the highest HP (255) of any Pokemon in the entire dex
- Pokemon requiring **1,250,000 exp** to reach level 100 average 515 BST and are 39% legendary
- The happiness paradox: base happiness correlates **negatively** (−0.275) with BST — the most powerful Pokemon are the unhappiest

---

## Charts Gallery

All charts are in the `charts/` folder and render directly on GitHub when clicked.

| File | What it shows |
|------|---------------|
| `01_bst_distribution.png` | Legendaries cluster tightly above 580 BST |
| `04_legendary_radar.png` | Legendaries outperform in every single stat |
| `06_type_matchup_heatmap.png` | Full 18×18 type effectiveness matrix |
| `09_evolution_jumps.png` | Magikarp's +440 jump dwarfs everything else |
| `10_offensive_vs_defensive.png` | Four clear quadrants — sweepers, walls, tanks, cannons |
| `11_speed_tiers.png` | The speed gap between tiers and its BST correlation |
| `14_cluster_role_scatter.png` | ML clusters mapped onto the offensive vs defensive plane |

---

## Machine Learning Results

### KMeans Cluster Summary

| Cluster | Count | Avg BST | Avg Offensive | Avg Defensive | Legendary % |
|---------|-------|---------|---------------|---------------|-------------|
| Elite Powerhouses | 142 | 589.4 | 100.8 | 95.7 | High |
| Defensive Tanks | 176 | 470.5 | 68.7 | 89.8 | Low |
| Speedsters | 188 | 468.7 | 86.4 | 69.7 | Low |
| Balanced / Weak | 295 | 300.0 | 50.4 | 49.4 | None |

### Random Forest — Top Predictive Features

| Rank | Feature | Importance |
|------|---------|------------|
| 1 | base_egg_steps | 0.269 |
| 2 | base_total | 0.218 |
| 3 | capture_rate | 0.157 |
| 4 | experience_growth | 0.119 |
| 5 | base_happiness | 0.051 |
| 6 | sp_attack | 0.044 |
| 7 | hp | 0.037 |

---

## Technologies Used

| Library | Purpose |
|---------|---------|
| pandas | Data manipulation and analysis |
| numpy | Numerical operations |
| matplotlib | Static chart generation |
| seaborn | Statistical visualizations and heatmaps |
| plotly | Interactive HTML charts |
| scikit-learn | KMeans clustering, PCA, Random Forest, StandardScaler |
| jupyter | Notebook environment |

---

*Dataset covers Pokemon from Generation 1 (Bulbasaur) through Generation 7 (Marshadow) — 801 Pokemon total.*
