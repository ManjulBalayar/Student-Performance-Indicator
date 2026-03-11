# Student Performance Predictor

An end-to-end ML project that predicts a student's math score based on demographic and academic factors. Built to practice the full cycle from raw data to a deployed web app.

## What it does

Takes in the following inputs:
- Gender, race/ethnicity, parental level of education
- Lunch type, test preparation course completion
- Reading and writing scores

And predicts a **math score** using the best-performing regression model from a trained pool.

## How it's built

**Data pipeline**

Raw CSV → train/test split (80/20) → separate preprocessing pipelines for numerical and categorical features. Numerical features use median imputation + standard scaling. Categorical features use most-frequent imputation + one-hot encoding + scaling. The fitted preprocessor is serialized to `artifacts/preprocessor.pkl`.

**Model selection**

Seven models are trained with grid search cross-validation:
- Linear Regression
- Decision Tree
- Random Forest
- Gradient Boosting
- XGBoost
- CatBoost
- AdaBoost

The model with the highest R² score on the test set is saved to `artifacts/model.pkl`. Training is rejected if no model clears R² > 0.6.

**Web app**

Flask app with a form-based UI. Submitting the form runs the saved preprocessor + model and returns the predicted score.

**Deployment**

Configured for AWS Elastic Beanstalk via `.ebextensions/python.config` (WSGI entry point: `application:application`).

## Stack

Python · pandas · scikit-learn · XGBoost · CatBoost · Flask · AWS Elastic Beanstalk
