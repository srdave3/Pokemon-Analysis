# Pokemon Data Analysis

End-to-end analysis of 801 Pokemon across Generations 1–7.

## Contents
| File | Description |
|------|-------------|
| `pokemon_analysis.ipynb` | Main notebook — full analysis |
| `pokemon.csv` | Raw dataset (801 Pokemon, 41 columns) |
| `pokemon_clean.csv` | Cleaned dataset with engineered features |
| `01_data_cleaning.py` | Standalone cleaning script |
| `02_eda.py` | Standalone EDA script (20 insights) |
| `03_visualizations.py` | 13 static charts |
| `04_interactive.py` | 5 interactive Plotly charts |
| `05_machine_learning.py` | KMeans, PCA, Random Forest |
| `charts/` | All generated chart PNGs |

## Setup
```bash
pip install pandas numpy matplotlib seaborn scikit-learn plotly jupyter
jupyter notebook pokemon_analysis.ipynb
```

## Key Insights
- Dragon/Flying is the strongest dual-type combination (avg BST 667)
- Magikarp → Gyarados has the biggest evolution jump (+440 BST)
- Capture rate is a near-linear proxy for power (~90 BST gap per tier)
- Genderless Pokemon average 552 BST vs 408 for mixed-gender
