# Predicting Diabetes from Survey Data: An Analysis of the BRFSS 2015 Health Indicators

A term project for FAU's Introduction to Data Science Analytics (CAP5768), analyzing the [CDC Diabetes Health Indicators dataset](https://archive.ics.uci.edu/dataset/891/cdc+diabetes+health+indicators), which is drawn from the CDC's 2015 Behavioral Risk Factor Surveillance System (BRFSS) survey.

**Authors:** Sydney Durrance and Jon Krenick

## Overview

Clinical diagnosis of diabetes requires lab work that not everyone can access or afford. This project asks whether self-reported survey data alone can flag people likely to have diabetes, which could serve as a low-cost first-pass screening tool. The full write-up, methodology, and results are in [`CAP5768_DataScience_FinalProjectReport.pdf`](./CAP5768_DataScience_FinalProjectReport.pdf).

**Research question:** Which self-reported health indicators, behaviors, and demographics are associated with diabetes status, and how well can those indicators alone predict whether someone has diabetes?

## Dataset

- 253,680 survey responses, 21 predictors plus a binary diabetes target
- After preprocessing (deduplication, type downcasting): 229,474 rows
- Class imbalance: 84.7% no diabetes, 15.3% diabetes

## Project Structure

| File | Contents |
|---|---|
| `TermProject_Part1.ipynb` | Data cleaning, preprocessing, and exploratory data analysis |
| `TermProject_Part2.ipynb` | Hypothesis testing (t-tests, chi-square) and OLS linear regression |
| `TermProject_Part3.ipynb` | Classification: model comparison and regularized logistic regression |
| `TermProject_Part4.ipynb` | Dimensionality reduction (PCA) and K-Means clustering |
| `CAP5768_DataScience_FinalProjectReport.pdf` | Full written report with methodology, tables, and figures |

## Methods

**Data preparation:** Type downcasting (42.6 MB to 6.0 MB), duplicate removal (24,206 rows dropped), and engineering of 8 new labeled variables for readability (e.g., BMI categories, age groups).

**Hypothesis testing:** Two-sample t-tests (BMI by diabetes status, BMI by high blood pressure) and chi-square tests of independence (diabetes by sex, diabetes by stroke history). Two OLS linear regression models examined physical activity and fruit consumption as predictors of BMI, controlling for demographic and socioeconomic variables.

**Classification:** Compared four models via 5-fold cross-validation, SVC, Decision Tree, Random Forest, and Gradient Boosting, on a randomly undersampled, balanced training set (test set retained the natural 85/15 split). Gradient Boosting performed best in cross-validation. Also fit L1, L2, and Elastic Net regularized logistic regression to compare against the tree-based models and identify the most informative predictors.

**Unsupervised learning:** Standardized the feature set and applied PCA to examine dimensionality, then ran K-Means clustering (k selected via the elbow method) to segment respondents into health-risk profiles.

## Key Results

| Model | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| Gradient Boosting | 70.8% | 31.6% | 76.8% | 44.8% |
| Logistic Regression (L1) | 71.6% | 32.1% | 75.3% | 45.0% |
| Logistic Regression (L2) | 71.5% | 32.0% | 75.3% | 45.0% |
| Logistic Regression (Elastic Net) | 71.3% | 31.3% | 72.1% | 43.7% |

All four hypotheses were statistically supported at α = 0.05. The strongest and most consistent predictors of diabetes across every method (correlation, regularization, and clustering) were `GeneralHealth`, `HighBloodPressure`, `HighCholesterol`, and `HeartDiseaseOrAttack`. Model choice had little effect on performance; the class imbalance and undersampling strategy mattered far more than which algorithm was used.

## Tools

Python, pandas, scikit-learn, scipy, statsmodels, matplotlib, Jupyter Notebook

## Limitations

All data is self-reported (subject to recall and social desirability bias), the design is cross-sectional (no causal claims), and undersampling discarded a large share of majority-class training rows. See the full report for a complete discussion.
