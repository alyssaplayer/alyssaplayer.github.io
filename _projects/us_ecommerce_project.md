---
layout: page
title: MS Data Science Capstone Project (US-ECommerce)
description: Application of varied machine learning techniques to a US E-Commerce dataset from 2020 to identify key profit drivers. 
img: assets/img/us_ecommerce_projectcard.png
importance: 2
category: machine learning
permalink: /projects/usecommerce/
---



Built supervised and unsupervised machine learning models in Python to predict order-level profit drivers for a US e-commerce retailer, applying feature scaling, hyperparameter tuning, and cluster analysis on real-world transaction data.

<hr>

## Background
This project builds on earlier exploratory work from a prior course, where correlation analysis, PCA, and heatmapping first surfaced Discount and Sales as the strongest predictors of Profit, and identified Office Supplies — not Technology — as the most consistently profitable category despite Technology's higher per-order value. A first-pass Random Forest model (tuned via GridSearchCV) reached a test R² of 0.899 and RMSE of $37.04, establishing that profit was highly predictable from these features and setting up the deeper model comparison below.

## Overview
This project addressed the challenge of identifying profit drivers at the order level for a US online retailer, to help regional sales/operations managers, demand analytics teams, and supply chain leaders optimize inventory allocation and target areas for improvement. KNN and Gradient Boosting were evaluated as supervised regressors against the continuous `Profit_clipped` target, while K-Means, DBSCAN, and HAC were used to test for hidden customer/order segments beyond the labeled features.

## Data & Preprocessing
The dataset is a subset of ~3,300 US sales transactions (Kaggle), spanning 2020 and capturing shifts in online purchasing behavior related to the COVID-19 pandemic. Features include product category, customer segment, region, and ship mode. Feature scaling had a substantial effect on KNN performance, and PCA-based dimensionality reduction was tested but ultimately hurt model performance, so the full feature set was retained.

## Modeling Approach
KNN regression was tuned via GridSearchCV across neighbor count, distance metric, and weighting scheme. Feature scaling improved RMSE by roughly 13% over the unscaled baseline, and the bias-variance tradeoff was plotted across k=1–49 to directly check for overfitting: at k=1, training RMSE was near $0 (the model memorizing individual points) while test RMSE peaked near $90, with test error dipping sharply between k=2–5 — consistent with GridSearchCV's selected k=3.

Gradient Boosting was tuned in two passes: an initial search over 300–500 estimators, extended to 500–700 once RMSE kept improving. The final model used `{learning_rate: 0.1, max_depth: 3, n_estimators: 700, subsample: 0.8}`, with the shallow trees, subsampling, and low learning rate together acting as regularization against overfitting.

## Model Results
| Model                  | Test RMSE | Test R² |
|-------------------------|-----------|---------|
| KNN Regression (k=3)    | $81.48    | 0.512   |
| **Gradient Boosting**   | **$36.81**| **0.9005**|

Gradient Boosting was the strongest model tested, with the extended estimator search showing a genuine performance plateau (RMSE $37.22 → $36.81) rather than continued gains — evidence of real model fit rather than overfitting. This result was reinforced by KNN's own strong performance at a small k=3, consistent with the strong Discount–Sub-category correlation observed during EDA.

## Classification & Baseline Regression
An earlier project phase tested a broader model set to establish baselines and explore profit as a classification problem. OLS regression and its regularized variants (Lasso, Ridge, ElasticNet) plateaued around R² = 0.39 and RMSE ≈ $91 — consistent across all three penalty types, indicating the ceiling was set by feature signal rather than model choice. Reframing profit as a binary high/low classification problem told a different story: logistic regression reached 77.9% accuracy, SVM reached 78%, and a tuned Random Forest classifier reached 90.05% accuracy — the strongest classification result across the whole project. Discount was the single most consistent negative predictor of profit across every model tested, from OLS coefficients through Lasso feature selection to Random Forest importances.

| Model                        | Task           | Result          |
|-------------------------------|----------------|------------------|
| OLS / Lasso / Ridge / ElasticNet | Regression   | R² ≈ 0.39, RMSE ≈ $91 |
| Logistic Regression            | Classification | 77.9% accuracy  |
| SVM (RBF)                      | Classification | 78% accuracy    |
| **Random Forest**              | **Classification** | **90.05% accuracy** |

The gap between weak exact-value prediction (R² 0.39) and strong directional classification (90% accuracy) suggests the data carries a clear high/low profit signal even where precise dollar-value prediction remains hard.

## Clustering Exploration
K-Means, DBSCAN, and HAC were applied to the scaled feature set to look for natural order segments. K-Means' best silhouette score (0.188, k=10) indicated weak cluster separation, and further inspection showed it was largely rediscovering existing product sub-category one-hot flags rather than finding new structure. DBSCAN collapsed to a single cluster with 10.3% noise and failed to find meaningful density-based structure — likely because the high-dimensional, largely one-hot-encoded feature space lacks the density contrast DBSCAN relies on. HAC (average linkage) modestly outperformed K-Means (silhouette 0.245 at k=10), though cluster sizes were highly uneven (4 to 2,375 rows). As with Olist, this is treated as a genuine negative result: no meaningful hidden segments were found beyond the already-labeled features.

## Limitations & Next Steps
The clustering methods largely confirmed that existing labels (like product sub-category) already capture the dominant structure in the data, rather than revealing new segments. Future work would include testing on a larger or more recent transaction set beyond 2020, comparing against Random Forest or other ensemble baselines, and enriching the feature set with time-based or regional demand signals to better capture seasonality effects like the pandemic-driven shifts noted in the source data.

## Code & Report
- ⚙️ <a href="https://github.com/alyssaplayer/BU_OMDS_APlayerRepo/tree/main/AlyssaPlayer_DX799_MilestoneTwo">GitHub Repository with Project Code (Python)</a>
