Spark Job for Daily customer transaction summary calculation

1. Scenario and tables overvie
  We’ll model a daily customer transaction summary for a bank.
  Source CSV files:
  - CUSTOMER.csv – customer master
  - ACCOUNT.csv – account master
  - TRANSACTION.csv – transaction fact 
  - BRANCH.csv – branch master
  - REF_TXN_TYPE.csv – reference table for transaction types
    
  Target table (CSV):
  - CUST_DAILY_TXN_SUMMARY.csv –  aggregated per customer/account/day with business logic.

2. Sample CSV structures
   check data/source directory for source data details

3.Target table design and business logic
customer_id,customer_number,customer_name,account_id,account_number,business_date,total_debit_amount,total_credit_amount,txn_count,atm_txn_count,pos_txn_count,online_txn_count,high_value_txn_flag,risk_segment,kyc_flag,country,branch_id,branch_region,last_txn_ts,net_txn_amount
1001,CUST0001,Ali Khan,2001,AE0000000001,2025-02-10,1250.00,0.00,2,1,1,0,N,LOW,Y,AE,3001,DXB,2025-02-10T09:15:00,-1250.00
1002,CUST0002,Sara Ahmed,2002,AE0000000002,2025-02-10,5000.00,0.00,1,0,0,1,Y,MEDIUM,Y,AE,3002,SHJ,2025-02-10T10:45:00,-5000.00

4. Confluence-style mapping table
   check mapping doc

5. CDC logic (applied before aggregation):
- Source: TRANSACTION.cdc_operation, last_updated_ts
- Rule:
- Include rows where cdc_operation IN ('INSERT','UPDATE') and last_updated_ts > :watermark_ts.
- Exclude cdc_operation = 'DELETE'.
- De-duplicate on txn_id using latest last_updated_ts.

6. Step-by-step PySpark job 
This is a single PySpark script that:
- Reads CSVs
- Applies schemas
- Filters active records
- Handles CDC
- Joins with reference and dimension tables
- Aggregates to target
- Writes target CSV

Thank you for reading. Be Happy 

