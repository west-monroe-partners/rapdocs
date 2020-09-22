# !! Checking Logs

If clicking the status icon and reading the logs does not help diagnose the failure, there are two main logs to check: The **Orchestrator Log** and the **Actor Log**.

The **Orchestrator Log** is stored in a file on the ETL box.

The **Actor Log** is stored in a [Postgres ](checking-logs.md)table but can be accessed to an extent through the RAP UI.

## Actor Log

The Actor Log holds an account of all activity that takes place with any of data processing activities, including source scheduling, input, staging, validation & enrichment, and output. The UI exposes most of the information in this log to the user via the “Processing Log” seen when clicking on the status icons.

![Validation Process Log](../../.gitbook/assets/image%20%28201%29.png)

If the desired information is not available, the full log is accessible through a log.actor\_log table query, e.g.

```sql
SELECT * FROM log.actor_log ORDER BY insert_datetime DESC LIMIT 1000
```

Common use cases for using the Actor Log include:

* Checking Agent connectivity
* Cannot access desired log message from UI Processing Log

## Orchestrator Log

The Orchestrator Log provides higher level of details, including information related to system initialization, queueing/wait logic, clean up, connection management. The Orchestrator log is available in file format directly on the ETL server. In the instance that the database is unavailable, the Orchestrator log can still be accessed and used in diagnosing issues with the application.

To access the Orchestrator log, follow instructions in the [EC2 section](checking-logs.md).

Common use cases for using the Orchestrator log include:

* Ensuring system initialization works as intended
* Seeing details behind the background file cleanup processes
* Finding the statuses of the connections available in the application
* Viewing an account of activities that occurred prior to a database crash

