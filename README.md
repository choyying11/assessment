# Task 1: Data modelling - Redshift schema design

dealers.csv
-	Dimension data (dim dealer)
-	3 dealer tiers: master -> agent -> sub_agent
-	recruiter_id: dealer’s upline

transactions.csv
-	transaction fact (fact_transactions)
-	stores transaction-level business events.
-	one row = one transaction (txn_id)
-	one subscriber_msisdn may have multiple transactions

topups.csv
-	fact table (fact_topups)
-	stores top-up-level business events.
-	one row= one top-up transactions (topup_id)
-	one subscriber_msisdn may have multiple top up

dealer_hierarchy
- supports multi-level upline relationships


## Distribution and Sort Key

### Distribution

`DISTSTYLE AUTO` is used because the assessment does not provide
a specific cluster configuration or production query
workload. This allows the system to determine an appropriate
distribution strategy.

### Sort keys
Transaction and top-up facts are sorted by their respective date
columns because analytical queries are expected to filter and
aggregate data by time periods.


# Task 2:

Raw CSV  -> staging_transactions -> data quality checks ->  Clean fact_transactions



# Task 3:

Identified subscribers as at risk of churn when they have not made a top-up for more than 35 days. The latest available topup_date in the dataset is used as the analysis date, and each subscriber's most recent top-up date is compared against this date to calculate the days since their last top-up.

For each at-risk subscriber, the original activation dealer is identified using the earliest activation transaction, and the corresponding dealer tier is included in the output.

Churn window: A 35-day window is used to identify subscribers with prolonged inactivity while allowing for normal gaps between top-ups. This is a churn-risk indicator rather than confirmed churn, as subscribers may return and top up after the 35-day period. New subscribers with limited transaction history may also be more likely to be flagged depending on their activity.

# Key Findings

- Monthly GMV remained relatively stable at around 467K–496K from Jan–Dec 2024, before dropping sharply to 160 in Jan 2026.
- D0016 had the highest total commission among the top 10 dealers, at approximately 3,139, followed by D0037 (2,908) and D0029 (2,553).
- D1945 had the highest number of churn-risk subscribers at 20, followed by D2732 (19). Several other dealers had 17–18 churn-risk subscribers.
- The churn-risk analysis highlights dealers with a higher concentration of subscribers who have not topped up for more than 35 days, which can help prioritize potential retention efforts.


   
