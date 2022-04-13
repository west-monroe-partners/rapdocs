# Spark and Databricks Overview

## Introduction

In order to minimize costs and processing times within IDO, it is often valuable to override the defaults for cluster sizing and storage partitioning to manually tune the most computationally expensive processes within the system.

This guide will provide a general overview of the different concepts and structures required to adjust your Sources to find the correct balance between cost and performance for your organization.

It is focused on only the details needed for performance tuning jobs within the context of Databricks and Intellio DataOps, but can be applied to many custom deployments as well.

## Spark Structures/Services Overview

In order to know how to tune your specific process, it's important to first understand the fundamentals of how Spark takes complex processes and breaks them down into parallelizable chunks of work and distribute this work among all the compute resources available to the cluster.

This guide will primarily focus on the Databricks and Spark Structures in the diagram above.

For details on IDO Cluster Configs and IDO Processes, as well as how they connect to the rest of the Intellio DataOps metadata structures, please refer to the [Cluster and Process Configuration Overview](../user-manual/system-configuration/cluster-and-process-configuration-overview/) section.

![](<../.gitbook/assets/image (388).png>)

### Databricks Job

A Databricks Job is a set of configurations to run non-interactive code within Databricks. It includes what code to run (notebook or Jar executable), what type of cluster to run the code on, and optionally, what order to run Databricks Tasks in.

A IDO Cluster Config is a wrapper around a Databricks Job that allows users to integrate Databricks Job configurations with the rest of the IDO Meta structures, as well as hold some additional IDO specific parameters.

### Databricks Task

A Databricks Task is a logical grouping of non-interactive code.

All Jabs managed by IDO, including Custom Notebooks integrated via SDK as assumed to only have one task per job. IDO does not currently support integration with Databricks orchestration of multiple tasks feature.

Databricks Tasks are roughly analogous to IDO Processes and/or Process Types, but lack a large number of features required to support the IDO meta framework. Because of these lack of features, IDO Processes are not a wrapper around, but a completely separate concept from Databricks Tasks at this time.

### Databricks Job Run

A Databricks Job Run is a specific execution (not to be confused with a Spark Executor) of a defined Databricks Task.

A IDO Job Run is a wrapper around a Databricks Job Run for purposes of integration with the IDO meta structures via the IDO Process structure.

### Spark Driver

The Spark Driver is responsible for translating your code into [Spark Jobs](spark-and-databricks-overview.md#spark-job), [Spark Stages](spark-and-databricks-overview.md#spark-stage), [Spark Tasks](spark-and-databricks-overview.md#spark-task), and Directed Analytic Graphs (DAGs).

You can think of the Spark Driver as the planner of operations for the Spark ecosystem. It generates Spark sub-structures to achieve the result requested by all code submitted.

Once it has generated these structures, the Driver then sends a request to the [Cluster Manager](spark-and-databricks-overview.md#cluster-manager) to be allocated [Spark Executors](spark-and-databricks-overview.md#spark-executor) to begin processing the data.

### Spark Job

A Spark Job is a stand-alone execution of parallelizable spark code, bounded by a [Spark Action](https://spark.apache.org/docs/latest/rdd-programming-guide.html#actions). Each Spark Action generates a separate Spark Job, which will be executed lazily and in FIFO order by default. This [PDF ](https://training.databricks.com/visualapi.pdf)presentation from Databricks has a good overview and list of Spark Actions.

By default, no data is shared between jobs, as they are by definition assumed to be fully parallelizable - even though they are executed FIFO by default.

In order to ensure operations across jobs are not duplicated, developers must manually code persistence hand-offs between Spark Action sub-segments. See the spark [documentation ](https://spark.apache.org/docs/latest/rdd-programming-guide.html#rdd-persistence)for details.

### Spark Stage

A Spark Stage is an execution plan for a portion of a Spark Job, delineated by computational boundaries such as&#x20;

### Cluster Manager

The Cluster Manager acts as the interface between the Driver and Executors. It allocates resources  to Driver(s) and distributes Tasks to workers. It also is responsible for tracking the success, failure, and retries of individual tasks, as well as the overall Spark Job status.

### Spark Executor

A Spark Executor is an abstraction of a combination of compute and memory resources&#x20;

### Spark Task

A Spark Task represents an atomic unit of work to be acted upon
