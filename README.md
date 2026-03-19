# Specific Strength Prediction with Bayesian Optimization and CBFV

This repository contains a full pipeline for predicting the specific strength of materials ($\text{MPa}\cdot\text{cm}^3\cdot\text{g}^{-1}$) using their chemical formula and temperature. It applies Composition-Based Feature Vectors (CBFV) and machine learning models, and constructs the uncertainty estimations required for Bayesian Optimization.

## Overview

The Jupyter Notebook (`Project_BO_final.ipynb`) demonstrates how to:
1. **Load Data**: Feed chemical formulas and corresponding temperatures from an excel dataset (`new_dataset.xlsx`).
2. **Feature Engineering**: Generate features directly from chemical formulas using the `CBFV` (Composition-Based Feature Vector) package with **Magpie** elemental properties.
3. **Data Preprocessing**: Standardize features using typical `StandardScaler` transformations.
4. **Machine Learning Model**: Train a `GradientBoostingRegressor`, optimizing its hyperparameters (learning rate, max depth, min samples split, n estimators) using `GridSearchCV`.
5. **Model Evaluation**: Evaluate models by computing R-squared and Root Mean Squared Error (RMSE) metrics, validating on hold-out test sets. Graphically assess predicted vs. actual values.
6. **Uncertainty Estimation & Bayesian Optimization Formulation**: Define a custom `bootstrap_estimator` to approximate prediction uncertainty (mean and standard deviation). A function for `Expected_Improvement` is also defined to guide future experimental design through Bayesian Optimization.

## Results & Discussion

The `GradientBoostingRegressor` achieved an **$R^2$ score of 0.826** and an **RMSE of 29.84** on the test dataset after hyperparameter tuning. It successfully highlighted lightweight, refractory High-Entropy Alloys (like `Al0.5Mo0.5NbTa0.5TiZr`) as optimal targets. Through the bootstrap-estimated Expected Improvement rankings, this AI-guided pipeline actively suggests which new formulas to synthesize next based on uncertainty and predicted strength! 

For an in-depth breakdown of the metrics and material science interpretation, please check out the **[Results Analysis Document](RESULTS_ANALYSIS.md)**!

## File Structure

- `Project_BO_final.ipynb`: The main Jupyter Notebook containing the end-to-end Python code for ML modeling, hyperparameter tuning, and expected improvement calculations.
- `RESULTS_ANALYSIS.md`: A detailed report discussing model performance, elemental features, and findings.
- `requirements.txt`: List of required Python packages.
- `new_dataset.xlsx`: The dataset that contains the formula, Temperature, and target columns.

## Installation & Setup

1. Clone this repository.
2. Install the necessary dependencies into your python environment:
   ```bash
   pip install -r requirements.txt
   ```
3. Open `Project_BO_final.ipynb` in a Jupyter Notebook environment (or Google Colab) and run the cells.

## Libraries and Tools Used

- **Feature engineering**: `CBFV`
- **Machine Learning**: `scikit-learn`
- **Mathematics & Tuning**: `scipy`, `numpy`
- **Data Wrangling**: `pandas`
- **Visualization**: `matplotlib`, `seaborn`
