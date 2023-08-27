# Detect money laundering and criminal networks
- Case competition description
  - https://www.utm.utoronto.ca/bigdataaihub/events/2022-23-imi-bigdataaihub-case-competition
- 1st place announcement
- include our presentation (visualization of what this project is about)

# Motivation
Financial Institutions are used by criminals to “legitimize” money generated from criminal activities, hence banks are expected by society and regulators to act proactively in detecting and stopping criminals from using the financial system to access their money through effective monitoring and reporting to authorities.

# Complication
- Number of clients and transaction volume are large and only expected to grow in the future
- Diminishing returns of adding more headcount
- Criminals are constantly innovating on ways to use financial system to “legitimize” their funds

# Ask from Scotiabank
- Address challenges across 3 stages of Money laundering (i.e. the act of “turning the proceeds of crime into cash or property that looks legitimate and can be used without suspicion”)
## Stage 1: Placement
- Money laundering starts by placing funds derived from illegal activities into the financial system in the form of cash deposits, cheque, money transfers or any type of transaction where money enters the Bank.
- Placement can be identified at the frontline by understanding the source of the funds.
- Problem statement
  - __Ideally__, financial institutions can prevent money-laundering criminals from infiltrating the banking network by employing a manual Know-Your-Customer (KYC) and ongoing monitoring process conducted by well-trained staff.
  - __In practice__, the sheer volume of new account registrations necessitates an automated screening system to identify trustworthy customers. However, criminals can manipulate personal identification details to exploit vulnerabilities in this system.
  - __Consequences:__ For both customers and the society as a whole, ineffective combat against money laundering and terrorist activity financing can potentially bring tremendous harm and disruption to the financial system . For banks, it poses a substantial risk of undermining their reputation and overall performance.
- __Proposal:__ One proposal is to find known high risk people in customer database using public data with a <ins>screening algorithm</ins> that can be
  - Flexible enough to handle minor discrepancies in spelling,
  - Efficient enough to process a significant volume of matches, and
  - Comprehensive enough to consider additional personal information beyond just the name itself (e.g. date of birth and politically exposed person affiliations).
- Task1: Find known high risk people in our customer base using public data
- if 
- by
- will
- because
## Stage 2: Layering
- Second stage is for a criminal to move funds around to hide their illegal origins. It may consist of multiple transactions without clear purpose to move money between products, clients, bank, corporations and geographies.
- Layering can be detected by understanding transaction purpose and if it is unusual for the customer.
- Problem statement
  - __Ideally__, financial institutions can identify every money-laundering criminals who have infiltrated the banking network based on transaction data, and subsequently stop providing banking services to them.
  - __In practice__, criminals can manipulate their activities to mimic like regular customers by avoiding conspicuously large or frequent transactions.
  - __Consequences:__ While misclassifying a regular customer as a criminal may damage relationship with the customer, misclassifying a criminal as a regular customer is far worse with the potential to face substantial reputational damage and heavy regulatory fines.
- __Proposal:__ 
  - Task 2A (link here): We can score customers as low, medium and high risk tiers based on their likelihood of engaging in money-laundering activities.
  - Task 2B (link here): We can build a model to classify 50 bad actors from a pool of 1 million customers based on their likelihood of engaging in money-laundering activities.
- if 
- by
- will
- because

## Stage 3: Integration
- The final stage integrates the illicit proceeds, that now appear to be clean funds, into the economy as "normal" personal or business transactions. By this stage is hard to distinguish between legal and illegal money, and the criminals can now use these funds without suspicion.
- Integration can be detected by knowing the client and monitor for odd and unusual transactions.
- Task 3: Enhance scoring and visualize networks using connections between clients
- if 
- by
- will
- because

- description
- dataset
- how to download the model
- installation
- setup
- inference demo
- contributors
- Key highlights (CV items)
- next steps
