# Day 4: Debugging Pipelines & Execute Pipeline Activity

**Date:** February 7, 2026

## What I Learned Today

### Debugging Pipelines in Azure Data Factory

**What debugging does:**
- Test pipeline runs without triggering actual execution on live data
- Step through activities one by one
- Inspect inputs, outputs, and errors at each step
- Validate logic before deploying to production

**How to debug:**
1. Open pipeline in ADF Studio
2. Click "Debug" button (top toolbar)
3. Pipeline runs in debug mode
4. View activity outputs in real-time
5. Check for errors without affecting production data

**Key debugging features:**

#### 1. Activity Output Inspection
- Click on any activity after debug run
- View "Output" tab → see exact JSON output
- View "Input" tab → see what data was passed in
- View "Details" tab → execution time, status, error messages

#### 2. Debug with Parameters
- Set parameter values specifically for debug run
- Test different scenarios without changing pipeline code
- Example: Debug with test file name vs production file name

#### 3. Breakpoint-like Behavior
- Debug runs sequentially
- Can see exactly where pipeline fails
- Output of each activity visible before next one runs

#### 4. Error Messages
- ADF provides detailed error messages
- Common errors:
  - **Source not found** → check Linked Service connection
  - **Schema mismatch** → verify column mappings
  - **Permission denied** → check service principal/managed identity permissions
  - **Timeout** → increase activity timeout settings or optimize query

**Debugging workflow:**
Write pipeline logic
↓
Debug with test data
↓
Check each activity output
↓
Fix errors
↓
Debug again
↓
Once successful → publish

**Why this matters:**
Debugging BEFORE publishing prevents production failures. Catch errors early when they're cheap to fix.

---

### Execute Pipeline Activity

**What it does:**
Run one pipeline from inside another pipeline (pipeline orchestration)

**Why you need this:**
- Break complex workflows into smaller, reusable pipelines
- Better maintainability (small pipelines easier to debug)
- Parallel execution of independent workflows
- Conditional pipeline execution

**How it works:**
1. Create a "master" pipeline
2. Add "Execute Pipeline" activity
3. Select which child pipeline to run
4. Pass parameters from master to child pipeline
5. Handle success/failure of child pipeline

**Pattern 1: Sequential Execution**
Master Pipeline:
Execute Pipeline A (data extraction)
↓ (on success)
Execute Pipeline B (data transformation)
↓ (on success)
Execute Pipeline C (data loading)

**Pattern 2: Parallel Execution**
Master Pipeline:
├─ Execute Pipeline A (process region 1 data)
├─ Execute Pipeline B (process region 2 data)
└─ Execute Pipeline C (process region 3 data)
All run simultaneously

**Pattern 3: Conditional Execution**
Master Pipeline:
Get Metadata
↓
If Condition (check file count)
↓
If True: Execute Pipeline A
If False: Execute Pipeline B

**Parameter passing:**
- Master pipeline parameters → child pipeline parameters
- Use `@pipeline().parameters.paramName` syntax
- Enables dynamic, reusable child pipelines

**Dependency management:**
- Set "On Success" vs "On Failure" vs "On Completion" dependencies
- Control execution flow based on child pipeline results
- Handle errors gracefully

---

## Real-World Use Cases

### Use Case 1: Modular ETL
**Before:** One giant pipeline with 20 activities. Hard to debug. Hard to maintain.

**After:** 
- Master pipeline calls:
  - Extract pipeline (gets data from sources)
  - Transform pipeline (cleans and transforms)
  - Load pipeline (writes to destination)

Each pipeline is small, focused, reusable.

### Use Case 2: Multi-Source Ingestion
**Scenario:** Ingest data from 3 different sources simultaneously

**Solution:**
Master pipeline with 3 Execute Pipeline activities running in parallel:
- Pipeline A: Ingest from SQL Database
- Pipeline B: Ingest from Blob Storage
- Pipeline C: Ingest from REST API

All finish → master pipeline proceeds to transformation step.

### Use Case 3: Error Handling & Retry
**Scenario:** If data extraction fails, try an alternate source

**Solution:**
Execute Pipeline (Primary Source)
↓ On Failure
Execute Pipeline (Backup Source)
↓ On Success
Continue processing

---

## What Clicked Today

**Debugging is not optional.**
Every pipeline should be debugged before publishing. Production errors are expensive. Debug errors are free.

**Execute Pipeline = modularity.**
Instead of one massive pipeline, build small, focused pipelines and orchestrate them. Easier to test. Easier to reuse.

**Parameter passing is the key.**
Master pipeline can pass dynamic values to child pipelines. One master pipeline can handle multiple scenarios by passing different parameters.

---

## Common Debugging Patterns I Used

### Pattern 1: Check Source Connection
Debug pipeline
↓
If "Source not found" error
↓
Check Linked Service
↓
Test connection
↓
Fix credentials/connection string
↓
Debug again

### Pattern 2: Validate Output
Debug pipeline
↓
Click on Copy Activity
↓
View "Output" tab
↓
Check "rowsCopied" count
↓
If 0 rows → check source query/filter

### Pattern 3: Schema Mismatch
Debug pipeline
↓
"Schema mismatch" error
↓
Check source vs sink column mappings
↓
Fix data type conversions
↓
Debug again

---

## Tomorrow's Plan (Monday)

**End-to-End Data Pipeline Project:**
Build a complete pipeline from scratch that demonstrates everything learned so far:
- Multiple sources
- Transformations
- Conditional logic
- Automated triggers
- Error handling
- Proper debugging

This will be the portfolio piece that shows I can build real pipelines, not just follow tutorials.

---

## Questions I Still Have
- Execute Pipeline Activity: Is there a limit to how many child pipelines you can call?
- Debugging: Can you debug only a specific section of a pipeline, or must you debug the entire thing?
- Performance: Does Execute Pipeline add significant overhead vs having everything in one pipeline?

---

**Status:** ✅ Debugging and orchestration learned. Ready for end-to-end project Monday.

**Taking Sunday off.** Rest and recharge.
