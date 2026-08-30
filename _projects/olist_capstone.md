---
layout: page
title: Predicting Customer Satisfaction (Olist)
description: Application of varied machine learning techniques to data from the e-commerce platform, Olist, to predict customer satisfaction from different features.  
img: assets/img/olist_projectcard.png
importance: 1
category: machine learning
permalink: /projects/olistcapstone/
---

Built supervised and unsupervised machine learning models in Python to predict customer satisfaction on the Brazilian e-commerce marketplace, Olist, merging nine relational tables and evaluating regression, classification, and clustering approaches as part of my Master's capstone project at Boston University.

<hr>

## Overview

This project aimed to predict customer satisfaction (`review_score`, 1–5) for Olist, a Brazilian marketplace connecting small and medium sellers to major online retailers. Nine relational tables covering order, product, seller, customer, payment, and review information were merged into a single analytic dataset. Supervised models (KNN, Gradient Boosting) were used to predict satisfaction directly, while unsupervised methods (K-Means, DBSCAN, HAC) were used to explore whether hidden customer or order segments existed beyond the labeled features.


## Data & Preprocessing 
The dataset is the Brazilian Olist E-Commerce Public Dataset (Kaggle), merged from the nine available relational tables into a master data frame of ~88,000 orders. Preprocessing included feature scaling, one-hot encoding of categorical fields, and dimensionality reduction via PCA (95% variance retained, 18 components). EDA revealed weak single-feature correlations with `review_score` and a U-shaped, imbalanced score distribution. These findings influenced the modeling choices below.


## Modelling Approach
KNN and Gradient Boosting were deliberately framed differently to match what each algorithm needed to perform well against the same target. KNN was run as a regressor to preserve the ordinal structure of review_score, where a prediction of 4 should be penalized less than a prediction of 1 when the true score is 5. Gradient Boosting was framed as a 5-class classifier to get class-level precision and recall on the imbalanced, U-shaped score distribution and to better surface low-score cases that a regression model would likely average away.

KNN was tuned via GridSearchCV across neighbour count, distance metric, and weighting scheme, ultimately selecting a large neighbourhood of k=50, which is well above a typical range. This is supported by the noisy feature space read from preprocessing. Gradient Boosting was trained as a classifier on a 30% subsample of the data due to runtime constraints, which likely understates its true performance relative to Random Forest.


## Model Results
| Model                          | Task        | Result                  |
|---------------------------------|---------------|--------------------------|
| KNN Regression (k=50)          | Regression | RMSE 1.1694, R² 0.2309 |
| Gradient Boosting (Classifier) | Classification | 62.16%                   |
| K-Means (k=4)                  | Classification    | 63.84% |


KNN improved modestly over the OLS/majority baseline (RMSE 1.2138, R² 0.1713) but needed a large neighbourhood to do so, further emphasising the weak per-feature signal in the merged dataset. Gradient Boosting underperformed the pre-tested Random Forest classifier, though the gap is partly attributable to the reduced training subsample (30%), defined for runtime constraints, rather than the model being poorly suited to the problem.

## Feature Importance

<div class="row">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/olist_featureimportance.png" title="Feature Importance" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/olist_correlationmatrix.png" title="Correlation Matrix" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left: Delivery delay days and seller average score denoted with high feature importance Right: Pairwise correlations confirm
    that size-related features drive tax value most strongly.
</div>



## Feature Importance

Feature importance rankings were consistent across models and closely mirrored EDA correlation findings. Discount-adjacent logistics features like delivery_delay_days mattered, but seller_avg_score, photo_count, and description_length ranked above or near delivery delay in importance, which indicated that merchant quality and listing quality are just as important as delivery logistics in driving customer satisfaction and give Olist merchants actionable direction beyond shipping speed.

## Clustering Exploration

K-Means, DBSCAN, and HAC were applied to the scaled feature set to look for natural order or seller segments beyond the existing labels. K-Means' best silhouette score (0.202 at k=4) indicated weak-to-moderate cluster separation. DBSCAN and HAC were run on subsamples due to the aforementioned runtime constraints, and Ward-linkage HAC produced the most realistic and balanced clustering of the methods tested, but overall the clustering methods mainly rediscovered structure already captured by existing labels rather than revealing novel segments.

## Limitations & Next Steps

The R² ceiling of roughly 0.19–0.23 and classification accuracy in the low 60s reflect how difficult it is to predict a five-point satisfaction score from a bimodal, U-shaped distribution, especially one assembled from nine merged tables with diluted per-feature signal. Future direction includes training Gradient Boosting on the full dataset rather than a 30% subsample to get a fairer comparison against Random Forest, testing additional review-text or sentiment features from the comment fields, and exploring seller-level (rather than order-level) aggregation to see whether merchant-quality signals become even stronger predictors at that grain.<img width="468" height="136" alt="image" src="https://github.com/user-attachments/assets/bdbf150f-873e-4726-acbe-75a1704029ca" />




## Code & Report
- ⚙️ <a href="https://github.com/alyssaplayer/BU_OMDS_APlayerRepo/tree/main/AlyssaPlayer_DX799_MilestoneTwo">GitHub Repository with Project Code (Python)</a>
