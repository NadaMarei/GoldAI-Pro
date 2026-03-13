# Gold Price Prediction using Random Forest with SHAP Explanations

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![scikit-learn](https://img.shields.io/badge/scikit--learn-1.0+-orange)](https://scikit-learn.org/)
[![SHAP](https://img.shields.io/badge/SHAP-0.41.0-green)](https://shap.readthedocs.io/)

## 📌 Overview

This project implements a **Random Forest Regressor** to predict gold prices based on historical market data. Beyond model training and evaluation, it integrates **SHAP (SHapley Additive exPlanations)** to provide interpretable insights into feature contributions, making the model’s predictions transparent and trustworthy. The code includes comprehensive exploratory data analysis (EDA), data preprocessing, model evaluation, and multiple visualisation techniques.

The goal is to demonstrate a complete machine learning pipeline—from raw data to explainable predictions—suitable for financial time series forecasting.

## 📑 Table of Contents

- [Features](#features)
- [Dataset](#dataset)
- [Installation](#installation)
- [Usage](#usage)
- [Methodology](#methodology)
  - [Data Preprocessing](#data-preprocessing)
  - [Exploratory Data Analysis](#exploratory-data-analysis)
  - [Model Training](#model-training)
  - [Evaluation](#evaluation)
  - [Feature Importance](#feature-importance)
  - [SHAP Explanations](#shap-explanations)
- [Results](#results)
- [Project Structure](#project-structure)
- [Dependencies](#dependencies)
- [License](#license)
- [Acknowledgments](#acknowledgments)

## ✨ Features

- **Data Cleaning**: Handles date conversion, comma removal, volume abbreviations (K, M), and percentage formatting.
- **Exploratory Data Analysis**: Correlation heatmaps, distribution plots, time series, boxplots, scatter plots.
- **Machine Learning**: Random Forest Regressor with hyperparameters (`n_estimators=100`) and train/test split.
- **Model Evaluation**: R² score, actual vs. predicted line plot, residual analysis.
- **Feature Importance**: Built-in Gini importance from Random Forest.
- **Model Interpretability**: SHAP explanations including:
  - Summary bar plot (global feature importance)
  - Beeswarm summary plot (distribution of SHAP values)
  - Dependence plot for the most influential feature.

## 📊 Dataset

The dataset used is `Gold Historical Data.csv`, which contains daily gold prices and related metrics. It is assumed to be obtained from sources like Kaggle or Yahoo Finance. The columns typically include:

- **Date**: Trading date (format: `MM/DD/YYYY`).
- **Price**: Closing price (USD), may contain commas.
- **Open**: Opening price (USD).
- **High**: Highest price of the day.
- **Low**: Lowest price of the day.
- **Vol.** : Trading volume, often abbreviated with K (thousands) or M (millions).
- **Change %**: Daily percentage change, may include `%` sign and negative values.

The dataset contains no missing values after preprocessing.

## 💻 Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/gold-price-prediction.git
   cd gold-price-prediction
   ```

2. (Optional) Create and activate a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. Install required packages:
   ```bash
   pip install -r requirements.txt
   ```

   If you don't have a `requirements.txt`, install manually:
   ```bash
   pip install numpy pandas seaborn matplotlib scikit-learn shap
   ```

## 🚀 Usage

1. Ensure the dataset file `Gold Historical Data.csv` is placed in the project root directory.

2. Run the main script:
   ```bash
   python gold_price_prediction.py
   ```

3. The script will:
   - Print dataset info and statistics.
   - Display multiple plots (EDA, model evaluation, SHAP) – they will appear sequentially; close each window to proceed.
   - Output the R² score and SHAP analysis summary in the console.

All generated plots are saved in memory only; you can modify the code to save them as image files if desired.

## 🔬 Methodology

### Data Preprocessing

- **Date**: Parsed to `datetime` using `pd.to_datetime(format='%m/%d/%Y')`.
- **Price columns** (`Price`, `Open`, `High`, `Low`): Converted to float after removing commas.
- **Volume**: Strings like `130.13K` and `1.2M` are converted to numeric by replacing `K` with `e3` (×1000) and `M` with `e6` (×1,000,000).
- **Change %**: Stripped of `%` sign and cast to float (negative values preserved).

### Exploratory Data Analysis

- **Correlation Heatmap**: Visualises relationships among numeric features.
- **Gold Price Distribution**: Histogram with KDE.
- **Time Series Plot**: Gold price over the entire date range.
- **Boxplots**: Distribution of `Price`, `Open`, `High`, `Low`.
- **Scatter Plot**: Volume vs. Price to detect potential correlation.

### Model Training

- Features (`X`): All columns except `Date` and `Price`.
- Target (`Y`): `Price`.
- Split: 80% training, 20% testing (`random_state=2` for reproducibility).
- Algorithm: `RandomForestRegressor` with 100 trees (default hyperparameters).

### Evaluation

- **R² Score**: Measures the proportion of variance explained by the model.
- **Actual vs. Predicted Plot**: Compares true and predicted values across the test set.
- **Residuals Plot**: Scatter of residuals (actual – predicted) vs. predicted values to check for homoscedasticity.

### Feature Importance

- **Gini Importance**: Extracted from the trained Random Forest model and displayed as a bar chart.

### SHAP Explanations

- **TreeExplainer**: SHAP explainer tailored for tree-based models.
- **SHAP Values**: Computed for the test set.
- **Summary Bar Plot**: Mean absolute SHAP values per feature – global importance.
- **Beeswarm Plot**: Distribution of SHAP values for each feature, showing direction and magnitude of impact.
- **Dependence Plot**: For the top feature (e.g., `Open`), illustrates how its SHAP value varies with the feature value, often revealing non-linear relationships.

SHAP provides both local (per-prediction) and global interpretability, making the model’s decisions transparent.

## 📈 Results

- **R² Score**: Approximately **0.999** (exact value printed during execution). This indicates that the model captures almost all variance in gold prices, which is expected given the high correlation among price fields (`Open`, `High`, `Low`).
- **Feature Importance**: Gini importance and SHAP both rank `Open` as the most influential feature, followed by `High` and `Low`.
- **SHAP Insights**: The beeswarm plot confirms that higher `Open` values push predictions higher, and lower values push them lower, with a roughly linear relationship.

*Note*: The extremely high R² is typical when predicting closing price using opening/high/low prices; the model essentially learns the intrinsic relationships among these strongly correlated variables. For a more challenging task, one could predict future prices using lagged features.

## 📁 Project Structure

```
gold-price-prediction/
│
├── gold_price_prediction.py   # Main script
├── Gold Historical Data.csv    # Dataset (not included in repo)
├── requirements.txt            # Python dependencies
├── README.md                   # This file
└── images/                     # (Optional) Saved plots
```

## 🧪 Dependencies

- Python 3.8+
- numpy
- pandas
- seaborn
- matplotlib
- scikit-learn
- shap

Install all with: `pip install -r requirements.txt`

## 📄 License

This project is licensed under the MIT License – see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Dataset source: [Kaggle – Gold Price Historical Data](https://www.kaggle.com/datasets) (example).
- SHAP library: [SHAP documentation](https://shap.readthedocs.io/).
- Inspired by various gold price prediction tutorials and explainable AI practices.
