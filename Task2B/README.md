# Highlights
- 
# Overview
![Alt text](image.png)

# 1) Business background
- Problem statement
  - __Ideally__, financial institutions can prevent money-laundering criminals from infiltrating the banking network by employing a manual Know-Your-Customer (KYC) and ongoing monitoring process conducted by well-trained staff.
  - __In practice__, the sheer volume of new account registrations necessitates an automated screening system to identify trustworthy customers. However, criminals can manipulate personal identification details to exploit vulnerabilities in this system.
  - __Consequences:__ For both customers and the society as a whole, this situation could lead to an inability to effectively counter money-laundering activities. For banks, it poses a substantial risk of undermining their reputation and overall performance.
  - __Proposal:__ One proposal is to find known high risk people in customer database using public data with a <ins>screening algorithm</ins> that can be
  -   - flexible enough to handle minor discrepancies in spelling,
      - efficient enough to process a significant volume of matches, and
      - comprehensive enough to consider addiitional personal information beyond just the name itself (e.g. date of birth and politically exposed person affiliations).
  - How to use
  - Suspicious clients will not be onboarded, or if they are already onboarded, their accounts will be frozen

- Data available
  - Scotiabank list of 1 million customer names (synthetic data)
  - [OpenSanctions watchlist](https://www.opensanctions.org/datasets/default/)
- Business terminology
- Analytical problem
  - Predict Bad actors using results from Task 1 as target variable
- Success criteria
  - Qualitative assessment of the matches
 
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
