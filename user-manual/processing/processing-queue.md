# Processing Queue

![Sample Processing Queue](<../../.gitbook/assets/image (350).png>)

The Processing Queue tab provides an interactive overview of all processes completed, active, errored or otherwise in the system, filtered by day (defaulted to today).

By default, the process tab is collapsed to only show "process chains". A process chain is a set of processes that have are grouped by their initializing process and executed as a series with one process in the chain causing the next to be triggered via DataOps workflow engine.

Each process chain can be expanded to show that chain's sub-processes to help with detailed investigation and log analysis.

The fields displayed for process are as follows:

* **Process Id:** A globally unique Id for this process
* **Timeline:** A rough sketch overview of what sub-processes happened when in relation to the other visible timelines on the page.
  * Clear/empty portions represent cluster startup or other infrastructure related activity where no data processing is being performed, and no cloud service charges are accrued
  * Black/filled portions represent active processing by the system
* **Scope:** Each sub-process has a scope of operation
  * Input: This process only operated on a single input
  * Source: This process operated on all data stored in the hub table for across all inputs
  * Output Channel: This process used data from a specific Channel within a Source to Output Channel mapping&#x20;
*   **Id:** For this specific Scope, the ID of the affected configuration elements and data

    **Status:**  An icon and help-text indicating the result of the Processing Step

    ![](../../.gitbook/assets/completed.png)   No errors or warnings. Processed successfully\
    ![](../../.gitbook/assets/failed.png)   A failure has occurred in the process chain\
    ![](../../.gitbook/assets/inprogress.png)   The process is currently running\
    ![](<../../.gitbook/assets/image (291).png>)  The process is launching a new cluster\
    ![](<../../.gitbook/assets/image (351).png>) The process chain has encountered an error but recovered successfully
* **Cost:** An _estimate_ of the cost to execute a specific job, based on published costs by the cloud vendor and Databricks multiplied by runtime, in cents.
* **#:** Processed record counts popup to quickly access and display metadata about the data processed
* **Log:** Provides a popup to the indexed logging stored in Postgres for this specific process
* **Parameters:** Provides a popup with the details of the parameters passed into the DataOps Sparky executable which uses these parameters to properly execute that processing stage.
