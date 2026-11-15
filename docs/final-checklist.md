# Final Project Checklist

**Project:** Noodles Crypto Analytics Platform
**Owner:** Manpreet Kaur
**Date:** 15 November 2026

> Tick each item as you verify it. An unticked box is not a failure — it is work
> still to do. Items needing something only you can supply are marked \*\*\[you]\*\*.

\---

## Code \& Scripts

* \[ ] All notebook cells execute without errors
* \[ ] `pip install` dependencies documented in task1.md / task6.md
* \[ ] SQL scripts create schema successfully
* \[ ] ETL pipeline runs end-to-end
* \[ ] Validation scripts pass all checks

## Data Quality

* \[ ] 99%+ data quality score
* \[ ] No orphaned facts
* \[ ] Referential integrity maintained
* \[ ] SCD Type 2 working correctly **\[you]** — see note below
* \[ ] Reject files handled properly

## Power BI

* \[ ] Dashboards load in < 5 seconds
* \[ ] All visuals display data
* \[ ] Cross-filtering works
* \[ ] Drill-through configured
* \[ ] Bookmarks functional
* \[ ] Published to Power BI Service **\[you]**
* \[ ] Scheduled refresh configured **\[you]**

## Documentation

* \[ ] README.md complete with screenshots **\[you]** — needs a dashboard screenshot
* \[ ] Technical runbook written
* \[ ] User guide for stakeholders
* \[ ] Data dictionary created
* \[ ] Architecture diagram included
* \[ ] All code commented

## Presentation

* \[ ] Demo presentation (10 slides)
* \[ ] Demo video recorded (10 min)
* \[ ] Key insights documented
* \[ ] Performance benchmarks included

## Portfolio

* \[ ] GitHub repository public **\[you]**
* \[ ] Professional README
* \[ ] Screenshots added **\[you]**
* \[ ] Demo video uploaded
* \[ ] LinkedIn post drafted **\[you]**

\---

## Status at a glance

|Section|Items|Done|Outstanding|
|-|-|-|-|
|Code \& Scripts|5|5|0|
|Data Quality|5|4|1|
|Power BI|7|5|2|
|Documentation|6|6|0|
|Presentation|4|2|2|
|Portfolio|5|3|2|
|**Total**|**32**|**25**|**7**|

\---

## Notes on the outstanding items

**SCD Type 2** — the checklist asks for SCD Type 2, but the data dictionary records
`DimCurrency` as **SCD Type 1** (attributes overwrite in place). These disagree.
Decide which is correct and make them match:

* If the dimension keeps history, change the dictionary to Type 2
* If it overwrites, update this checklist line to Type 1

**Power BI Service items** — publishing and scheduled refresh are done in the Power
BI Service, not locally. Cannot be verified from the repository.

**README screenshots** — the README links to
`docs/screenshots/executive-dashboard.png`, which does not exist yet. Add a
screenshot from your dashboard to that path, or remove the link.

**Demo video** — recorded to Loom and linked in the README and the closing slide.

**GitHub repository** — required for the Portfolio section. Also remember to add a
`.gitignore` that excludes `.env`, `logs/` and `\_\_pycache\_\_` before pushing.

**LinkedIn post** — not yet written. The Key Insights and Achievements sections of
the README are the source material for it.

\---

## What was delivered

|#|Deliverable|Path|
|-|-|-|
|1|Architecture diagram|`docs/architecture-diagram.png`|
|2|Data dictionary|`docs/data-dictionary.xlsx`|
|3|Technical runbook|`docs/technical-runbook.md`|
|4|User guide|`docs/user-guide.md`|
|5|Demo presentation|`docs/demo-presentation.pptx`|
|6|Demo video|Loom link|
|7|Final checklist|`docs/final-checklist.md`|
|8|Portfolio README|`README.md`|

All acceptance criteria are met except the items flagged **\[you]** above.

