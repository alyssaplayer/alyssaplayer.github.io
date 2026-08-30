---
layout: page
title: Predicting Key Profit Drivers from US E-Commerce Records 
description: Application of varied machine learning techniques to a US E-Commerce dataset from 2020 to identify key profit drivers as part of my Master's capstone project at Boston University.
img: assets/img/us_ecommerce_projectcard.png
importance: 2
category: machine learning
permalink: /projects/usecommerce/
---

Built supervised and unsupervised machine learning models in Python to predict order-level profit drivers for a US e-commerce retailer, applying feature scaling, hyperparameter tuning, and cluster analysis on real-world transaction data.

<hr>

## Background
Exploratory analysis, PCR, and correlation heatmapping indicated that Discount and Sales would be the strongest predictor of the target variable, Profit, and identified Office Supplies as the most consistently profitable category despite Technology's higher per-order value. A first pass Random Forest model (tuned via GridSearchCV) reached a test R² of 0.899 and RMSE of $37.04, establishing that profit was highly predictable from these features and sets up the deeper model comparison below. 

## Overview
This project addressed the challenge of identifying profit drivers at the order level for a US E-Commerce records dataset from 2020 to help regional sales/operations managers, demand analytics teams, and supply chain leaders optimise inventory allocation and target areas for improvement. Supervised models, KNN and Gradient Boosting, were evaluated against the continuous Profit target, while K-Means, DBSCAN and HAC were used to test for hidden customer/order segments beyond the pre-labeled features. 

## Data & Preprocessing
The dataset spans ~3,300 US sales transactions (Kaggle) across the year 2020. Features include product category, customer segment, region, and ship mode. Feature scaling had a substantial effect on KNN performance, and PCA-based dimensionality reduction was tested but ultimately negatively impacted model performance so the full feature set was retained. 

## Modeling Approach
KNN regression was tuned via GridSearchCV across neighbour count, distance metrics, and weighting scheme. Feature scaling showed improvement to RMSE by approximately 13% over the raw baseline. The bias-variance tradeoff was plotted across a range of k=1-49 to evaluate overfitting. At k=1 training RMSE was near $0 which indicates the model is memorising individual points while test RMSE peaked near $90 with test error dipping between k=2-5, which supported GridSearchCV result of k=3. 

Gradient Boosting was tuned in two rounds, with the initial search spanning 300-500 estimators and extending to 500-700 as RMSE kept improving. The parameters of the final model were `{learning_rate: 0.1, max_depth: 3, n_estimators: 700, subsample: 0.8}`. Overfitting was addressed through the use of shallow trees, subsampling, and low learning rate acting as regularisation against overfitting.

## Model Results
| Model                  | Test RMSE | Test R² |
|-------------------------|-----------|---------|
| KNN Regression (k=3)    | $81.48    | 0.512   |
| **Gradient Boosting**   | **$36.81**| **0.9005**|

Out of the models tested, Gradient Boosting was the best performing. The extended estimator search yielded a performance plateau (RMSE $37.22 → $36.81) which indicated real model fit. The small k=3 from KNN reinforced these findings, which furthermore supports the Discount / Sub-category correlation observed during EDA. 

## Classification & Baseline Regression

In an early phase of this project, broad regression models were used to establish baselines and explore profit from a classification angle. OLS regression and alternate regularised models (Lasso, Ridge, and ElasticNet) plateaued around R² = 0.39 and RMSE ≈ $91 across all three penalty types indicating that this performance is derived from feature signal rather than choice of model. Reframing profit as a binary value (high/low) altered the model performance where logistic regression reached 77.9% accuracy, SVM reached 78%, and a tuned Random Forest classifier reached 90.05% accuracy. Discount was consistently the negative predictor of profit across every model tested. 

| Model                        | Task           | Result          |
|-------------------------------|----------------|------------------|
| OLS / Lasso / Ridge / ElasticNet | Regression   | R² = 0.39, RMSE = $91 |
| Logistic Regression            | Classification | 77.9% accuracy  |
| SVM (RBF)                      | Classification | 78% accuracy    |
| **Random Forest**              | **Classification** | **90.05% accuracy** |

The disparity between the weak value prediction (R² 0.39) and strong classification (90% accuracy) suggests the data carries a clear high/low profit signal even where precise dollar-value prediction is muddier. 

## Clustering Exploration
K-Means, DBSCAN, and HAC were applied to the scaled feature set to look for natural order segments. K-Mean's best silhouette score (0.188, k=10) indicated weak cluster separation and upon futher exploration showed that it was largely rediscovering existing relationships rather than finding new structure. DBSCAN collapsed to a single cluster with 10.3% noise and failed to find meaningful density-based structure. This was most likely due to the high-dimensional, one-hot-encoded feature space lacking the density contrast that DBSCAN requires. HAC (average linkage) outperformed K-Means (silhouette 0.245 at k=10) though cluster sizes were highly uneven. This was a real negative result and no meaningful hidden segments were found. 

## Limitations & Next Steps
The clustering methods confirmed that the existing labels throughout the dataset were capturing dominant structure in the data rather than uncovering new segments. Future direction would include testing on a larger dataset beyond the span of one year, and can compare against other models. It would also be interesting to look at this dataset on a more minute scale and see shifts in month-to-month performance given the context of the 2020 COVID-19 pandemic that may have influenced consumer behaviour. 

## Code & Report
- ⚙️ <a href="https://github.com/alyssaplayer/BU_OMDS_APlayerRepo/tree/main/AlyssaPlayer_DX799_MilestoneTwo">GitHub Repository with Project Code (Python)</a>
