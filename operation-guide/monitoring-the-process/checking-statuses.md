# Checking Statuses

## Understanding Status Codes

RAP utilizes the following status codes to report on for each processing phase including input, staging, validation and enrichment, and output.  The symbol and meaning of each status code are below:

\*\*\*\*![](../../.gitbook/assets/ready%20%281%29.png) **Ready \(null\)**: Landing is prepared for processing phase or waiting for dependent processing phase\(s\) to complete.

![](../../.gitbook/assets/queued.png) **Queued \(Q\)**: Process queued, waiting for free connections to execute.

![](../../.gitbook/assets/pending%20%281%29.png) **Pending \(CDC\)** : Phase is waiting on a dependent source to complete processing.

\*\*\*\*![](../../.gitbook/assets/inprogress.png) **In-Progress \(I\)**: Processing phase currently executing.

![](../../.gitbook/assets/warning.png) **Warning \(W\)**: State of Source out of sync with destination table.

\*\*\*\*![](../../.gitbook/assets/failed.png) **Failed \(F\)**: Processing phase failed. Check detail modal to discover reason of failure.

\*\*\*\*![](../../.gitbook/assets/completed.png) **Passed \(P\)**: Processing phase successfully completed. Check detail modal to discover phase metadata.

{% hint style="info" %}
Clicking any of these status icons will display the processing logs for that input/phase. This is typically the most useful and accessible way to troubleshoot the platform.
{% endhint %}

## Investigating Status Codes

If the status code is `F`, or `Failed`, then the processing will not continue on to the next process in the chain. However, the platform contains reset functionality to provide the user with the ability to make changes and reset the failed process. For more information on using the reset buttons and the various options available to users, refer to the [Inputs](../../configuring-the-data-integration-process/source-configuration/source-inputs.md#controlling-all-inputs) section of the Configuration guide.

In a general troubleshooting scenario, it is best to make changes to a source that would fix the error that caused failure before blindly running a reset. 

{% hint style="danger" %}
Note: Delete will permanently delete the input. Do not click delete unless you are certain that the input needs removing.
{% endhint %}

A common scenario, especially when doing large reloads or running many processes concurrently, is longer `In-Progress`phases for processes to proceed to `Passed` or `Failed`. In most situations, the process is waiting for connections to allocate internally.

![A Staging process waiting for connections to allocate](../../.gitbook/assets/10%20%281%29%20%281%29.png)

If any processes timeout, then they will fail and update their status to fail accordingly. If a process is still In Progress for an abnormally long time, a deeper investigation is necessary.

