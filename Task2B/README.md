# Overview

<img src="../data/image/2023-08-27-14-43-04.png"  width="500"> <img src="../data/image/2023-08-27-14-43-21.png"  width="500">

## 1) Business background

- Problem statement see [main README](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/tree/main#task-2a-and-2b)
- Analytical problem
  - Binary classification with highly imbalanced data (50 bad actors / 1,000,000 customers = 0.005%), or outlier detection
- Available data
  - Scotiabank synthetic data: UofT_nodes.csv (KYC, Transactional data and Risk Rating)
- Challenges in modeling
  - Model is prone to overfitting on the 50 bad actors
- Use case
  - Once we have identified the bad actors, we should consider punitive actions such as closing their accounts and reporting them to the authorities

## 2) Data Understanding

- See more details about exploratory data analysis from [Task 2A README](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/Task2A)
  - Volumetric analysis, hypothesis based on domain knowledge, data exploration, data quality assessment
- Target variable = label of bad actors from [Task 1](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/Task1)
  - Huge class imbalance
  - 50 bad actors / 1,000,000 customers = 0.005%
  - <img src="../data/image/2023-08-27-09-54-40.png"  width="300">

## 3) Data preparation

- See more details about pipeline to prepare data from [Task 2A README](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/Task2A)
  - Fix data types
  - Treat missing values
  - Derived attributes based on domain knowledge
  - Single-attribute transformation

## 4) Modeling

- Analytical problem = <ins>binary classification</ins> or outlier detection
- Evaluation metric = <ins>Average Precision</ins>
  - i.e. Area under precision-recall curve ([sklearn documentation](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html#sklearn.metrics.average_precision_score))
  - Preferable to AUROC for highly imbalanced data
    - Precision = $\frac{TP}{TP+FP}$, Recall = $\frac{TP}{TP+FN}$, Specificity = $\frac{TN}{TN+FP}$
    - Average precision considers both precision and recall, while AUROC considers both recall and specificity
    - For highly imbalanced data, a high number of True Negatives produced by a poor model can inflate AUROC

### Shortlist promising models

- Consider a list of appropriate modeling techniques
  - Isolation forest
  - Logistic regression
  - Gaussian Naive Bayes
  - Decision tree
  - Random forest
  - Extra trees
  - Histogram-based Gradient Boosting
  - Bagging classifier
  - XGBoost classifier
  - LightGBM classifier
- Define procedure to test a model's quality and validity
  - Stratified shuffle split for both train-test split and cross validation to address class imbalance
    - 60-40 train-test split to have 30 bad actors in train set and 20 bad actors in test set
    - 3-fold cross validation to have 10 bad actors in each fold
- Shorlisted LightGBM, XGBoost and Logistic regression for hyperparameter tuning
  - <img src="../data/image/2023-08-27-11-23-05.png"  width="1000">

### Hyperparameter tuning

- Rationale for hyperparameter tuning  
  - First, determine the possible range of hyperparameters and their associated distributions with Grid Search to avoid overfitting on the 30 bad actors in the training set
    - LightGBM
      - max_bin: 150 to 300 (uniform)
      - num_leaves: 20 to 40 (uniform)
      - min_data_in_leaf: 10 to 30 (uniform)
      - bagging_fraction: 0.1 to 1.0 (uniform)
      - feature_fraction: 0.1 to 1.0 (uniform)
      - lambda_l1: 0.01 to 100 (log-uniform)
      - lambda_l2: 0.01 to 100 (log-uniform)
      - min_split_gain: 0.0001 to 0.01 (log-uniform)
      - max_depth: 15 to 30 (uniform)
      - extra_trees: True
    - XGBoost
      - max_depth: 2 to 10 (uniform)
      - n_estimators: 50 to 150 (uniform)
      - max_bin: 100 to 1000 (uniform)
      - min_child_weight: 0.1 to 10 (log-uniform)
      - subsample: 0.2 to 1.0 (uniform)
      - colsamples_bytree: 0.2 to 1.0 (uniform)
      - colsamples_bylevel: 0.2 to 1.0 (uniform)
      - colsamples_bynode: 0.2 to 1.0 (uniform)
    - Logistic regression
      - alpha: 0.0001 to 0.1 (log-uniform)
- Next, find optimal hyperparameters using [Bayesian optimization from skopt](https://scikit-optimize.github.io/stable/modules/generated/skopt.BayesSearchCV.html)
  - 3-fold CV average precision and AUROC (for reporting purpose only) after hyperparameter tuning
  - <img src="../data/image/2023-08-27-11-25-01.png"  width="1000">
  - <img src="../data/image/2023-08-27-11-45-28.png"  width="1000">
- Best model after hyperparameter tuning
  - Balance class weights to address class imbalance
  - <img src="../data/image/2023-08-27-11-52-07.png"  width="1000">

## 5) Evaluation

- Distribution of predicted probabilities
  - Distribution is quite skewed towards 0 due to class imbalance, suggesting potential overfitting
  - ![2023-08-27-11-53-46](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/assets/12086923/3961c7f5-6479-4087-bf80-3b4fc1d3a519)
- Average precision on test set = 0.0018
  - While the model performs 37x better than baseline (random guess = 50 / 1,000,000 = 0.00005),
  - it still seems to have overfitted as it cannot perform as well in the test set as in cross-validation
  - ![2023-08-27-12-00-03](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/assets/12086923/2509ca8a-8efa-408c-92df-ed543284ba32)
  - Precision-recall curve
    - Due to huge class imbalance, the model's precision is very low even if the cutoff is chosen at high expected probability
    - ![2023-08-27-12-12-16](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/assets/12086923/e6a451b9-b7dc-4773-84c1-2526340ac508)
- Lift and gain charts
  - Our model achieved 6.5x lift within the 1st decile
  - Quite low compared to maximum possible lift in 1st decile = ${999950\text{ normal customers} + 50\text{ bad actors} \over 50\text{ bad actors}} = 20000\times$
  - ![2023-08-27-12-10-09](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/assets/12086923/de94a45a-cfb7-4561-b729-aa4c20287bf6)
- AUROC on test set = 0.8983
  - ![2023-08-27-12-11-18](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/assets/12086923/c358a460-f5e7-4c85-bb2c-d99773f5c056)
  - As mentioned earlier, while AUROC is high, it is misleading for highly imbalanced data

## 6) Improvements to make: [Task 3](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/Task3)

- While the model performs better than random guess, its lift in the 1st decile is still quite low
- Possible improvements for the current approach
  - More data to train the model on (see [Task 3](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/Task3))
  - More experimentation with sampling techniques to address class imbalance
    - Likely require caching to reduce training time
