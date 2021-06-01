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

### Process Chain

By default, the process tab is collapsed to only show "process chains". A process chain is a set of processes that have been executed as a series with one process in the chain causing the next to be triggered. The filters mentioned above will filter the process chains based on whether they contain any matching process inside of them.

The fields displayed for process chains are as follows:

* **Process ID** - This field is only used for individual processes, not chains. Clicking the arrow in this column will open the individual process view. More information about the individual process view is available below.
* **Timeline** - The total execution time of the process chain displayed in seconds. The bars displayed for each process chain timeline are relative to each other in display with overlaps indicating that the two process chains were running at the same time.  
* **Scope** - This field indicates the grain at which the process chain was started. Possible values include _Input, Source, and Output Channel_. 
* **ID -** This field is only used for individual processes, not chains.
* **Status** - Indicates the overall health of the processes that occurred as part of the process chain. 
  * ![](../.gitbook/assets/completed.png)  Everything has processed correctly
  * ![](../.gitbook/assets/failed.png)  A failure has occurred in the process chain
  * ![](../.gitbook/assets/inprogress.png)  The process chain has a currently running input
  * ![](../.gitbook/assets/image%20%28291%29.png)The process chain is launching a new cluster
  * ![](../.gitbook/assets/image%20%28351%29.png) The process chain has encountered an error but recovered successfully
* **Cost -** An estimate of the cost in cents to run the process chain. Estimated by taking the average VM spot price _x_ number of workers _x_ run time.
* **\#** - This field is only used for individual processes, not chains.
* **Log -** This field is only used for individual processes, not chains.
* **Parameters** - This field is only used for individual processes, not chains.

### Individual Process View

Expanding a process chain will show the individual processes included within it. Each individual process represents a unit of work with parameters, logs, and execution steps. Each process is configured to automatically enqueue the next process steps upon its completion. The filters mentioned above will filter the individual processes.

The fields displayed for individual processes are as follows:

* **Process ID** - The unique identifier of the process
* **Timeline** - The execution timeline of the individual process. The white box indicates time spent in the queue while not executing. The blue box indicates time spent processing. 





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

