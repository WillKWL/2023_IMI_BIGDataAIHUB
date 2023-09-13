# Detect money laundering using criminal networks

1st place ([announcement](https://www.linkedin.com/feed/update/urn:li:activity:7045542079829086208/)), 2022-23 IMI BIGDataAIHUB Case Competition

- The [competition](https://www.utm.utoronto.ca/bigdataaihub/events/2022-23-imi-bigdataaihub-case-competition) sponsored by Scotiabank and UofT BIGDataAIHUB was held from November 2022 to March 2023, and was open to graduate students (masters, PhD) and undergrads with big data/AI experience from any academic discipline at University of Toronto
- Contributors:
  - Anny Huang
  - Juandiego Morzán Samamé
  - William Kwok
- [Presentation PDF](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/data/IMI_Team35_Slides_Final_v3.pdf)

[<img src="data/image/2023-08-27-15-29-27.png"  width="500">](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/data/IMI_Team35_Slides_Final_v3.pdf) <img src="data/image/2023-08-27-15-24-10.png"  width="200">

## Problem statement

- __Ideally__,
  - Financial institutions can prevent money-laundering criminals from infiltrating the banking network by employing a manual Know-Your-Customer (KYC) and ongoing monitoring process conducted by a team of subject matter experts.
- __In practice__,
  - The sheer volume of new account registrations and the ever-growing number of payment transactions necessitate an automated and cost-efficient monitoring system to identify suspicious identities who are constantly innovating ways to “legitimize” their funds through the financial system.
  - While misclassifying a normal customer as a high-risk customer may damage the customer relationship and missed opportunities, misclassifying a criminal as a normal customer is far worse, with the potential to face substantial reputational damage and heavy regulatory fines and let criminal activities go undetected.
- __Consequences for ineffective Anti-Money Laundering (AML) practice:__
  - For the general public, ineffective combat against money laundering can lead to financial exclusion for marginalized individuals, while failure to cut off the money laundering that finances terrorism may lead to pervasive destruction in the society.
  - For banks and other financial institutions, AML failures pose a substantial risk of undermining their reputation and overall performance.

## Modeling tasks
There are three modeling tasks raised in this case competition.

### Task 1

- Data = KYC of 1 million synthetic customers and OpenSanctions public watchlist
- Ask = identify 50 bad actors in the customer database using the public watchlist
  - NLP technique that can be flexible enough to handle minor discrepancies in spelling, efficient enough to process a significant volume of matches, and comprehensive enough to consider additional personal information beyond just the name itself (e.g. date of birth and politically exposed person affiliations)
- More details about our code implementation and discussion of our results
  - [Task 1 README](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/Task1)

### Task 2A and 2B

- Data = KYC of 1 million synthetic customers and transaction data 
- Ask =
  - Task 2A: assign customers as low-, medium- or high-risk ratings based on their likelihood of engaging in money-laundering activities
    - Ordinal regression that can take into account the ranking information provided by the label, instead of multi-class classification
  - Task 2B: identify 50 bad actors from a pool of 1 million customers based on their likelihood of engaging in money-laundering activities
    - Binary classification with extremely imbalanced data (50 bad actors / 1,000,000 customers = 0.005%)
    - Anomaly detection
- More details about our code implementation and discussion of our results
  - [Task 2A README](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/Task2A)
  - [Task 2B README](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/Task2B)

### Task 3

- Data = same as Task 2 + directed payments between customers (graph data of nodes and links)
- Ask = enhance task 2 models by using client connections to either:
  - Extract new features
  - Develop graph models
  - Visualize interesting networks
- More details about our code implementation and discussion of our results
  - [Task 3 README](https://github.com/WillKWL/2023_IMI_BIGDataAIHUB/blob/main/Task3)

## 3 stages of money laundering

Money laundering refers to the act of “turning the proceeds of crime into cash or property that looks legitimate and can be used without suspicion”.

### Stage 1: Placement

- Money laundering starts by placing funds derived from illegal activities into the financial system in the form of cash deposits, cheques, money transfers, or any type of transaction where money enters the bank.
- Placement can be identified at the frontline by understanding the source of the funds.

### Stage 2: Layering

The second stage is for a criminal to move funds around to hide their illegal origins. It may consist of multiple transactions without a clear purpose to move money between products, clients, banks, corporations, and geographies.
- Layering can be detected by studying the purpose of the transaction and if it is abnormal for the customer.

### Stage 3: Integration

- The final stage integrates the illicit proceeds, which now appear to be clean funds, into the economy as "normal" personal or business transactions. By this stage it is hard to distinguish between legal and illegal money, and the criminals can now use these funds without suspicion.
- Integration can be detected by accumulating knowledge of the customer and monitoring for suspicious transactions.
