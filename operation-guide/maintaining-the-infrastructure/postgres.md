# Postgres

## Introduction

PgAdmin is free, open source tool for connecting to Postgres downloadable on the Postgres website. Datagrip is a commercial product that also works very well for querying Postgres. Once the tool of choice downloads, acquire a VPN connection and then enter database server credentials acquired from a platform administrator into the tool. 

Access Postgres to address some of the following issues:

* Find data row that failed staging, investigate raw data \(`log.staging_error`\)
* Review orchestration and agent logs \(`log.actor_log`\)
* Check queued/in progress processes, and if any are waiting in dependency queue \(`stage.process`, `stage.process_batch`, `stage.dependency_queue`\)
* Get file names that have been output in the past reload \(`stage.output_send`, `stage.output_send_history`\)
* Get source file information, and find the location of the file in the staging folder or archive folder \(`stage.input`\)
* Check if Agent is communicating \(`stage.agent`\)

## Tables

This section covers the important Postgres tables. Knowing the backend tables will allow the user to dive deeper into the RAP processing steps and support the platform more effectively.

| **Table** | **Description** |
| :--- | :--- |
| `Stage.agent` | Check health of the configured Agents, validate Agent parameters |
| `Stage.input` | Input metadata, transaction start and end times, source file name, get file location |
| `Stage.input_history` | Input record is moved to this table upon deletion, snapshot of the input record is put in this table when a reset is performed |
| `Stage.landing` | Used to link input record to work and data tables |
| `Stage.landing_history` | Landing records are moved to this table upon input deletion, and input reset |
| `Stage.output_send` | Record of Output runs, 1 Output Send record per Output Source, tracks most recent run of each Output |
| `Stage.output_send_history` | When Output is reset, old Output Send records from the Output are moved to this table |
| `Stage.process` | Tracks queued and running processes in RAP |
| `Stage.process_history` | History of processes in RAP |
| `Stage.process_batch` | Tracks queued and running processes in RAP at a lower grain than stage.process |
| `Stage.process_batch_history` | History of batch processes in RAP |
| `Stage.dependency_queue` | Tracks inputs that are waiting on a dependent source to finish processing |
| `Log.actor_log` | Ledger of log messages from the Agent and Orchestrator |
| `Log.staging_error` | Contains failed records from staging phase |

{% hint style="info" %}
The [Example Daily Routine](../monitoring-the-process/example-daily-routine.md) contains support queries to run against these tables.
{% endhint %}

