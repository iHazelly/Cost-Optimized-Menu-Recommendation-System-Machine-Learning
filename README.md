# Cost‑Optimized Menu Recommendation System using Machine Learning
Paradorn Khanongsuwan (Hazell)
This project forecasts daily prices of 13 key agricultural commodities (e.g., chicken, pork, chili, lime) seven days ahead, then calculates the total cost of 20 popular Thai dishes and recommends the most cost‑efficient menu. The system helps consumers and small food vendors plan weekly budgets under volatile market conditions.

---

## Overview

- **Goal**: Predict ingredient prices → compute recipe costs → rank menus by total cost.
- **Time span**: 5 years (2021–2025), daily granularity.
- **Forecast horizon**: 7 days (walk‑forward validation).
- **Target ingredient** (for evaluation): Coriander (`Price_coriander`).
- **Final output**: Dynamic ranking of 20 Thai menus based on predicted costs.

---

## Data Sources

| Category | Source | Features |
|----------|--------|----------|
| Agricultural prices | Department of Internal Trade (Thailand) | Daily average retail prices of 13 ingredients (THB/kg) |
| Weather | Open‑Meteo API (4 provinces) | Temperature, rainfall, humidity, soil moisture |
| Diesel price | Bangchak Corporation | Daily high‑speed diesel price (THB/litre) |
| Holidays / festivals | Custom calendar | Binary flag for major Thai cultural events |
| Recipes | Manual collection (20 menus) | Ingredient quantities (converted to kg) |

All data were merged into a single time‑series dataset (`final_df.csv`) with 1,827 rows and 36 columns.

---

## Methodology

### Feature Engineering
- Averaged weather variables across 4 provinces to reduce multicollinearity.
- Extracted `Day`, `Month`, `Year`, `DayOfWeek_Num` from dates.
- Created `Important_Days` binary flag (festivals/holidays).
- Standardised recipe units to kilograms for direct cost calculation.

### Models
- **Random Forest Regressor** (with hyperparameter tuning via `GridSearchCV` + `TimeSeriesSplit`)
- **SimpleRNN**
- **LSTM**

### Validation Strategy
- **Walk‑Forward Validation** (retrain every 7 days) – simulates real‑world deployment.
- Training: first 80% of data (2021–2024)  
  Testing: last 20% (2025)

### Evaluation Metrics
- MAE (Mean Absolute Error)
- RMSE (Root Mean Squared Error)
- MAPE (Mean Absolute Percentage Error)

---

## Results

| Model          | MAE (THB/kg) | RMSE (THB/kg) | MAPE (%) |
|----------------|--------------|---------------|-----------|
| Random Forest  | **19.55**    | 28.48         | **17.22** |
| SimpleRNN      | 25.31        | 35.61         | 22.65     |
| LSTM           | 23.35        | 33.93         | 20.73     |

**Key finding:** Random Forest outperforms deep learning models because tree‑based algorithms handle abrupt price spikes better than RNN/LSTM (which tend to oversmooth).

---

## Project Structure 
Cost-Optimized-Menu-Recommendation-System/
├── data/ # Raw & processed data (not included in repo)
├── notebooks/
│ ├── 01_eda.ipynb
│ ├── 02_feature_engineering.ipynb
│ └── 03_model_training_evaluation.ipynb
├── models/ # Saved model weights (optional)
├── docker/ # Dockerfile & deployment files
├── reports/ # PDF report & figures
├── requirements.txt
├── README.md
└── .gitignore


---

## How to Run

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/Cost-Optimized-Menu-Recommendation-System.git
   cd Cost-Optimized-Menu-Recommendation-System

2. **Install dependencies**
    pip install -r requirements.txt

3. **Run the main notebook**
    - Open feature_engineering_and_train_model.ipynb in Jupyter.
    - Execute cells sequentially (data loading, feature engineering, training, evaluation).

## AI Use Declaration
During the development of this project, we used AI tools for:

    - Language translation and sentence refinement

    - Code suggestions, debugging, and structural guidance

    - Writing assistance for the report and this README

    - Brainstorming and conceptual support

However, all core model design, data collection, feature engineering, result interpretation, and final technical decisions were made by the authors (Jirapat Datephanyawat & Paradorn Khanongsuwan), and all outputs have been manually verified.

## Citation
If you use this code or ideas, please cite:

@misc{datephanyawat_khanongsuwan_2025_menu,
  title={Cost‑Optimized Menu Recommendation System using Machine Learning},
  author={Datephanyawat, Jirapat and Khanongsuwan, Paradorn},
  year={2025},
  howpublished={\url{https://github.com/yourusername/Cost-Optimized-Menu-Recommendation-System}}
}
