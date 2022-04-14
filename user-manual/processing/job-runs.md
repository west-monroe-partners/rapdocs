---
description: Table showing all job run details
---

# Job Runs

![Job Runs tab](../../.gitbook/assets/job\_runs\_2.5.1.png)

The Job Runs table shows detailed information on all job runs in the current environment. Both active and terminated jobs are displayed and are sorted by descending **Job Run ID** by default.

The fields displayed for job runs are as follows:

* **Job Run ID** - a global unique id for the job run
* **Databricks Run ID** - the run id assigned by Databricks
  * Clicking on the run id will open a new tab to the Databricks run table
* **Active** - a flag that shows a checkmark when job runs are currently active
* **Cluster Configuration** - the name of the cluster configuration associated with the job run
  * Clicking on the cluster configuration will open a new tab to the **Cluster Settings** page.
* **Databricks Job ID** - the job id assigned by Databricks
  * Clicking on the databricks job id will open a new tab to the Databricks job table
* **State** - the state metadata created by each job
  * Clicking the state icon will open a popup with state data
* **Launch** - the date when the job first launched
  * Shown as full local datetime
  * All other dates abbreviated to local time
* **Start** - the start date
* **Stop** - the stop date
* **Last Heartbeat** - the last heartbeat date
* **Last Process Complete** - the last process complete date
* **Last Dbr Update** - the last Databricks runtime update date
* **Current Process ID** - the current process id for active jobs
* **Current Source ID** - the current source id for active jobs
* **Stop Reason** - the reason why a job stopped
  * Hover to see full stop reason if abbreviated

{% hint style="info" %}
If a date has an asterisk \*, the date is different from the Launch date. Hover over the table cell to see full datetime value
{% endhint %}

The job runs table also includes an array of filter options for users to choose. Filters are either pre-populated dropdowns or standard inputs:

* **Active/Terminated**
  * Filter by active or terminated jobs
  * Dropdown
* **Cluster Configuration**
  * Filter by specific cluster configuration name
  * Dropdown
* **Current Source ID**
  * Integer input
* **Databricks Run ID**
  * Integer input
* **Job Run ID**
  * Integer input
* **State**
  * Filter by the key or value of the state
  * Text input
* **Stop Reason**
  * Filter by the stop reason for a job
  * Text input

{% hint style="info" %}
All integer and text input filters use partial matching
{% endhint %}

Users simply select a **Filter Type** from the leftmost dropdown, then either select or enter in the desired filter value.

Multiple filter types can be used at once. However, duplicate filter types are not available. For instance, users can only filter by one **Job Run ID**, not multiple.&#x20;

More filters, including a date picker, will be available in future releases.

