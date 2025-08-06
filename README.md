# databricks_event_driven_pipeline
## Project Overview
This project demonstrates how to build an event-driven data ingestion pipeline in Databricks, using tools like Google Cloud Storage, Delta Lake, and Databricks Workflows. The pipeline automates the process of ingesting daily files into a staging Delta table, performing an SCD Type 1 merge into a target Delta table, and archiving processed files.

It simulates a common industrial use case: order or user activity tracking, where updated records need to be efficiently ingested and merged into a data lake with minimal manual effort.

## Core Objectives
- Automate incremental file ingestion from cloud storage.
- Maintain historical consistency via SCD Type 1 logic (upsert).
- Use Databricks Workflows to handle multi-step pipeline execution.
- Ensure all processed files are archived after ingestion.
- Keep the solution modular, maintainable, and production-ready.

## Tech Stack
- Google Cloud Storage
- Databricks Notebooks
- Delta Lake
- PySpark
- Databricks Workflows
- GitHub

## Implementation Highlights
This project follows a modular notebook-based structure in GitHub and implements the following pipeline steps:

1. Connect Databricks to GitHub
 - Repository structure organized by use-case-specific notebooks.
2. Load Daily Files to Staging Table
 - New files from a source folder in Google Cloud Storage are read into a staging Delta table in Databricks.
3. Archive Processed Files
 - Successfully loaded files are moved to an /archive/ folder to prevent reprocessing.
4. SCD Type 1 Merge into Target Table
 - Data from the staging table is merged into the target Delta table:
  - If the record exists, it's updated.
  - If it doesn’t exist, it's inserted.
5. Create and Run Databricks Workflow
 - Two sequential tasks:
   - Task 1: Load to staging.
   - Task 2: Merge into target.
 - Setup as a linear workflow.
6. Configure Event Trigger
 - Pipeline is triggered automatically when a new file arrives in the source folder (event-driven setup).

## Example Scenario
Imagine you're receiving daily customer or user activity logs:
- Day 1:
  file1.csv has user 123 logging in. This is inserted into the target table.
- Day 2:
  file2.csv shows the same user making a purchase.
The pipeline updates the record in the target table from "login" to "purchase".
This illustrates the incremental nature of the pipeline and the SCD Type 1 logic.

## Workflow Overview
1️⃣ File lands in /source/ folder (GCS)
    
    ⬇    
2️⃣ Databricks Workflow is triggered
   
    ⬇    
3️⃣ Task 1: Load data into staging table (overwrite)
   
    ⬇  
4️⃣ Task 2: Merge staging data into target table (upsert)
    
    ⬇    
5️⃣ Archive the processed file to /archive/



