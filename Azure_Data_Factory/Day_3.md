# Day 3: ADF Triggers & Real-Time Automation

**Date:** February 6, 2026

## What I Learned Today

### Triggers in Azure Data Factory

Triggers = what starts your pipeline automatically (instead of manual runs)

#### 1. Schedule Trigger
- Time-based pipeline execution
- Like a cron job for data pipelines
- Examples:
  - Run every day at 2 AM
  - Run every Monday at 9 AM
  - Run every hour on weekdays

**Use case:** Nightly batch loads, daily report generation, scheduled ETL jobs

**Configuration:**
- Set frequency (minute, hour, day, week, month)
- Set start time and optional end time
- Can add recurrence patterns (e.g., "every Monday and Thursday")

**When to use:** Predictable, time-based workloads

---

#### 2. Storage Event Trigger
- Event-based pipeline execution
- Runs automatically when files are added/modified/deleted in Azure Blob Storage or ADLS Gen2
- Near real-time processing

**Use case:** Process files as soon as they land in storage

**Configuration:**
- Specify storage account and container
- Set event type: 
  - Blob created (new file uploaded)
  - Blob deleted (file removed)
- Can filter by file path pattern (e.g., `/data/sales/*.csv`)

**When to use:** Real-time or near-real-time data ingestion

**Key insight:** This is how you build event-driven pipelines. File lands → pipeline runs automatically. No schedule needed.

---

### Set Variable Activity

**What it does:** Create and assign values to variables within a pipeline

**Why it matters:**
- Store intermediate results
- Pass data between activities
- Build conditional logic based on computed values

**Example pattern:**
1. Get Metadata → count files in a container
2. Set Variable → store file count as a variable
3. If Condition → if count > 0, process files; else, skip

**Variable types:**
- String
- Bool
- Array

**Common use:** Dynamic pipeline behavior based on runtime conditions

---

### Delete Activity

**What it does:** Delete files or folders from storage (Blob, ADLS Gen2, etc.)

**Use cases:**
- Clean up staging/temp files after processing
- Archive old files
- Implement data retention policies

**Example pattern:**
1. Copy Activity → move data from source to destination
2. Delete Activity → remove source files after successful copy

**Important:** Always validate copy success BEFORE deleting source files. Use If Condition or dependency settings.

---

### Real-Time Scenarios I Worked Through

#### Scenario 1: Automated File Processing
**Requirement:** Process CSV files as soon as they land in Blob storage

**Solution:**
- Storage Event Trigger (blob created event)
- Filter: `/incoming/*.csv`
- Pipeline processes file immediately on arrival

**Why this matters:** No manual intervention. No schedule delays. Files processed within seconds of upload.

---

#### Scenario 2: Dynamic File Cleanup
**Requirement:** After copying files to archive storage, delete source files only if copy succeeded

**Solution:**
1. Copy Activity (source → archive)
2. If Condition → check copy activity succeeded
3. Delete Activity (inside True branch) → remove source files

**Why this matters:** Prevents data loss. Only deletes after confirmed successful copy.

---

#### Scenario 3: Daily Batch with Variable Control
**Requirement:** Run pipeline daily, but only if new files exist

**Solution:**
1. Schedule Trigger (daily at 2 AM)
2. Get Metadata → count files in staging folder
3. Set Variable → store count
4. If Condition → if count > 0, run pipeline; else, skip

**Why this matters:** Saves compute costs. Pipeline doesn't run if there's nothing to process.

---

## Key Patterns Learned

### Pattern 1: Event-Driven PipelineStorage Event Trigger → Copy Activity → Data Flow → Delete Source Files
Use for: Real-time ingestion

### Pattern 2: Scheduled + ConditionalSchedule Trigger → Get Metadata → Set Variable → If Condition → Process/Skip
Use for: Cost-optimized batch processing

### Pattern 3: Safe DeleteCopy Activity → If (Success) → Delete Activity
Use for: File archiving with data safety

---

## What Clicked Today

**Storage Event Trigger = game-changer for real-time pipelines.**
Before: Schedule trigger runs every hour, checks for files, wastes compute if nothing is there.
After: Event trigger only runs when a file actually lands. Instant processing. Zero waste.

**Variables = control flow.**
You can build smart pipelines that adapt based on what they find at runtime.

**Delete Activity = must be used carefully.**
Always validate success before deleting source data. One wrong If Condition = data loss.

---

## Tomorrow's Plan
- Debugging pipelines (how to troubleshoot when things break)
- Execute Pipeline Activity (how to call one pipeline from another)
- End-to-end data pipeline build (putting it all together)

---

## Questions I Still Have
- Storage Event Trigger latency: How fast is "near real-time"? Seconds? Minutes?
- Cost: Does Storage Event Trigger constantly poll storage, or is it truly event-based?
- Execute Pipeline Activity: When should you split pipelines vs keep everything in one?

---

**Status:** ✅ Triggers and automation solid. Ready for debugging and end-to-end build tomorrow.
