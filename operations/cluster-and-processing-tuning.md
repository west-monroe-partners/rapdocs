# Cluster and Processing Tuning

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

The Spark Driver is responsible for translating your code into [Spark Jobs](cluster-and-processing-tuning.md#spark-job), [Spark Stages](cluster-and-processing-tuning.md#spark-stage), [Spark Tasks](cluster-and-processing-tuning.md#spark-task), and Directed Analytic Graphs (DAGs).

You can think of the Spark Driver as the planner of operations for the Spark ecosystem. It generates Spark sub-structures to achieve the result requested by all code submitted.

Once it has generated these structures, the Driver then sends a request to the [Cluster Manager](cluster-and-processing-tuning.md#cluster-manager) to be allocated [Spark Executors](cluster-and-processing-tuning.md#spark-executor) to begin processing the data.

### Spark Job

A Spark Job is a stand-alone execution of parallelizable spark code, bounded by a [Spark Action](https://spark.apache.org/docs/latest/rdd-programming-guide.html#actions). Each Spark Action generates a separate Spark Job, which will be executed lazily and in FIFO order by default. This [PDF ](https://training.databricks.com/visualapi.pdf)presentation from Databricks has a good overview and list of Spark Actions.

By default, no data is shared between jobs, as they are by definition assumed to be fully parallelizable - even though they are executed FIFO by default.

In order to ensure operations across jobs are not duplicated, developers must manually code persistence hand-offs between Spark Action sub-segments. See the spark [documentation ](https://spark.apache.org/docs/latest/rdd-programming-guide.html#rdd-persistence)for details.

#### Spark Job Optimization Example:

In this example, we will show the importance of spark coding best practices to avoid duplicate processing and improve performance by designing Spark Actions and using persistence/caching between them.

In this example, we will create a sample Dataframe, create a new calculated column summarizing two columns for each row, rollup the calculation by department, count the records before and after the rollup, print out the difference between counts, and then save the summarized Dataframe to a file.

#### Example 1: Naive / Bad Practice Design

```scala
import spark.implicits._

val sourceData: DataFrame = Seq(("James","Sales","NY",90000,34,10000),
  ("Michael","Sales","NY",86000,56,20000),
  ("Robert","Sales","CA",81000,30,23000),
  ("Maria","Finance","CA",90000,24,23000),
  ("Raman","Finance","CA",99000,40,24000),
  ("Scott","Finance","NY",83000,36,19000),
  ("Jen","Finance","NY",79000,53,15000),
  ("Jeff","Marketing","CA",80000,25,18000),
  ("Kumar","Marketing","NY",91000,50,21000)
).toDF("employee_name","department","state","salary","age","bonus")

val totalCompAdded: DataFrame = sourceData
  .withColumn("total_comp", col("salary") + col("bonus") )

val groupedByDepartment: RelationalGroupedDataset = 
  totalCompAdded.groupBy("department")

val salarySummarized: DataFrame = 
  groupedByDepartment.sum("salary")

val originalCount: Long = totalCompAdded.count()
val newCount: Long = salarySummarized.count()

val difference: Long = originalCount - newCount
println(difference)
println(sum)

salarySummarized.write.text("examples/src/main/resources/output.txt")

```

This example shows how a developer may intuitively write the code to build this data pipeline. While relatively easy to follow and read, this simple code block will actually perform a number of calculations multiple times across multiple jobs.

Spark will generate one Spark Job per Spark Action written in the code. In this case, there are four Spark Actions:

```scala
val df4: DataFrame = df3.sum("salary")
```

```scala
val originalCount: Long = df2.count()
```

```scala
val newCount: Long = df4.count()
```

```scala
df2.write.text("examples/src/main/resources/output.txt")
```

As a result, Spark will translate this code into four separate jobs that will execute independently from each other. Here is the code equivalent for the four jobs that the Spark Driver will generate:

#### Job 1: Summarize by Salary

```scala
import spark.implicits._

val sourceData: DataFrame = Seq(("James","Sales","NY",90000,34,10000),
  ("Michael","Sales","NY",86000,56,20000),
  ("Robert","Sales","CA",81000,30,23000),
  ("Maria","Finance","CA",90000,24,23000),
  ("Raman","Finance","CA",99000,40,24000),
  ("Scott","Finance","NY",83000,36,19000),
  ("Jen","Finance","NY",79000,53,15000),
  ("Jeff","Marketing","CA",80000,25,18000),
  ("Kumar","Marketing","NY",91000,50,21000)
).toDF("employee_name","department","state","salary","age","bonus")

val totalCompAdded: DataFrame = sourceData
  .withColumn("total_comp", col("salary") + col("bonus") )

val groupedByDepartment: RelationalGroupedDataset = 
  totalCompAdded.groupBy("department")

val salarySummarized: DataFrame = 
  groupedByDepartment.sum("salary")
```

Because we referenced totalCompAdded as the starting dataframe for the groupBy and subsequent sum Action, Job 1 will perform both the new column calculation as well as the sum operation.

This job highlights the first optimization: perform actions as early in the transformation pipeline as possible.

With a simple tweak, we can cut out the column calculation from this job:

```scala
//old code referencing totalCompAdded
val groupedByDepartment: RelationalGroupedDataset = 
  totalCompAdded.groupBy("department")

val salarySummarized: DataFrame = 
  groupedByDepartment.sum("salary")
  
//new code referencing the SourceData dataframe, as it will logically result
//in the same count (we do not need the total_comp column to calculate the count)
val groupedByDepartment: RelationalGroupedDataset = 
  sourceData.groupBy("department")

val salarySummarized: DataFrame = 
  groupedByDepartment.sum("salary")
```

As a result, the column calculation is eliminated from Job 1:

```scala
import spark.implicits._

val sourceData: DataFrame = Seq(("James","Sales","NY",90000,34,10000),
  ("Michael","Sales","NY",86000,56,20000),
  ("Robert","Sales","CA",81000,30,23000),
  ("Maria","Finance","CA",90000,24,23000),
  ("Raman","Finance","CA",99000,40,24000),
  ("Scott","Finance","NY",83000,36,19000),
  ("Jen","Finance","NY",79000,53,15000),
  ("Jeff","Marketing","CA",80000,25,18000),
  ("Kumar","Marketing","NY",91000,50,21000)
).toDF("employee_name","department","state","salary","age","bonus")

val groupedByDepartment: RelationalGroupedDataset = 
  sourceData.groupBy("department")

val salarySummarized: DataFrame = 
  groupedByDepartment.sum("salary")
```

Careful elimination of upstream dependencies in your logical pipeline is the most clean and effective way to reduce processing and improve performance.

#### Job 2: Count rows before summarization

```scala
import spark.implicits._

val sourceData: DataFrame = Seq(("James","Sales","NY",90000,34,10000),
  ("Michael","Sales","NY",86000,56,20000),
  ("Robert","Sales","CA",81000,30,23000),
  ("Maria","Finance","CA",90000,24,23000),
  ("Raman","Finance","CA",99000,40,24000),
  ("Scott","Finance","NY",83000,36,19000),
  ("Jen","Finance","NY",79000,53,15000),
  ("Jeff","Marketing","CA",80000,25,18000),
  ("Kumar","Marketing","NY",91000,50,21000)
).toDF("employee_name","department","state","salary","age","bonus")

//Similar to Job 1, this code block can be eliminated
val totalCompAdded: DataFrame = sourceData
  .withColumn("total_comp", col("salary") + col("bonus") )

//totalCompAdded should be changed to sourceData to avoid unnecessary processing
val originalCount: Long = totalCompAdded.count()
```

Job 2 does not execute the groupBy or sum steps, even though in the original code, they are written before the count Action.

Similar to Job 1, this job can be optimized by refencing the sourceData Dataframe rather than the totalCompAdded Dataframe.

The resulting Job looks like:

```scala
import spark.implicits._

val sourceData: DataFrame = Seq(("James","Sales","NY",90000,34,10000),
  ("Michael","Sales","NY",86000,56,20000),
  ("Robert","Sales","CA",81000,30,23000),
  ("Maria","Finance","CA",90000,24,23000),
  ("Raman","Finance","CA",99000,40,24000),
  ("Scott","Finance","NY",83000,36,19000),
  ("Jen","Finance","NY",79000,53,15000),
  ("Jeff","Marketing","CA",80000,25,18000),
  ("Kumar","Marketing","NY",91000,50,21000)
).toDF("employee_name","department","state","salary","age","bonus")

val originalCount: Long = sourceData.count()
```



###

### Spark Stage

A Spark Stage is an execution plan for a portion of a Spark Job, delineated by computational boundaries such as&#x20;

### Cluster Manager

The Cluster Manager acts as the interface between the Driver and Executors. It allocates resources  to Driver(s) and distributes Tasks to workers. It also is responsible for tracking the success, failure, and retries of individual tasks, as well as the overall Spark Job status.

### Spark Executor

A Spark Executor is an abstraction of a combination of compute and memory resources&#x20;

### Spark Task

A Spark Task represents an atomic unit of work to be acted upon
