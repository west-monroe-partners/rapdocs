---
description: >-
  The Process page provides an operational dashboard of the processes completed
  or currently active for this source
---

# Process

A process is Intellio’s atomic unit of data processing. Each process corresponds to a distinct unit of work with its own set of parameters, logs, and action steps. 

This tab provides a quick view into the [Processing ](../processing.md)dashboard, pre-filtered for this Source and/or a specific InputID to assist with debugging and monitoring

## Source Process Tab <a id="validations-screen"></a>

![](../../.gitbook/assets/image%20%28348%29.png)

The Source Process tab allows users to monitor the status of their Sources' Processes. The Process tab provides insight into the processing of all stages for a given Source. The top of the page has a variety of filters, which allow filtering based on the input id of the process, the operation type, the status, and/or the date of the process run. 

### Process Chain View

By default, the process tab is collapsed to only show "process chains". A process chain is a set of processes that have been executed as a series with one process in the chain causing the next to be triggered. The filters mentioned above will filter the process chains based on whether they contain any matching process inside of them.

The fields displayed for process chains are as follows:

* **Process ID** - This field is only used for individual processes, not chains. Clicking the arrow in this column will open the individual process view. More information about the individual process view is available below.
* **Timeline** - The total execution time of the process chain displayed in seconds. The bars displayed for each process chain timeline are relative to each other in display with overlaps indicating that the two process chains were running at the same time.  
* **Scope** - This field indicates the grain at which the process chain was started. Possible values include _Input, Source, and Output Channel_. 
* **ID -** This field is only used for individual processes, not chains.
* **Status** - Indicates the overall health of the processes that occurred as part of the process chain. 
  * ![](../../.gitbook/assets/completed.png)  Everything has processed correctly
  * ![](../../.gitbook/assets/failed.png)  A failure has occurred in the process chain
  * ![](../../.gitbook/assets/inprogress.png)  The process chain has a currently running input
  * ![](../../.gitbook/assets/image%20%28291%29.png)The process chain is launching a new cluster
  * ![](../../.gitbook/assets/image%20%28351%29.png) The process chain has encountered an error but recovered successfully
* **Cost -** An estimate of the cost in cents to run the process chain. Estimated by taking the average VM spot price _x_ number of workers _x_ run time.
* **\#** - This field is only used for individual processes, not chains.
* **Log -** This field is only used for individual processes, not chains.
* **Parameters** - This field is only used for individual processes, not chains.

### Individual Process View

Expanding a process chain will show the individual processes included within it. Each individual process represents a unit of work with parameters, logs, and execution steps. Each process is configured to automatically enqueue the next process steps upon its completion. The filters mentioned above will filter the individual processes.

The fields displayed for individual processes are as follows:

* **Process ID** - The unique identifier of the process
* **Timeline** - The execution timeline of the individual process. The white box indicates time spent in the queue while not executing. The blue box indicates time spent processing. 



