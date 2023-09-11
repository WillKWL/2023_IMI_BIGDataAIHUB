# Overview

<img src="../data/image/task1-image-8.png"  width="1000">

## 1) Business background

- See [main README](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/tree/main#task-1) for problem statement
- Analytical problem
  - NLP fuzzy matching algorithm that can continuously monitor client names with most updated watchlists
- Data sources
  - 1 million synthetic customer names from UofT_nodes.csv
  - 430 thousand sanctioned persons from [OpenSanctions watchlist](https://www.opensanctions.org/datasets/default/)
- Challenges
  - __Need for speed:__ there was a total of 430 billion combinations between 1 million customer names and 430 thousand sanctioned persons to check
  - __Need for fuzzy matching:__ exact name matching cannot capture variations of the same name
  - __No ground truth:__ we could only rely on qualitative assessment of the matches as there is no ground truth to evaluate the quality of the matches
- Summary
  - <img src="../data/image/2023-08-26-12-37-11.png"  width="1000">

## 2) Data Understanding

- Attributes from UofT_nodes.csv and [OpenSanctions watchlist](https://www.opensanctions.org/datasets/default/):
  - Unique identifier (id)
  - Name (caption / name / alias / weakAlias / previousName)
  - Date of birth ("BirthDate")
  - Gender
  - Country / nationality
- Data quality issues
  - Possible variations of the same name include:
    - Spelling mistake (e.g. Mayr instead of Mary)
    - Abbreviation (e.g. M. instead of Marie)
    - Nickname and alias
    - Shuffling of first, middle and last name due to different convention
    - Phonetic error due to inconsistent spelling of phonetically identical names
  - Solution = fuzzy name matching algorithm
    - Exact name matching cannot capture these variations of the same name

## 3) Data preparation

- Text processing to address data quality issues
  - Remove non-english characters, e.g. Иван
  - Remove punctuation and spaces between characters as we can assume delimiter has to be a space or underscore or hyphen
  - Use only lowercase characters
- Final output
  - Each name becomes a continuous string of characters, which is then converted into a list of 3-gram tokens
  - <img src="../data/image/2023-08-27-20-38-31.png"  width="1000">

## 4) Modeling


- List of appropriate modeling techniques
  - Vector space model
    - Bag of words
  - Fuzzy matching
    - Levenshtein distance / Jaro-Winkler distance / Cosine similarity / Jaccard similarity
  - Phonetic algorithms
    - Soundex / Metaphone / Double Metaphone
  - Word embedding
    - However for the purpose of matching names, we don't need to capture the semantic meaning of the words
- Constraints in choosing a modeling technique
  - Checking one combination at a time is infeasible
    - If checking a pair of names takes 1 millisecond (i.e. 0.001 second), checking a total of 430 billion combinations between 1 million customer names and 430 thousand sanctioned persons would take roughly 14 years
  - Checking all combinations is inefficient
    - Most pairs of names are vastly different and should be dismissed quickly as a false match
- Final approach to address the constraints and challenges in modeling
  1) Bag of words
     - Quick to compute as a vector space model to represent a name as a real-valued vector
  2) Binary occurrence (instead of TF-IDF)
     - Very short texts (e.g. names) tend to have noisy TF-IDF values while binary occurrence values are more stable
  3) Tokenization with 3-gram
     - Flexibility to handle minor discrepancies in spelling and ordering of words
  4) Cosine similarity
     - Simple matrix multiplication is quick to compute
  5) CSR sparse matrix format
     - Efficient top-n multiplication based on SciPy sparse matrix dot function and NumPy argpartition function ([more details](https://www.sun-analytics.nl/posts/2017-07-26-boosting-selection-of-most-similar-entities-in-large-scale-datasets/))
- Procedure to test our approach's quality and validity
  - Given there is no ground truth, we had to resort to qualitative assessment of the matches
  - Define a name to match = 'Young Marie Mildren'
  - Draw ideas from contrastive learning to manually define a set of examples containing
    - Positive examples that include possible variations of the same name (e.g.     'Young MarieMildren', 'Young M Mildren', 'Young, Maarrie Mildren', 'Young, Mildren', 'Young, aMrei Mildren', 'Marie Mildren Young', 'Yung Mary Mildren' etc.)
    - Negative examples that include names that do not represent the same person (e.g. Arei mr Remi.)
  - A good fuzzy matching algorithm should be able to assign a high similarity score to positive examples and a low similarity score to negative examples (as illustrated below)
    - ![2023-08-27-21-24-32](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/assets/12086923/a7a05b9c-110c-4c5d-8901-765872d24eed)
- Other considerations to identify the same person beyond matching just the name
  - Similarity of the following attributes is calculated as ${(1 - abs(difference))} \over {max(abs(difference)})$ (e.g. by converting the difference in date of birth to a similarity score between 0 and 1)
    - Date of birth
    - Country
    - Politically exposed person affiliations
  - Prioritize results with
    - Exact match in name
    - Difference in date of birth <= 2 years
    - Length of name >= 3 to avoid just matching first and last name
  - Only consider matches for customer with "RISK" attribute = high
  - Sort the list of matches by average similarity score
  - Remove multiple matches for the same customer (i.e. only keep the one with the highest similarity score)

## 5) Evaluation

- Output = a list of 50 customers that are most similar to the sanctioned persons in the watchlist
  - <img src="../data/image/task1-image-3.png"  width="1000">
- What is good about the current approach
  - It is quick to compute and seems to return reasonable matches based on qualitative assessment
- What is bad about the current approach
  - Since there is no ground truth,
    - We adopted a heuristic-driven choice of modeling approach using bag-of-words, binary occurrence, 3-gram tokenization and cosine similarity
    - We made a heuristic-driven decision to calculate an "average similarity score" across multiple attributes, including name, date of birth and other personal information
  - CI/CD is not considered
    - Continuous monitoring of new customer names against updated watchlists is needed for a production-ready solution to be used in onboarding process
  - Additional information to incorporate
    - More refined text processing
      - There can be rules to manually exclude certain characters in a name (such as titles, legal entity status, Sr/Jr / II / III)
    - Existing [pairs data from OpenSanctions](https://www.opensanctions.org/docs/pairs/) to reduce false positives
      - The data has two entities on each line, and a judgement that states if the two records refer to the same logical item (positive) or if they are different logical items (negative)
- Next steps
  - Schedule deployment to check against updated watchlists
  - Apply contrastive learning with a larger set of positive and negative examples to learn good representation of personal identification information of an individual such that the fuzzy matching algorithm can be more robust in identifying the same person

## References

Domain knowledge

- Monetary Authority of Singapore [AML Name Screening Practices 2022](https://www.mas.gov.sg/-/media/MAS-Media-Library/publications/monographs-or-information-paper/IMD/2022/Strengthening-AML-CFT-Name-Screening-Practices.pdf)

Current approach in fuzzy name matching

- <https://pypi.org/project/sparse-dot-topn/>
- <https://medium.com/wbaa/https-medium-com-ingwbaa-boosting-selection-of-the-most-similar-entities-in-large-scale-datasets-450b3242e618>
- <https://www.sun-analytics.nl/posts/2017-07-26-boosting-selection-of-most-similar-entities-in-large-scale-datasets/>

Other approaches

- <https://towardsdatascience.com/fuzzy-matching-at-scale-84f2bfd0c536>
- <https://bergvca.github.io/2017/10/14/super-fast-string-matching.html>
- <https://towardsdatascience.com/how-to-build-a-smart-search-engine-a86fca0d0795>
- <https://towardsdatascience.com/how-to-build-a-search-engine-9f8ffa405eac>
- oreilly book <https://learning-oreilly-com.ezproxy.torontopubliclibrary.ca/library/view/natural-language-processing/9781617294631/OEBPS/Text/03.html#ch03lev1sec2>
- annoy for nearest neighbors <https://pypi.org/project/annoy/>
- word2vec embedding <https://radimrehurek.com/gensim/models/word2vec.html>
