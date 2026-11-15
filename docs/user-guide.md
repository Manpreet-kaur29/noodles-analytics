# User Guide: Noodles Crypto Dashboards

**For:** Business stakeholders, analysts and executives
**Covers:** The three Power BI reports in the Noodles Analytics workspace
**Last updated:** 2026-11-15

> \\\*\\\*In a hurry?\\\*\\\* Section 3 tells you which dashboard to open. Section 6 explains what
> the metrics mean. If a number looks wrong, section 8 answers the usual questions.

\---

## 1\. Overview

The Noodles Crypto Dashboards turn daily social engagement and market data into
market insights you can act on. Data is collected from Twitter, Reddit and market
sources, cleaned automatically, and published to Power BI every morning.

This guide explains how to reach the dashboards, what each one shows, and how to get
numbers out of them for your own reports.

### How the data reaches you

```
Twitter, Reddit, market data  →  automatic daily refresh  →  Power BI dashboards
```

You do not need to load or prepare anything. Open a report and the current data is
already there.

\---

## 2\. Accessing the Dashboards

### Option 1 — Power BI Service (recommended)

Works from any browser, on any device, and always shows the current data.

1. Go to [https://app.powerbi.com](https://app.powerbi.com)
2. Sign in with your organisational account
3. Open the **Noodles Analytics** workspace
4. Click the report you want — see section 3

### Option 2 — Power BI Desktop

Use this when you need to work offline, or want to build your own analysis on top of
the data.

1. Download the `.pbix` file from the shared folder
2. Open it with Power BI Desktop
3. Click **Refresh** to load the latest data

> The Desktop copy is a snapshot. It only updates when you press \\\*\\\*Refresh\\\*\\\*, so its
> numbers can be older than the Service.

### Which should I use?

|Situation|Use|
|-|-|
|Checking today's numbers|Power BI Service|
|Sharing with a colleague|Power BI Service|
|Working offline, or on a plane|Power BI Desktop|
|Building your own report|Power BI Desktop|

\---

## 3\. Available Dashboards

### 3.1 Executive Dashboard

**Purpose:** A high-level market overview for leadership and decision-making.

**Key metrics:**

* Total market capitalisation
* Average price across currencies
* Trading volume (24h)
* Number of active currencies

**How to use it:**

* Use the date slicer at the top-right to choose a time period
* Click any visual to filter the others — this is cross-filtering
* Hover over a data point to see a detailed tooltip

### 3.2 Top Performers Report

**Purpose:** Identify market winners and losers at a glance.

**Features:**

* Top gainers table, sorted by percentage change
* Top losers table
* Volume leaders
* Price trend indicators

**How to use it:**

* Adjust the **Top N** parameter — it defaults to 10
* Right-click any currency to drill through to its detail page
* Use the **...** menu to export the data

### 3.3 Currency Deep Dive

**Purpose:** Detailed analysis of an individual currency.

**Features:**

* Price trends with moving averages
* Volume analysis
* Social engagement metrics
* Historical comparisons

**How to use it:**

* Reach it by drill-through from either report above, or
* Use the currency selector dropdown, or
* **Ctrl + Click** several currencies to compare them side by side

\---

## 4\. Common Tasks

### Filter by date range

1. Find the date slicer — usually top-right
2. Click the calendar icon
3. Pick a start and end date, or
4. Use a relative option such as **Last 30 days** or **YTD**

### Export data

1. Click the **...** menu on the visual you want
2. Choose **Export data**
3. Pick a format — `.xlsx` or `.json`
4. Open it in Excel for further analysis

### Print or export the report

1. **File → Export → PDF**
2. Choose the pages to include
3. Adjust the page size if the layout needs it
4. Save to your chosen location

### Share with colleagues

1. Click **Share**, top-right
2. Enter the email addresses
3. Set permissions — can view, or can edit
4. Add a short message and send

\---

## 5\. Understanding Key Metrics

|Metric|What it means|How to use it|
|-|-|-|
|Market Cap|Total value of all coins|Gauge overall market size|
|24h Volume|Trading activity in the last 24 hours|Identify liquidity|
|Price Change %|How far the price moved|Spot trends|
|Moving Average|A smoothed trend line|Identify direction|
|Volatility|How stable the price is|Assess risk|

\---

## 6\. Tips \& Best Practices

**Do:**

* Refresh before an important meeting, so you are not presenting yesterday's numbers
* Use filters to focus on the currencies that matter to your decision
* Export data when you want to analyse further in Excel
* Check the **Last Refresh** timestamp in the report header before you quote a figure

**Don't:**

* Make decisions on stale data — check the refresh time first
* Ignore data quality warnings when they appear
* Share reports externally without checking whether they contain sensitive data

\---

## 7\. Frequently Asked Questions

**How often is the data updated?**
Daily at 6:00 AM. The **Last Refresh** timestamp in the report header tells you when
the visible data was pulled.

**Why do the numbers differ from yesterday's figures for the same date?**
Data is refreshed daily, and historical values can be revised when a source corrects
or backfills its records. Compare like with like by checking the refresh timestamp.

**Can I build my own reports?**
Yes, if you have a Power BI Pro licence. Your IT team can arrange access.

**Who do I contact with a question about the numbers?**
The data team — see section 8.

\---

## 8\. Support

For technical problems, or a question about what a number means:

|Channel|Contact|Best for|
|-|-|-|
|Email|support@noodles.com|Anything that needs a written trail|
|Slack|#data-help|Quick questions|
|Phone|ext. 1234|Urgent problems during working hours|

When you report a problem, include the report name, the timestamp shown in the
header, and a screenshot if you have one. That is usually enough to identify the
cause without a follow-up round.

\---

## 9\. Glossary

|Term|Plain meaning|
|-|-|
|Cross-filtering|Clicking one visual to filter the others on the page|
|Drill-through|Jumping from a summary number to the detail behind it|
|Slicer|The filter control at the top of a report page|
|Last Refresh|When the data you are looking at was last updated|
|Workspace|The shared folder in Power BI Service that holds the reports|



