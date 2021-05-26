---
description: >-
  The processing page provides a real-time overview of the system, including
  those processes currently waiting as part of the workflow engine's dependency
  queue
---

# Processing

## Processing Queue

![Sample Processing Queue](../.gitbook/assets/image%20%28350%29.png)

The Processing Queue tab provides an interactive overview of all processes completed, active, errored or otherwise in the system, filtered by day \(defaulted to today\).

Processes are grouped by initiating operation, which can include, but are not limited to: new inputs, manual executions, and system maintenance processes. Users are then able to click on a line representing an initiating operation to view all downstream processes in the workflow dependency chain and navigate to the appropriate sub-detail of the process to triage or debug an issue.

Each Column in the table provides details which are helpful in understanding performance and underlying execution at a glance:

* **Process Id:** A globally unique Id for this process
* **Timeline:** A rough sketch overview of what sub-processes happened when in relation to the other visible timelines on the page.
  * Clear/empty portions represent cluster startup or other infrastructure related activity where no data processing is being performed, and no cloud service charges are accrued
  * Black/filled portions represent active processing by the system
* **Scope:** Each sub-process has a scope of operation
  * Input: This process only operated on a single input
  * Source: This process operated on all data stored in the hub table for across all inputs
  * Output Channel: This process used data from a specific Channel within a Source to Output Channel mapping
* **Id:** For this specific Scope, the ID of the affected configuration elements and data
* **Operation:** The name of the specific [Processing Step](../logical-architecture-overview/data-processing-engine/data-processing-1.md) executed
* **Job Id:** The [Databricks Job Id](https://docs.databricks.com/jobs.html), with a hyperlink to the Databricks UI for further debugging
* **Status:** An icon and help-text indicating the result of the Processing Step
* **Cost, c:** An _estimate_ of the cost to execute a specific job, based on published costs by the cloud vendor and Databricks multiplied by runtime, in cents.
* **\#:** Processed record counts popup to quickly access and display metadata about the data processed

