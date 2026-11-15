# Technical Runbook: Noodles Crypto Analytics

**System:** Noodles Crypto Analytics — social engagement ETL and reporting platform
**Database:** MySQL (`noodles\\\_dw`)
**Document type:** Operations runbook
**Last updated:** 2026-11-15

> \\\*\\\*How to read this document.\\\*\\\* Every command, path, table and column name below is
> either taken from the project brief or marked. Anything marked \\\*\\\*`\\\[CONFIRM]`\\\*\\\* is a
> placeholder you must replace with your real value before this runbook is used in
> production. Search the file for `\\\[CONFIRM]` and work through them in order.

\---

## 1\. System Overview

Noodles Crypto Analytics is a Jupyter-notebook-based ETL pipeline written in Python
with pandas. It ingests social engagement and market data, loads it into a MySQL
star-schema warehouse (`noodles\\\_dw`), and exposes aggregated tables to Power BI for
executive reporting.

**All pipeline work runs inside Jupyter notebooks.** There are no standalone `.py`
runner scripts — the notebooks are the entry points, and anything shared between them
is imported as a module.

|Component|Technology|Location|
|-|-|-|
|Extract / Transform|Python + pandas, run in Jupyter|`05\\\_...ipynb`, `06\\\_...ipynb`|
|Database|MySQL — `noodles\\\_dw`|`\\\[CONFIRM]` local or hosted MySQL server|
|DB Connectivity|SQLAlchemy, MySQL connector|`\\\[CONFIRM]` shared module imported by notebooks|
|Aggregation|Python + pandas, run in Jupyter|`06\\\_powerbi\\\_prep.ipynb`|
|Reporting|Power BI Desktop / Service|`reports/\\\*.pbix`|
|Scheduling|cron (Linux/macOS) or Task Scheduler (Windows)|`\\\[CONFIRM]` host machine|

**Power BI connectivity.** Reports connect to `noodles\\\_dw` through either the MySQL
connector (recommended for Power BI Service) or an ODBC DSN on the gateway machine.
The connector choice is fixed per `.pbix` file — changing it requires a report
republish.

### Data flow

```
JSON sources  →  Jupyter ETL (Python/pandas)  →  MySQL noodles\\\_dw  →  Power BI
                 staging + quality               star schema          dashboards
                 checks + logging                + aggregations
```

\---

## 2\. Daily Operations

### 2.1 Manual ETL Execution

Run this to refresh outside the scheduled window, or when investigating a failed
automated run.

```bash
# 1. Go to the project root
cd C:\\\\Users\\\\singh\\\\noodles-analytics

# 2. Open Anaconda Prompt (the environment is provided by Anaconda —
#    there is no venv folder in this project)

# 3. Run the Task 5 warehouse load, then the Task 6 aggregation notebook
jupyter nbconvert --execute --to notebook ^
  --output-dir logs/executed ^
  --ExecutePreprocessor.timeout=1800 ^
  05\\\_data\\\_warehouse\\\_design.ipynb 06\\\_powerbi\\\_prep.ipynb

# Linux / macOS equivalent
# source venv/bin/activate \\\&\\\& jupyter nbconvert --execute --to notebook \\\\
#   --output-dir logs/executed --ExecutePreprocessor.timeout=1800 \\\\
#   05\\\_data\\\_warehouse\\\_design.ipynb 06\\\_powerbi\\\_prep.ipynb
```

**Why `--output-dir` is in the command.** Without it, `nbconvert` writes the
executed notebook back over your original file, so every run silently overwrites the
notebook you are editing. `--output-dir logs/executed` keeps the originals intact and
leaves a dated record of each executed run.

**Expected duration:** 5–10 minutes.

**Success indicators:**

* The process exit code is `0` (`echo $?` after the run).
* The run log records a success line — `\\\[CONFIRM]` exact wording your pipeline prints.
* No new `ERROR` entries in `logs/` from this run.
* Aggregation table row counts have moved since the previous run.

### 2.2 Automated Execution (Cron / Task Scheduler)

#### Linux / macOS — cron

```cron
# Run daily at 06:00
0 6 \\\* \\\* \\\* cd /path/to/noodles-analytics \\\&\\\& source venv/bin/activate \\\&\\\& jupyter nbconvert --execute --to notebook --output-dir logs/executed --ExecutePreprocessor.timeout=1800 06\\\_powerbi\\\_prep.ipynb >> logs/cron.log 2>\\\&1
```

Install with `crontab -e`; verify with `crontab -l`. Cron does not inherit your shell
environment, so the line activates the virtual environment itself — this is the most
common reason a job that works by hand fails on the schedule.

#### Windows — Task Scheduler

1. Open **Task Scheduler**.
2. **Create Basic Task** → name it `Noodles ETL`.
3. **Trigger:** Daily at 6:00 AM.
4. **Action:** Start a program.

   * **Program/script:** `C:\\\\path\\\\to\\\\noodles-analytics\\\\venv\\\\Scripts\\\\jupyter.exe` `\\\[CONFIRM]`
   * **Arguments:** `nbconvert --execute --to notebook --output-dir logs\\\\executed --ExecutePreprocessor.timeout=1800 06\\\_powerbi\\\_prep.ipynb`
   * **Start in:** `C:\\\\path\\\\to\\\\noodles-analytics\\\\notebooks\\\\` `\\\[CONFIRM]`
5. Tick **Run whether user is logged on or not**, and enable **Run task as soon as
possible after a scheduled start is missed** so a sleeping machine does not
silently skip a day.
6. Right-click the task → **Run** once, and confirm it completed, before relying on
the schedule.

> \\\*\\\*Start in\\\*\\\* matters. The notebook resolves data paths relative to its working
> directory, so an empty \\\*\\\*Start in\\\*\\\* field produces a job that runs fine manually
> and fails on the trigger.

### 2.3 Power BI Refresh

#### Manual refresh (Power BI Desktop)

1. Open the relevant `.pbix` from `reports/`.
2. **Home → Refresh**.
3. Wait 30–60 seconds.
4. Confirm the **Refresh** timestamp in the status bar has updated.

#### Scheduled refresh (Power BI Service)

* Configured for **daily at 7:00 AM** — one hour after the ETL window, so it reads a
fully written warehouse.
* Verify under **dataset settings → Refresh history**.
* If refreshes fail, check the on-premises data gateway first; gateway connectivity
is the most frequent cause.

\---

## 3\. How to Verify It Worked

Run these after any manual or automated execution.

|#|Check|How|Expected|
|-|-|-|-|
|1|Notebook exit status|Read the run log|Success line present, exit code `0`|
|2|Error scan|Search `logs/` for `ERROR`|No new matches for this run|
|3|Fact table loaded|`SELECT COUNT(\\\*) FROM FactSocialEngagement;`|Greater than the previous run|
|4|Dimensions intact|`SELECT COUNT(\\\*) FROM DimCurrency;`|78|
|5|Date coverage|`SELECT MAX(DateKey) FROM DimDate;`|Today or the previous day `\\\[CONFIRM]` column name|
|6|Aggregations fresh|`SELECT COUNT(\\\*) FROM DailySocialSummary;`|Row count changed since last run|
|7|Power BI refreshed|Refresh history in the Service|Completed, timestamp current|

```sql
-- Quick post-run sanity query
SELECT
    (SELECT COUNT(\\\*) FROM FactSocialEngagement) AS fact\\\_rows,
    (SELECT COUNT(\\\*) FROM DimCurrency)          AS currencies,
    (SELECT COUNT(\\\*) FROM DailySocialSummary)   AS daily\\\_summary\\\_rows;
```

> \\\*\\\*Before first use:\\\*\\\* open MySQL and run `SHOW TABLES;` followed by
> `DESCRIBE <table>;` for each table named above. Replace any column name that does
> not match. Do not run these queries from this document until you have.

\---

## 4\. Troubleshooting

### 4.1 Notebook fails or exits non-zero

1. Read the newest file in `logs/` — the failing stage is named in the last `ERROR`.
2. Re-run the notebook interactively in JupyterLab or Jupyter Notebook. A cell that
errors on the command line but runs by hand points to an environment problem, not
a code fault: almost always the wrong kernel, or the virtual environment not
activated.
3. Check that the source JSON files exist and are non-empty.
4. Correct the cause and re-run. The pipeline is idempotent, so a repeat run is safe.

### 4.2 Database connection errors

|What you see|Likely cause|What to do|
|-|-|-|
|`Access denied for user`|Wrong credentials or a revoked grant|Verify the DB user in the connection settings; re-grant if needed|
|`Can't connect to MySQL server`|MySQL service stopped, or wrong host/port|Confirm the service is running; check host and port `3306`|
|`Unknown database 'noodles\\\_dw'`|Database not created on this host|Re-run the Task 5 schema-creation notebook|
|Timeout during a large load|Connection dropped mid-insert|Reduce batch size, or raise the connection timeout|

### 4.3 Quality check failures

A failing quality check means the run is **unsuccessful**, even when the process
exited cleanly.

* **Null-rate threshold breached** — inspect that column in the source JSON; an
upstream schema change is the usual cause.
* **Duplicate rows detected** — confirm the source export was not run twice into the
same landing directory.
* **Schema drift detected** — a source platform changed its payload; update the
staging normalisation cell before the next run.
* **Quarantine populated** — review the quarantined records, fix or accept them, then
re-run.

### 4.4 Power BI refresh failures

1. Check **dataset settings → Refresh history** for the error text.
2. **Gateway offline** — restart the on-premises data gateway service and retry.
3. **Credentials expired** — re-enter the MySQL credentials on the dataset's data
source settings.
4. **Timeout on large tables** — confirm Power BI reads the pre-aggregated tables
(`CurrencySummary`, `DailySocialSummary`, `SocialEngagementSummary`,
`FactSocialEngagementEnriched`) rather than the raw fact table.
5. **Stale numbers after a refresh** — verify the `.pbix` points at the same database
the notebooks write to.

\---

## 5\. Routine Maintenance

|Frequency|Task|
|-|-|
|Daily|Confirm the scheduled notebook run and the 7:00 AM Power BI refresh both completed|
|Weekly|Review `logs/` for recurring warnings; check disk space on the DB host|
|Monthly|Verify row-count growth against expectations; archive logs older than 30 days|
|Per source change|Update the staging normalisation cell and re-run the quality checks|
|Per report change|Republish the `.pbix` and confirm credentials on the Service|

\---

## 6\. Escalation

|Level|Scope|Owner|
|-|-|-|
|L1|Re-run the notebooks, verify outputs, restart the gateway|On-call analyst|
|L2|Source schema changes, quality-check tuning, SQL fixes|Data engineer|
|L3|Database host, credentials, infrastructure|Platform / DBA|

`\\\[CONFIRM]` Replace the roles above with real names and contact details before this
runbook is shared.

Log every incident with the date, symptom, action taken and outcome, so a recurring
fault becomes visible over time.

\---

## 7\. Reference

### Key paths

|Path|Contents|
|-|-|
|Project root|Jupyter notebooks: `01`–`06\\\_\\\*.ipynb` (project root, no subfolder)|
|`etl/`|Shared Python modules imported by the notebooks `\\\[CONFIRM]` if used|
|`data/raw/`|Landing area for source JSON|
|`logs/`|Notebook-run and scheduler logs|
|`reports/`|Power BI `.pbix` workbooks|
|`docs/`|Documentation, including this runbook|

### Source files

|File|Purpose|
|-|-|
|`v2\\\_token.json`|Currency metadata|
|`v2\\\_token\\\_overview.json`|Market snapshot|
|`v4\\\_x\\\_tweets.json`|Twitter engagement|
|`v5\\\_reddit\\\_post\\\_engagement.json`|Reddit engagement|

### Notebooks

|Notebook|Task|Purpose|
|-|-|-|
|`05\\\_data\\\_warehouse\\\_design.ipynb`|Task 5|Load the star schema|
|`06\\\_powerbi\\\_prep.ipynb`|Task 6|Aggregations and Power BI views|

\---

## 8\. Open Items to Confirm

Work through this list once, then delete the section and every `\\\[CONFIRM]` marker.

|#|Item|Where|Replace with|
|-|-|-|-|
|1|Repository root path|§2.1, §2.2|Your real absolute path|
|2|Virtual environment folder name|§2.1, §2.2|Usually `venv` or `.venv`|
|3|Success log line wording|§2.1|The exact line your notebook prints|
|4|Table and column names in §3|§3|Verified against `SHOW TABLES;`|
|5|MySQL host, port, DB user|§1, §4.2|Your connection settings|
|6|Power BI gateway name|§2.3|Your gateway's name|
|7|Escalation contacts|§6|Real names and contacts|
|8|Notebook folder name|§2.2, §7|Your actual folder name|



