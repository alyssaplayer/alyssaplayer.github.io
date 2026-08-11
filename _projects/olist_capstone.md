---
layout: page
title: MS Data Science Capstone Project 
description: Application of varied machine learning techniques to data from the e-commerce platform, Olist, to predict customer satisfaction from different features.  
img: assets/img/olist_projectcard.png
importance: 1
category: machine learning
permalink: /projects/olistcapstone/
---

Built supervised and unsupervised machine learning models in Python to predict customer satisfaction on the Brazilian e-commerce marketplace, Olist, merging nine relational tables and evaluating regression, classification, and clustering approaches.

<hr>

## Overview

This project aimed to predict customer satisfaction (`review_score`, 1–5) for Olist, a Brazilian marketplace connecting small and medium sellers to major online retailers. Nine relational tables covering order, product, seller, customer, payment, and review information were merged into a single analytic dataset. Supervised models (KNN, Gradient Boosting) were used to predict satisfaction directly, while unsupervised methods (K-Means, DBSCAN, HAC) were used to explore whether hidden customer or order segments existed beyond the labeled features.


## Data & Preprocessing 

The dataset is the Brazilian Olist E-Commerce Public Dataset (Kaggle), merged from nine relational tables into a master data frame of ~88,000 orders. Preprocessing included feature scaling, one-hot encoding of categorical fields, and dimensionality reduction via PCA (95% variance retained, 18 components). EDA revealed weak single-feature correlations with `review_score` and a U-shaped, imbalanced score distribution. These findings influenced the modelling choices below.


## Modelling Approach

Two different formulations of the same target were deliberately used to best match what each algorithm required to perform well. 
- **KNN** Used a regression model to preserve the ordinal structure of `review_score`. A prediction of 4 should be penalized less than a prediction of 1 when the true score is 5, which classification would treat those differences as equally incorrect.

- **Gradient Boosting** Used a classification model to get class-level precision/recall on the imbalanced, U-shaped score distribution and to better identify low-score cases that a regression model would likely to average away.

Scaling had a much smaller effect on Olist than on comparable models trained on cleaner datasets and GridSearchCV selected a large neighborhood (k=50) than typically expected. This indicates a noisy, diluted feature space, which is likely an artifact of merging the nine separate tables.

## Model Results
| Model                          | Metric        | Result                  |
|---------------------------------|---------------|--------------------------|
| KNN Regression (k=50)          | Test RMSE / R²| 1.1694 / 0.2309          |
| Gradient Boosting (Classifier) | Test Accuracy | 62.16%                   |
| K-Means (k=4)                  | Silhouette    | 0.202 (best of tested k) |


KNN improved over baseline (RMSE 1.2138, R² 0.1713) but required a far larger neighbourhood than the comparable US e-commerce model (k=50 vs. k=3) highlighted on its own — [project page](https://alyssaplayer.github.io/projects/usecommerce/) — reinforcing that Olist's feature space carries weaker per-feature signal. Gradient Boosting underperformed Random Forest results from an earlier iteration of this project, and is likely due to the runtime-constrained algorithm (trained on a 30% subsample) rather than being unsuited to the problem.



## Feature Importance

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/feature_importance.png" title="Feature Importance" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/correlation_matrix.png" title="Correlation Matrix" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Gradient Boosting feature importances — sqft_squared dominates, followed by
    finished square footage and bathroom counts. Right: Pairwise correlations confirm
    that size-related features drive tax value most strongly.
</div>



## Target Distribution & Residuals

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/distributions.png" title="Feature Distributions" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The target variable is strongly right-skewed. A small number of high-value outliers
    inflate the mean and contributed to the model's maximum absolute error of $40.8M.
    The model is well-balanced overall, with near-equal over- and under-prediction rates.
</div>



## Limitations & Next Steps

The model struggles with extreme high-value properties due to high skew in the target variable. Future work
would include additional hyperparameter tuning, comparison with more complex models such as Random Forest, and enriching the
dataset with neighborhood-level features (proximity to schools, walkability, comparable sales).



## Code & Report

- 📄 <a href="/assets/pdf/zillow_exec_summary.pdf">Executive Summary</a>
- ⚙️ <a href="https://github.com/alyssaplayer/BU_OMDS_APlayerRepo/blob/main/DX603_FinalProject_ZillowValuationTool.ipynb">GitHub Repository with Project Code (Python) </a>
