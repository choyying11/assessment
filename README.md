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

Raw CSV
   ↓
staging_transactions
   ↓
Data quality checks
   ├── duplicate txn_id
   ├── missing values
   ├── invalid dates
   └── invalid amounts
   ↓
Clean fact_transactions
