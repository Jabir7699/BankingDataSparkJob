<h1> Spark Job for Daily customer transaction summary calculation </h1>
<br>
<h3>1. Scenario and tables overview</h3>
<br>
  We’ll model a daily customer transaction summary for a bank.<br>
  <h5>Source CSV files:</h5>
  - CUSTOMER.csv – customer master <br>
  - ACCOUNT.csv – account master <br>
  - TRANSACTION.csv – transaction fact  <br>
  - BRANCH.csv – branch master <br>
  - REF_TXN_TYPE.csv – reference table for transaction types <br>
     <br>
  Target table (CSV): <br>
  - CUST_DAILY_TXN_SUMMARY.csv –  aggregated per customer/account/day with business logic.
 <br>
<h3>2. Sample CSV structures </h3> <br>
   check data/source directory for source data details
 <br>
<h3>3.Target table design and file  </h3> <br>
customer_id,customer_number,customer_name,account_id,account_number,business_date,total_debit_amount,total_credit_amount,txn_count,atm_txn_count,pos_txn_count,online_txn_count,high_value_txn_flag,risk_segment,kyc_flag,country,branch_id,branch_region,last_txn_ts,net_txn_amount  <br>
Target file name : CUST_DAILY_TXN_SUMMARY.csv _ Available in data/target folder <br>
 <br>
<h3>4. Confluence-style mapping table</h3>  <br>
   check mapping doc in mapping folder

<h3>5. CDC logic (applied before aggregation):</h3>  <br>
- Source: TRANSACTION.cdc_operation, last_updated_ts  <br>
- Rule:  <br>
- Include rows where cdc_operation IN ('INSERT','UPDATE') and last_updated_ts > :watermark_ts.  <br>
- Exclude cdc_operation = 'DELETE'.  <br>
- De-duplicate on txn_id using latest last_updated_ts.  <br>
 <br>
<h3>6. Step-by-step PySpark job</h3>   <br>
<h5>This is a single PySpark script that:</h5> <br>
- Reads CSVs</li> <br>
- Applies schemas <br>
- Filters active records <br>
- Handles CDC <br>
- Joins with reference and dimension tables <br>
- Aggregates to target <br>
- Writes target CSV <br>
 <br>
Thank you for reading. Be Happy  

