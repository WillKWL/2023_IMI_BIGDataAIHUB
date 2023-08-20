# 2023_IMI_BIGDataAIHUB
https://www.utm.utoronto.ca/bigdataaihub/events/2022-23-imi-bigdataaihub-case-competition

- include our presentation (visualization of what this project is about)
- description
- dataset
- how to download the model
- installation
- setup
- inference demo
- contributors
- Key highlights (CV items)
- next steps

# provide one set of the following for each task

# 1) Business background
- business problem
- benefits if the problem is solved
- data available
- business terminology
- data mining goal
  - task 1: name matching
  - task 2A: ranking
  - task 2B: classification
  - task 3: embedding
- success criteria
  - task 1: runtime and qualitative assessment
  - task 2A: confusion matrix and multi-partite AUC
  - task 2B: gain and lift, average precision
  - task 3: improvement on task 2A and B
 
# 2) Data Understanding
- volumetric analysis
  - history of the data
  - quantity of data available (number of rows and what columns are there)
- attribute types and values
  - check attribute types
  - check attribute value ranges
  - meaning of each attribute in business terms
  - basic statistics of each attribute (distribution, average, min, max, sd, mode, skewness etc.)
  - attributes (which ones are relevant / irrelevant)
  - industry domain knowledge
  - data imbalance?
- data exploration
  - hypothesis
- data quality
  - coverage (if all possible values are represented)
  - missing values
  - plausibility of values

# 3) Data preparation
- data cleaning report
  - decisions and actions taken to address data quality problems
- derived attributes
  - domain knowledge
  - constraints in modeling approach (e.g. heteroscedasticity)
  - impute missing values
- single-attribute transformation

# 4) Modeling
- consider a list of appropriate modeling techniques
- constraints for no deep learning (interpretability, computation time, knowledge)
- assumptions for chosen model
- define procedure to test a model's quality and validity
  - train / test split
  - how to define folds for cross validation
  - performance measure
- build model
  - rationale for initial hyperparameters
  - hyperparameter tuning
- model description
  - final set of hyperparameters
- assess model
  - evaluation criteria / lift and gain tables
  - test result
  - interpreation of performance on unseen data
  - interpretaion in business terms
  - analyze potential deployment of each result
  - insights in why a certain model / certain hyperparameter lead to good / bad results

# 5) Evaluation
- Results = Models + Findings
- findings that are important in
  - meeting business objectives
  - leading to new questions
  - recommendations for new data mining projects
- review process
  - for each stage, ask
    - was it necessary
    - was it executed optimally
    - in what ways can it be improved
  - identify failures
  - identify misleading steps
  - identify possible alternative actions and / or unexpected paths in the process
- list possible actions
  - with reasons for / against each option
  - rank each possible action
  - 
