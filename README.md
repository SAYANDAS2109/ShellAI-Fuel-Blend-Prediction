# Shell.ai Hackathon 2025 — Fuel Blend Properties Prediction

## Overview

This repository contains a machine learning solution developed for the Shell.ai Hackathon for Sustainable and Affordable Energy 2025 — Fuel Blend Properties Prediction Challenge.

The objective of the challenge is to predict the final properties of complex fuel blends using the composition and properties of their constituent components.

The problem involves nonlinear relationships and interactions between component fractions and their properties.

## Problem Statement

The challenge provides information about five base fuel components and their respective properties. The task is to use this information to predict ten final properties of the resulting fuel blend.

The input data contains 55 features:

- 5 component fractions
- 50 component-property features
- 10 properties for each of the 5 components

The model predicts:

- BlendProperty1
- BlendProperty2
- BlendProperty3
- BlendProperty4
- BlendProperty5
- BlendProperty6
- BlendProperty7
- BlendProperty8
- BlendProperty9
- BlendProperty10

The dataset contains:

- Training samples: 2,000
- Test samples: 500
- Input features: 55
- Target properties: 10

## Project Objective

The main objective is to develop a machine learning model that learns the relationship between fuel component composition, component properties, and the final properties of the resulting fuel blend.

The overall process can be represented as:
```
Component Composition + Component Properties
                    |
                    v
            Machine Learning Model
                    |
                    v
          Final Blend Properties
```
## Machine Learning Approach

Several machine learning approaches were explored during development:

1. XGBoost
2. LightGBM
3. XGBoost + LightGBM Ensemble

The models were evaluated using Mean Absolute Percentage Error (MAPE).

After comparison, LightGBM achieved the best local validation performance among the tested approaches and was selected as the final model.

## Model Development Workflow

```text
Training Data
     |
     v
Data Loading & Inspection
     |
     v
Feature / Target Separation
     |
     v
Train–Validation Split
     |
     v
Model Experiments
     |
     +------------------+------------------+
     |                  |                  |
     v                  v                  v
  XGBoost           LightGBM        XGBoost + LightGBM
     |                  |                  |
     +------------------+------------------+
                        |
                        v
                 MAPE Evaluation
                        |
                        v
                 Model Comparison
                        |
                        v
                 LightGBM Selected
                        |
                        v
          Train on Complete Dataset
                        |
                        v
                Predict Test Data
                        |
                        v
             Generate 10 Properties
                        |
                        v
                 submission.csv 
```

## Data Processing

The training dataset was separated into input features and target variables.

### Input Features

The 55 input features consist of:

- Component composition fractions
- Component-specific property values

### Target Variables

The ten target variables are:

- BlendProperty1
- BlendProperty2
- BlendProperty3
- BlendProperty4
- BlendProperty5
- BlendProperty6
- BlendProperty7
- BlendProperty8
- BlendProperty9
- BlendProperty10

The ID column in the test dataset is retained for the final submission but is not used as a model input feature.

## Models Tested

### XGBoost

XGBoost was used as a gradient-boosting baseline for the fuel blend prediction task.

The tuned XGBoost model achieved an overall local validation MAPE of approximately 2.6126.

### LightGBM

LightGBM was evaluated as another gradient-boosting approach.

The selected configuration was:

LGBMRegressor(
    n_estimators=500,
    learning_rate=0.03,
    max_depth=6,
    num_leaves=31,
    subsample=0.8,
    colsample_bytree=0.8,
    random_state=42,
    n_jobs=-1,
    verbosity=-1
)

A separate LightGBM regression model was trained for each of the ten target properties.

The overall local validation MAPE achieved was approximately 1.1377.

### XGBoost + LightGBM Ensemble

An ensemble combining XGBoost and LightGBM predictions was also tested.

The tested ensemble did not improve upon the selected LightGBM model, so LightGBM was used as the final model.

## Final Model

The final solution uses 10 independent LightGBM regression models.

Each model predicts one of the ten blend properties.

Input Features
      |
      +-----------------------------+
      |                             |
      v                             v
   LightGBM                     LightGBM
   Model 1                      Model 2
      |                             |
      v                             v
BlendProperty1                BlendProperty2
      |
      |
     ...
      |
      v
   LightGBM
   Model 10
      |
      v
BlendProperty10

The models were first evaluated using a train-validation split.

After selecting the final configuration, all ten models were retrained using the complete training dataset of 2,000 samples.

The final models were then used to generate predictions for the 500 test samples.

## Training and Validation

The training data was divided into training and validation subsets using:

train_test_split(
    X_train,
    y_train,
    test_size=0.2,
    random_state=42
)

This resulted in:

- Training samples: 1,600
- Validation samples: 400

The validation set was used to compare the different models and configurations.

After selecting LightGBM, the final models were trained using the complete training dataset.

## Evaluation Metric

The competition uses Mean Absolute Percentage Error (MAPE) as the evaluation metric.

The general formulation is:

MAPE = Mean(|Actual - Predicted| / |Actual|) × 100

Lower MAPE represents lower percentage prediction error.

The best local validation MAPE obtained during development was approximately:

1.1377

This is a local validation result and not the official competition leaderboard score.

## Validation Results

| Target | MAPE |
|---|---:|
| BlendProperty1 | 1.4986 |
| BlendProperty2 | 1.5079 |
| BlendProperty3 | 1.0631 |
| BlendProperty4 | 0.8297 |
| BlendProperty5 | 0.3118 |
| BlendProperty6 | 1.0580 |
| BlendProperty7 | 0.7345 |
| BlendProperty8 | 2.0155 |
| BlendProperty9 | 1.8301 |
| BlendProperty10 | 0.5281 |
| Overall | 1.1377 |

## Final Prediction Process

The final prediction pipeline follows these steps:

1. Load train.csv
2. Separate input features and target variables
3. Train ten LightGBM models
4. Load test.csv
5. Remove the ID column from model inputs
6. Generate predictions for all ten target properties
7. Combine the predictions
8. Match the required sample submission structure
9. Save the final predictions as submission.csv

The final prediction matrix has the shape:

500 × 10

This represents:

- 500 test samples
- 10 predicted blend properties

## Dataset

### train.csv

Contains:

- 2,000 training samples
- 55 input features
- 10 target properties

### test.csv

Contains:

- 500 unseen samples
- 55 input features
- Hidden target values

The actual target values of the test dataset are not available locally.

### sample_solution.csv

Provides the required structure for the competition submission.

### submission.csv

Contains the final predictions generated by the ten LightGBM models for all 500 test samples.

## Repository Structure

ShellAI-Fuel-Blend-Prediction/
|
├── README.md
├── ShellAI_Fuel_Blend_Prediction.ipynb
├── requirements.txt
├── train.csv
├── test.csv
├── sample_solution.csv
├── submission.csv
└── .gitignore

If the competition rules restrict redistribution of competition datasets, the CSV files should not be uploaded to a public GitHub repository. In that case, users should obtain the datasets directly from the competition platform.

## Technologies Used

- Python
- Pandas
- NumPy
- Scikit-learn
- XGBoost
- LightGBM
- Matplotlib
- Seaborn
- Jupyter Notebook
- Google Colab

## Requirements

The main Python dependencies used in this project are:

pandas>=2.0
numpy>=1.24
scikit-learn>=1.3
xgboost>=2.0
lightgbm>=4.0
matplotlib>=3.7
seaborn>=0.12
jupyter>=1.0

## Installation

Clone the repository:

git clone <repository-url>

cd ShellAI-Fuel-Blend-Prediction

Install the required dependencies:

pip install -r requirements.txt

## Running the Project

Open the following notebook:

ShellAI_Fuel_Blend_Prediction.ipynb

The notebook contains the complete workflow, including:

1. Data loading
2. Data inspection
3. Feature and target separation
4. Train-validation split
5. XGBoost training
6. LightGBM training
7. Model comparison
8. MAPE evaluation
9. Final LightGBM training
10. Test-set prediction
11. Submission generation

The notebook can be executed using Jupyter Notebook, JupyterLab, or Google Colab.

## Key Findings

The experiments showed that gradient-boosting models can effectively capture relationships between fuel component properties and the resulting blend properties.

Key findings from the experiments include:

- XGBoost provided a strong baseline.
- LightGBM achieved lower validation MAPE than the tested XGBoost configuration.
- The tested XGBoost-LightGBM ensemble did not improve the validation result.
- Separate models were trained for the ten target properties.
- LightGBM was selected as the final model.
- The final LightGBM models were retrained using the complete training dataset.
- Predictions were generated for all 500 test samples.

## Final Result

The final LightGBM solution achieved a local validation MAPE of approximately:

1.1377

The final trained models generated predictions for all 500 test samples.

The predictions were saved in:

submission.csv

## Limitations

The reported MAPE is based on a local validation split.

The actual target values for the competition test dataset are hidden, so the true test-set MAPE cannot be calculated locally.

Therefore:

Local Validation MAPE != Official Competition MAPE

The official performance is determined by the competition evaluation system.

## Disclaimer

This repository represents a machine learning project developed for the Shell.ai Hackathon for Sustainable and Affordable Energy 2025 — Fuel Blend Properties Prediction Challenge.

The validation results shown in this repository are experimental results obtained during model development and should not be interpreted as an official competition leaderboard score or ranking.

## Author

Sayan Das

Petroleum Engineering

## Acknowledgements

This project was developed as part of the Shell.ai Hackathon for Sustainable and Affordable Energy 2025 — Fuel Blend Properties Prediction Challenge.
