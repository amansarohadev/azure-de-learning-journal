# Day 2: Azure Data Factory — Orchestration Basics

**Date:** February 5, 2026

## What I Learned Today

### ADF Core Concepts
- **Azure Data Factory = Orchestration tool** for data pipelines
- Think of it as the "control center" that moves and transforms data between systems

### Key Terms
- **Source:** Where data comes from (e.g., SQL database, Blob storage)
- **Sink:** Where data goes (ADF's term for "destination")
- **Linked Service:** Connection to a data store (like a connection string)
- **Dataset:** Represents the data structure at source or sink
- **Pipeline:** The workflow that orchestrates activities

### Activities I Worked With

**1. Get Metadata Activity**
- Used to retrieve file properties (names, size, last modified, etc.)
- Useful when you need to process multiple files dynamically
- Example: Get all file names in a Blob container, then loop through them

**2. ForEach Loop**
- Iterate over a collection (like file names from Get Metadata)
- Runs activities for each item in the array
- Can run sequentially or in parallel

**3. If Condition**
- Conditional logic in pipelines
- Example: "If file name starts with 'sales_', do X, else do Y"
- Syntax: `@startswith(item().name, 'sales_')`

**4. Copy Activity**
- Core activity for moving data from source to sink
- Can copy from SQL → Blob, Blob → SQL, etc.
- Supports schema mapping and data type conversion

**5. Data Flow**
- Visual transformation tool inside ADF
- Familiar if you know SQL or Python (similar functions)
- Transformations: filter, join, aggregate, derive columns, etc.

### Parameterization (Game-changer)
Instead of hardcoding file names:
```
Bad: fileName = "sales_2024.csv"
Good: fileName = @pipeline().parameters.inputFileName
```

This makes pipelines reusable. One pipeline can process any file you pass to it.

### ADF Expressions I Used

| Expression | What It Does | Example |
|------------|--------------|---------|
| `@startswith(item().name, 'prefix')` | Check if string starts with a value | Check if filename starts with "sales_" |
| `@pipeline().parameters.paramName` | Reference a pipeline parameter | Get dynamic file name |
| `@activity('activityName').output` | Get output from previous activity | Use Get Metadata results in ForEach |
| `@item()` | Current item in a ForEach loop | Access each file name in loop |

## Real-World Pattern I Built

**Problem:** Process all CSV files in a Blob container, but only if they start with "sales_"

**Solution:**
1. Get Metadata → retrieve all file names in container
2. ForEach → loop through each file
3. If Condition → check if `@startswith(item().name, 'sales_')`
4. Copy Activity (inside If = True) → copy file to destination

This is a dynamic, scalable pattern. No hardcoded file names.

## Data Flow vs Copy Activity

**Copy Activity:**
- Simple extract and load (EL)
- Minimal transformation (e.g., column mapping)
- Fast, lightweight

**Data Flow:**
- Full transformation capability (ETL)
- Visual, code-free (but uses Spark under the hood)
- Use when you need complex transformations (joins, aggregations, filters)

The SQL and Python knowledge I already have transferred directly here. Same logic, different UI.

## What Clicked Today
- **Parameterization is everything.** Without it, pipelines are brittle.
- **ForEach + If Condition = powerful combo.** You can build smart, conditional pipelines.
- **Data Flow isn't scary.** If you know SQL/Python, you already know this.

## Tomorrow's Plan
- Triggers (schedule, event-based)
- Set Variable activity
- Storage Event Trigger (real-time pipeline triggering)
- Delete activity
- Debugging techniques
- End-to-end pipeline build

## Questions I Still Have
- Performance tuning: When does ForEach become too slow? When to use parallel execution?
- Cost implications: Data Flow uses Spark clusters. How much does that actually cost at scale?

---

**Status:** ✅ ADF basics solid. Moving to triggers and real-time scenarios next.
