# Overview
<img src="../data/image/task1-image-8.png"  width="1000">

# 1) Business background
- See [main README](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/tree/main#task-1) for problem statement 
- Analytical problem
  - NLP fuzzy matching algorithm that can continuously monitor client names with most updated watchlists
- Data sources
  - 1 million synthetic customer names from UofT_nodes.csv
  - 430 thousand sanctioned persons from [OpenSanctions watchlist](https://www.opensanctions.org/datasets/default/) 
- Challenging aspect
  - Need for speed: a total of 430 billion combinations between 1 million customer names and 430 thousand sanctioned persons to check
  - Need for fuzzy matching: exact name matching cannot capture variations of the same name
  - Can only rely on qualitative assessment of the matches as there is no ground truth to evaluate the quality of the matches
  - <img src="../data/image/2023-08-26-12-37-11.png"  width="1000">
 
# 2) Data Understanding
- Attributes from UofT_nodes.csv and OpenSanctions watchlist: 
  - Unique identifier (id)
  - Name (caption / name / alias / weakAlias / previousName)
  - Date of birth ("BirthDate")
  - Gender
  - Country / nationality
- Data quality issues
  - Possible variations of the same name exist due to
    - Spelling mistake (e.g. Mayr instead of Mary)
    - Random abbreviation (e.g. M. instead of Marie)
    - Nicknames and alias
    - Shuffling of first, middle and last name due to different conventions
    - Phonetic error due to inconsistent translation from other languages
  - Solution = fuzzy name matching algorithm 
    - Exact name matching cannot capture these variations of the same name

# 3) Data preparation
- Text processing to address data quality issues
  - Remove all non-english characters, e.g. Иван
  - Remove all punctuation and spaces between characters: no assumption in delimiter being space or underscore or hyphen
  - Make all letters lowercase
- Final output
  - Convert each name into a continuous string of characters, which is then converted into a list of 3-grams
  - <img src="../data/image/2023-08-27-20-38-31.png"  width="1000">

# 4) Modeling
- List of appropriate modelling techniques
  - Vector space model
    - Bag of words / TF-IDF
  - Fuzzy matching
    - Levenshtein distance / Jaro-Winkler distance / Cosine similarity / Jaccard similarity
  - Phonetic algorithms
    - Soundex / Metaphone / Double Metaphone
  - Word embedding
    - But we don't really need to capture the semantic meaning of the words for the purpose of matching names
- Constraints
  - Checking one combination at a time is computationally infeasible
    - If checking a pair of names takes 1 millisecond (i.e. 0.001 second), checking a total of 430 billion combinations between 1 million customer names and 430 thousand sanctioned persons would take roughly 14 years
  - Checking all combinations is inefficient
    - Most pairs of names are vastly different and should be dismissed quickly
- Chosen approach
  - Bag of words
    - Quick to compute as a vector space model
  - Binary occurrence (instead of tf-idf)
    - Very short texts (e.g. names) tend to have noisy tf–idf values while binary occurrence values are more stable
  - 3-gram
    - Flexibility to handle minor discrepancies in spelling and ordering of words
- Measure of similarity between two names
  - Cosine similarity computed using CSR sparse matrix format
    - Simple matrix multiplication is quick to compute
    - CSR sparse matrix format allows efficient top-n multiplication based on SciPy sparse matrix dot function and NumPy argpartition function ([more details](https://www.sun-analytics.nl/posts/2017-07-26-boosting-selection-of-most-similar-entities-in-large-scale-datasets/))
- Procedure to test a model's quality and validity
  - Given there is no ground truth, we have to resort to qualitative assessment of the matches
  - Define a name to match = 'Young Marie Mildren'
  - Draw ideas from contrastive learning to manually define a set of examples containing
    - Positive examples that include possible variations of the same name (e.g.     'Young MarieMildren', 'Young M Mildren', 'Young, Maarrie Mildren', 'Young, Mildren', 'Young, aMrei Mildren', 'Marie Mildren Young', 'Yung Mary Mildren' etc.)
    - Negative examples that include names that do not represent the same person (e.g. Arei mr Remi.)
  - A good fuzzy matching algorithm should be able to assign a high similarity score to these variations
  - <img width="1006" alt="image" src="https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/assets/12086923/bcf46d52-a350-4345-9776-0821b275f7db">
- Other considerations beyond just the name itself
  - Date of birth
  - Country
  - Politically exposed person affiliations
  - Similarity is calculated as ${(1 - abs(difference))} \over {max(abs(difference)})$
    - e.g. converting the difference in date of birth to a similarity score
  - Prioritize results with 
    - Exact match in name
    - Difference in date of birth <= 2 years
    - Length of name >= 3 to avoid just matching first and last name
  - Only match if RISK = "high" for each customer
  - Sort by average similarity score
  - Remove multiple matches for the same customer (i.e. only keep the one with the highest similarity score)

# 5) Evaluation
- Output = a list of 50 customers that are most similar to the sanctioned persons in the watchlist
  - <img src="../data/image/task1-image-3.png"  width="1000">
- What is good about the current approach
  - It is quick to compute and seems to return reasonable matches based on qualitative assessment
- What is not good about the current approach
  - Since there is no ground truth, 
    - We adopted a heuristic-driven choice of bag-of-words + binary occurrence + 3-gram
    - We made a heuristic-driven decision to combine cosine similarity with similarity in name, date of birth and other personal information into an average similarity score is not optimal
  - Deployment option is not considered
    - Continuous monitoring of new customer names against updated watchlists is required
  - Additional resources to incorporate
    - There can be rules to manually exclude certain characters in a name (such as titles, legal entity status, Sr/Jr / II / III)
    - Existing [pairs data from OpenSanctions](https://www.opensanctions.org/docs/pairs/)
      - The data has two entities on each line, and a judgement that states if the two records refer to the same logical item (positive) or if they are different logical items (negative)
- Possible next steps
  - Schedule deployment to check against updated watchlists
  - Apply contrastive learning with a larger set of positive and negative examples to generalize the fuzzy matching algorithm

# References
Domain knowledge
- Monetary Authority of Singapore [AML Name Screening Practices 2022](https://www.mas.gov.sg/-/media/MAS-Media-Library/publications/monographs-or-information-paper/IMD/2022/Strengthening-AML-CFT-Name-Screening-Practices.pdf)

Current approach in fuzzy name matching
- https://pypi.org/project/sparse-dot-topn/
- https://medium.com/wbaa/https-medium-com-ingwbaa-boosting-selection-of-the-most-similar-entities-in-large-scale-datasets-450b3242e618
- https://www.sun-analytics.nl/posts/2017-07-26-boosting-selection-of-most-similar-entities-in-large-scale-datasets/

Other approaches
- https://towardsdatascience.com/fuzzy-matching-at-scale-84f2bfd0c536
- https://bergvca.github.io/2017/10/14/super-fast-string-matching.html
- https://towardsdatascience.com/how-to-build-a-smart-search-engine-a86fca0d0795
- https://towardsdatascience.com/how-to-build-a-search-engine-9f8ffa405eac
- oreilly book https://learning-oreilly-com.ezproxy.torontopubliclibrary.ca/library/view/natural-language-processing/9781617294631/OEBPS/Text/03.html#ch03lev1sec2
- annoy for nearest neighbors https://pypi.org/project/annoy/
- word2vec embedding https://radimrehurek.com/gensim/models/word2vec.html 
