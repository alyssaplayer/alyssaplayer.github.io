---
layout: page
title: Automated Property Valuation
description: Supervised ML model to predict residential assessed property values using Zillow data
img: assets/img/zillow_house.png
importance: 1
category: machine learning
permalink: /projects/zillow/
---

Built a supervised machine learning model in Python to predict residential property valuations, applying feature engineering, model selection, and performance evaluation on real-world Zillow data.

<hr>

## Overview

This project addressed the challenge of accurately pricing residential properties at scale for a real estate data platform. We engineered features from raw property data, evaluated
three regression models, and selected a Gradient Boosting model that achieved a cross-validation MAE of $189,297 and a test MAE of $195,927 which outperformed linear and
tree-based baselines by a significant margin.



## Data & Preprocessing 

The dataset used is a subset of the Zillow Kaggle competition dataset (77,613 properties, 55 features). Preprocessing involved removing columns with >90% missing values, median/mode imputation, one-hot encoding, and outlier filtering. The target variable (`taxvaluedollarcnt`) was highly right-skewed, which influenced the selection of MAE over RMSE
as the primary performance metric.



## Feature Engineering 

Engineered features included log-transformed square footage (`log_sqft`), squared square footage (`sqft_squared`), bathroom-to-bedroom ratio, and house age. These transformations
were most impactful for Lasso Regression, reducing its CV MAE from $242k to $234k.
Tree-based models did not benefit from these transformations as they handle nonlinear relationships. 




## Model Results

| Model                 | CV MAE       | Test MAE     |
|-----------------------|-------------|-------------|
| Lasso Regression      | $233,626    | —           |
| Decision Tree         | $209,967    | —           |
| **Gradient Boosting** | **$189,297**| **$195,927**|

Training MAE was $157,135 which indicates mild overfitting but the model was shown to still generalise well. 



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
