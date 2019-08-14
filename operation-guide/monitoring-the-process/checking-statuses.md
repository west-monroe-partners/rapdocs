# Checking Statuses

In RAP, every input marks its progress with a status code for each phase. The symbol and meaning of each status code are below:

 Passed \(P\) – All processes in the phase have succeeded

![](../../.gitbook/assets/4.png) Failed \(F\) – A process in the phase has failed

* ![](../../.gitbook/assets/4.png) Failed \(F\) – A process in the phase has failed 
* In Progress \(I\) – A process in the phase is in currently executing 
* CDC \(C\) – Input is waiting on a previous input to pass the Validation and Enrichment phase 
* Queued \(Q\) – Input is queued in the process table and receives a connection when one becomes available 
* Ready \(null\) – Input is ready to be queued for processing
* \

