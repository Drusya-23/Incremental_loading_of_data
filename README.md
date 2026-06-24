# Incremental_loading_of_data
Data_engineering_project
# Azure Data Factory Incremental Loading Project

## Overview

This project demonstrates the implementation of an Incremental Data Loading pipeline using Azure Data Factory (ADF) and Azure SQL Database.

The pipeline loads only newly added or modified records from a source table into a target table by maintaining a watermark table that tracks the last successful load timestamp.

This approach improves performance, reduces data movement, and follows industry-standard ETL best practices.

---

## Architecture

Source Table (company_staff)
↓
Watermark Table (Watermark1)
↓
Lookup Activity
↓
Set Variable Activity
↓
Copy New Records Activity
↓
Update Watermark Stored Procedure
↓
Target Table (Staff_Target)

---

## Technologies Used

* Azure Data Factory
* Azure SQL Database
* Azure Data Lake Storage Gen2
* T-SQL
* Stored Procedures
* GitHub

---

## Project Components

### Source Table

**company_staff**

Stores employee information along with a ModifiedDate column used for incremental loading.

Example:

| EmployeeID | EmployeeName | Salary | ModifiedDate        |
| ---------- | ------------ | ------ | ------------------- |
| 1          | John         | 50000  | 2026-06-20 10:00:00 |
| 2          | Alice        | 60000  | 2026-06-21 12:00:00 |

---

### Watermark Table

**Watermark1**

Stores the timestamp of the last successful load.

| TableName     | LastLoadDate        |
| ------------- | ------------------- |
| company_staff | 2026-06-21 12:00:00 |

---

### Target Table

**Staff_Target**

Receives only the new records identified during each pipeline execution.

---

## Pipeline Activities

### 1. Lookup_Watermark

Retrieves the last successful load timestamp from the Watermark table.

SQL Query:

```sql
SELECT LastLoadDate
FROM Watermark1
WHERE TableName='company_staff'
```

---

### 2. Set_LastLoadDate

Stores the retrieved watermark value in a pipeline variable.

Variable:

```text
varLastLoadDate
```

---

### 3. Copy_New_Records

Dynamically loads only records newer than the stored watermark value.

Dynamic Query:

```sql
SELECT *
FROM company_staff
WHERE ModifiedDate > @LastLoadDate
```

---

### 4. Update_Watermark

Executes a stored procedure that updates the watermark table with the current timestamp after a successful load.

Stored Procedure:

```sql
CREATE PROCEDURE UpdateWatermark1
AS
BEGIN
    UPDATE Watermark1
    SET LastLoadDate = GETDATE()
    WHERE TableName = 'company_staff'
END
```

---

## Key Concepts Demonstrated

* Incremental Loading
* Watermark Pattern
* Azure Data Factory Pipelines
* Variables and Expressions
* Lookup Activity
* Copy Activity
* Stored Procedures
* Dynamic SQL Queries
* Azure SQL Integration

---

## Benefits

* Loads only new data
* Reduces execution time
* Minimizes database load
* Improves pipeline efficiency
* Supports scalable ETL processes

---

## Learning Outcomes

Through this project, I gained hands-on experience with:

* Building Azure Data Factory pipelines
* Implementing enterprise ETL patterns
* Managing watermark tables
* Creating dynamic SQL queries
* Working with Azure SQL Database
* Automating incremental data ingestion

---

## Author

Drusya Suresh

Aspiring Data Engineer | Azure Data Factory | Azure Synapse Analytics | SQL | Python | ETL Development

