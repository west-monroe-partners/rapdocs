# Cluster and Processing Tuning

## Introduction

In order to minimize costs and processing times within IDO, it is often valuable to override the defaults for cluster sizing and storage partitioning to manually tune the most computationally expensive processes within the system.

This guide will provide a general overview of the different concepts and structures required to adjust your Sources to find the correct balance between cost and performance for your organization.

It is focused on only the details needed for performance tuning jobs within the context of Databricks and Intellio DataOps, but can be applied to many custom deployments as well.

## Spark Structures/Services Overview

In order to know how to tune your specific process, it's important to first understand the fundamentals of how Spark takes complex processes and breaks them down into parallelizable chunks of work and distribute this work among all the compute resources available to the cluster.

![Logical Model between IDO, Databricks, and Spark Structures](<../.gitbook/assets/image (383).png>)

For additional details on how IDO Cluster Configs and IDO Processes connect to the rest of the Intellio DataOps metadata structures, please refer to the [Cluster and Process Configuration Overview](../user-manual/system-configuration/cluster-and-process-configuration-overview/) section.

This guide will primarily focus on the detailed Spark Structures in the diagram above, but will also cover how to adjust the infrastructure these structures operate on top of to improve performance and cost efficiency.

### Spark Driver

The Spark Driver is responsible for translating your code into a Spark Job, Spark Stages, [Spark Tasks](cluster-and-processing-tuning.md#spark-task), and scheduling the order of operations and dependencies between tasks within a job.

Once it has generated these tasks and their associated schedule (also referred to as a Directed Analytic Graph or DAG) the Driver then sends a request to the [Cluster Manager](cluster-and-processing-tuning.md#cluster-manager) to be allocated [Spark Executors](cluster-and-processing-tuning.md#spark-executor) to begin processing the data.

### Spark Job

A Spark Job is a user-defined logical grouping of one or more Spark Stage(s), Tasks, and their associated schedules to complete a data processing pipeline. Often times, a Job is a single code block or group of code that is individually submitted to the Spark environment for processing.

{% hint style="info" %}
A Databricks Job, Databricks Job Run, IDO Run, and IDO Process are all different concepts and structures than a Spark Job.

While helpful in debugging, the nuanced differences between these concepts and how they interact is largely moot for the purposes of performance tuning.
{% endhint %}

### Spark Stage



### Cluster Manager

The Cluster Manager acts as the interface between the Driver and Executors. It allocates resources  to Driver(s) and distributes Tasks to workers. It also is responsible for tracking the success, failure, and retries of individual tasks, as well as the overall Spark Job status.

### Spark Executor

A Spark Executor is an abstraction of a combination of compute and memory resources&#x20;

### Spark Task

A Spark Task represents an atomic unit of work to be acted upon
