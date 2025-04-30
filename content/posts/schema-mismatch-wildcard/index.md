+++
title = 'The Schema Mismatch Problem with BigQuery Wildcard Tables'
draft = false
description = 'Queries against BigQuery wildcard tables can fail due to schema mismatches. This post explains the problem and how to diagnose and fix it.'
tags = ["bigquery", "schema", "data-pipelines"]
categories = ["Tech", "Tutorials"]
date = 2025-04-30
+++



### What are wildcard tables?
Wildcard tables provide a concise way to query multiple tables that share a common naming pattern and schema.

A wildcard table effectively represents a `UNION` of all tables matching a specified wildcard expression (e.g., `table_prefix*`). When you query a wildcard table, BigQuery treats the matching tables as a single logical table. A special pseudo-column, `_TABLE_SUFFIX`, is automatically included in the query results. This column contains the value matched by the wildcard character for each row, allowing you to identify the source table. 

For example, this query selects all columns from all tables starting with `table_prefix` within the specified dataset: 

```sql
SELECT * FROM `{project_id}.{dataset_id}.table_prefix*`
```

*(Note: Remember to replace `{project_id}` and `{dataset_id}` with your actual project and dataset IDs)*

### When to Use Wildcard Tables
Wildcard tables are useful when you generate data incrementally into new, date-sharded, or logically separated tables, rather than appending to a single large table.

A common use case is storing daily data snapshots or results from recurring jobs, such as daily ML model predictions. Creating a new table each day (e.g., `predictions_20250404`, `predictions_20250405`) simplifies data ingestion and allows for easy querying of specific date ranges or the entire history via a wildcard (`predictions_*`). This approach can also align well with data lifecycle management and immutability principles.

### When Wildcard Queries Fail: The Schema Requirement
Querying wildcard tables comes with a critical requirement: **all tables matching the wildcard expression must have identical schemas.**

When any of the matching tables has a schema mismatch,  queries agains the wildcard table will fail. This mismatch is usually caused by a concept called Schema drift — a common operational hazard, often introduced by changes in automated data pipelines, manual table creation errors, or evolving upstream data sources if not carefully managed.

We recently encountered a scenario where a previously functional wildcard query started failing. The root cause was a schema inconsistency: a column that was expected to be a `DATETIME` type across all tables had been created with a `TIMESTAMP` type in some of the newer tables matching the wildcard. Although `DATETIME` and `TIMESTAMP` are related, they are distinct types in BigQuery and cause a schema conflict in this context.

### How to Diagnose the Schema Mismatch
BigQuery's error messages for schema mismatch in wildcard queries are often quite informative, typically pointing to the specific column causing the conflict and the different data types encountered.

Once you know the name of the conflicting column (e.g., `prediction_time`), you can use the `INFORMATION_SCHEMA.COLUMNS` view to inspect the data types of that column across all tables matching your wildcard pattern within the dataset.

Run a query like this, replacing the placeholders:

```sql
SELECT
    table_name,
    column_name,
    data_type
FROM
    `{project_id}.{dataset_id}`.INFORMATION_SCHEMA.COLUMNS
WHERE
    table_name LIKE '{table_prefix}%' -- Match tables by prefix
    AND column_name = '{faulty_column}'; -- Filter for the conflicting column
```

This query will list the `table_name`, the `column_name`, and its `data_type` for the specified column in all tables matching the prefix. By examining the results, you can quickly identify which tables have the incorrect data type for the column.

### How to Resolve the Problem
Addressing a schema mismatch requires correcting the schema in the affected tables. There isn't a single "fancy" command to automatically harmonise types in this scenario, especially for incompatible type changes.

### How to Prevent Recurrence
Preventing schema drift in automated data pipelines is crucial for maintaining the reliability of wildcard queries. Implement measures to ensure consistent schema when new tables are created:
1. **Explicit Schema Definition:** Always define the schema explicitly when creating new tables or loading data. Avoid relying on auto-detection, which can sometimes infer different types based on the data sample.
2. **Schema Validation in ETL/ELT Pipelines:** Incorporate schema validation steps into your data pipelines _before_writing data to BigQuery. Tools like Great Expectations, dbt's schema tests, or custom validation scripts can assert that the data conforms to the expected schema and prevent inconsistent tables from being created.

In our case, we were writing a pandas DataFrame to BigQuery, but we providing a schema to the `google.cloud.bigquery.LoadJobConfig` object, something like this:

```python

# DataFrame of scores to be persisted
scores_df = ... 
score_df["score"] = datetime.strptime(date, "%Y%m%d")

client = bigquery.Client()
job_config = bigquery.LoadJobConfig()
job_config.autodetect = True
job_config.write_disposition = "WRITE_TRUNCATE"

# Schema definition and BigQuery upload
job_config.schema = [
    bigquery.SchemaField("user_id", "INTEGER"),
    bigquery.SchemaField("score", "FLOAT"),
    bigquery.SchemaField("prediction_time", "DATETIME"),
]
load_job = client.load_table_from_dataframe(
	score_df, f"{project}.{dataset}.{table}_{date}", job_config=job_config
)
```

Enforcing schema consistency at the point of table creation ensures that queries against wildcard tables will not fail due to schema mismatches.
