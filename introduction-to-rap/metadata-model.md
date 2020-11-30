---
description: >-
  An overview of the configuration metadata tables in RAP, as well as some
  useful query patterns that can be leveraged to quickly get configuration
  information.
---

# Metadata Model

As a metadata driven tool, Intellio has a broad interconnected set of metadata tables that store and relate the various aspects of the platform. The majority of this data is stored in the meta schema, with the exception of logs which are stored in the log schema. An understanding of the backend tables can be helpful for diagnosing issues and creating efficient platform configurations. The metadata tables can be broken into three major categories:

* Configuration Metadata
* Processing Metadata
* User Interface Metadata

## Configuration Metadata

Configuration metadata tracks and stores all of the configurations that a user performs within the user interface. This includes the creation of sources, dependnecies, agents, connection, outputs, rules, relations, and mappings. Additionally, there are a number of supporting tables that store parsed and parameterized versions of user input, as well as static information tables that are used as reference. The configuration metadata tables are all stored in the Meta schema within the Postgres database. The diagram below illustrates how each of these tables is connected. This category of tables can be further broken down into three types of tables:

### User Configured 

 These tables store the exact user configurations as specified in the user interface

* **Source**
  * This table tracks the configurations for Source objects, including all parsing, cdc, retention, schedule, and cost parameters as well as the connection and agent used.
  * Changing values in the Source Settings page will change values in this table
  * Key fields: source\_id
* **Source Relation** \(source\_relation\)
  * This table tracks the configurations of Source Relations. A single record is created for each relation, tracking its associated sources, cardinality, and expression. 
  * Changing values in the Source Relations tab will change values in this table.
  * Additional detail for this table can be found in the Source Relation Parameter table.
  * Key field: source\_relation\_id.
* **Enrichment**
  * This table tracks the configurations of Enrichments. A single record is created for each enrichment, tracking its associated source, expression, and datatype.
  * Changing values in the Enrichments tab on the Source page will change values in this table.
  * Additonal detail for this table can be found in the Enrichment Parameter table.
  * Key field: Enrichment\_id
* **Output**
  * This table tracks the configurations of Outputs. A single record is created for each output, tracking its parameters and connection.
  * Changing values in the Setting tab of the Output page will change values in this table.
  * Key field: output\_id
* **Output Source** \(output\_source\)
  * This table tracks the configurations of Source to Output Mappings. A single record is created for each Source to Output Mapping, tracking its source, output, and parameters.
  * Adding/Removing Sources in the Mappings tab or editing a Source to Output Mapping in the View/Edit Details popup will changes values in this table.
  * Key field: output\_source\_id
* **Output Column** \(output\_column\)
  * This table tracks the configurations for the individual columns that exist within an output. A single record exists for each column defined on an output, tracking its name, position, associated output, and datatype.
  * Adding/removing/editing an output column on the Mappings tab of the Output page will change values in this table.
  * Key Field: output\_column\_id
* **Output Source Column** \(output\_source\_column\)
  * This table tracks the configurations for the individual fields that are mapped into output columns. A single record exists for each Output Column to Source Field, tracking its associated output column, associated output source, and associated field.
  * Adding/removing/editing a cell in the Mapping tab of the Output page will change values in this table.
  * Key field: Output\_source\_column\_id
*  **Source Dependency** \(source\_dependency\)
  * This table tracks the configurations for Source Dependencies. A single record is created for each dependency, tracking its associated sources and and interval settings.
  * Adding or editing a dependency on the Dependencies tab of the Source page will change values in this table. 
  * Key field: source\_dependency\_id
*  **Agent**
  * This table tracks configurations for Agents. A single record is created for each agent, tracking its name, code, and parameters.
  * Adding or editing an Agent on the Agents tab will change values in this table.
  * Key field: agent\_code
*  **Connection**
  * This table tracks configuration of Connections to be used in Sources and Outputs, tracking its credentials and relevant file paths.
  * Adding or editing a Connection on the Connections page will change values in this table.
  * Key field: connection\_id
* Typically, each of these tables has Service API functions that can read and write the data to the table as users update their configurations.



### Derived 

These tables store dynamic data that is derived from user input. Editing configurations in the UI does not directly change them, but they are updated to reflect changes in their associated User Configured tables.

* **Raw Attribute** \(raw\_attribute\)
  * This table tracks the raw attributes that appear in each source. A single record is created for each attribute present in the data upon ingestion. The table tracks the original name of the attribute, the normalized name, with things like numbers, spaces etc. removed, the RAP column alias, and the most recent input that had the attribute present.
  * Raw attributes will be added and updated whenever a new input is pulled into a source.
  * Raw attributes are referenced in the creation of Rules and Relations.
  * Key fields: source\_id/raw\_attribute\_name/data\_type
* **Enrichment Parameter** \(enrichment\_parameter\)
  * This table tracks all of the attributes that are used in enrichment rules. This includes raw, system, and enriched attributes. A single record exists for each attribute that appears in an enrichment rule, tracking its associated enrichment, associated attribute, and any relations needed to get there.
  * Enrichment parameters will be added, updated, or deleted whenever enrichments are added/edited by the user.
  * Enrichment parameters are referenced in the expression\_parsed field of the enrichment table and the enrichment aggregation table.
  * Key field: enrichment\_parameter\_id
* **Enrichment Aggregation** \(enrichment\_aggregation\)
  * This table tracks all of the aggregations used in enrichment rules when utilizing x-to-many relations. A single record exists for each aggregation that appears in an enrichment rule, tracking the aggregation used and the expression that is contained within it.
  * Enrichment Aggregations contain expressions, which are further parsed down to enrichment parameters.
  * Enrichment parameters will be added, updated, or deleted whenever Rules using x-to-many relations are added/edited by the user.
  * Enrichment parameters are referenced in the expression\_parsed field of the enrichment table.
  * Key field: enrichment\_aggregation\_id
* **Source Relation Parameter** \(source\_relation\_parameter\)
  * This table tracks all of the attributes used in source relations. This includes raw, system, and enriched attributes. A single record exists for each attribute that appears in a relation, tracking the attribute and the source that it comes from.
  * Source Relation Parameters will be added, updated, or deleted whenever source relations are added/edited by the user.
  * Source Relation Parameters are referenced in the expression \_parsed field of the source relation table.
  * Key field: Source Relation ID

### System Provided

These tables store static data provided by Intellio itself. This data is used as a reference and drives aspects of the tables discussed above.

* **System Attribute** \(system\_attribute\)
  * This table has a record for each system attribute that is available for use in output column mappings, rules, and relations.
  * It tracks which source refresh types have each attribute as well as their datatypes.
  * Key field: system\_attribute\_id
* **Type Map** \(type\_map\)
  * This table has a record for each datatype that is supported for usage in output columns. Based on the data types of the fields mapped in the output mapping page, this table will determine what datatypes are valid for user to set the output column too. It also helps determine if a particular source field can be mapped to a particular output column.
  * Each record has an output type, signifying the type of output a user is trying to create, an internal type, and an external type mapping specific to the outputs type. 
  * The presence of a record in this table indicates that the type mapping is valid i.e. a number can be mapped to numeric or string so both of those records exist. A string can only be mapped to string, so no other type mappings exist. 
  * A priority is given to each type mapping. Users will be prompted with allowed datatypes in priority order when assigning datatypes to output columns.
  * Key field: external\_name/hive\_type/external\_type

![Configuration Metadata Tables](../.gitbook/assets/image%20%28281%29.png)

## Runtime Metadata

Processing metadata tracks and stores the movement and manipulation of data within Intellio. Every time that data is pulled into the system, it goes through a standard set of operations that can be found [here](https://intellio.gitbook.io/dataops/logical-architecture-overview/data-processing-engine/data-processing-1). Throughout the course of this processing, Intellio must constantly evaluate if and when a process is valid to proceed forward to its next steps. This logic called the Intellio **Workflow** takes place in a series of tables and functions. Additionally, details about each process and the Spark jobs that run it are stored in **Log** tables keep track of all logs produced by the processes.

### **Workflow**

#### **Tables**

* **User Configured**
  *  Source Dependency ****\(source\_dependency\)
    * This table tracks the configurations for Source Dependencies. A single record is created for each dependency, tracking its associated sources and and interval settings. This table will be considered when determining if an enrichment process is clear to run.
    * Adding or editing a dependency on the Dependencies tab of the Source page will change values in this table. 
    * Key field: source\_dependency\_id
* **Derived**
  * Schedule Run \(schedule\_run\)
    * This table tracks the scheduled ingestion processes within Intellio tacking the source, start time, and agent involved.
    * Every time an ingestion process is supposed to run, whether by a Pull Now, Schedule, or Watcher, a new record will be created in this table 
    * Key field: schedule\_run\_id
  * Input
    * This table tracks single pulls of data, whether that is a single pull of a table, a CSV file, etc. It tracks the time of ingest, number of records, and overall status of the input.
    * Every time an ingestion completes, a new record will be created in this table
    * Key field: input\_id
  * Workflow Queue \(workflow\_queue\)
    * This table tracks each process that is waiting to be run though Intellio including all parameters necessary for running the process. 
    * Before a process can be run, it MUST be inserted into this table. Intellio will then check the ability of that process to run. 
      * If it is unable to be run in the current state, the wait\_reasons field will be updated.
      *  If it is able to run, the record will be immediately removed from the table and passed along to the process enqueueing funcion.
    * Records in this table will be checked every time an Intellio process finishes.
    * Key field: workflow\_queue\_id
  * Process 
    * This table tracks each process that is ready to run in Intellio including all parameters necessary to run the process.
    * If a process is present in this table, it is valid to run. All of its wait conditions and dependencies have been satisfied. 
    * Spark jobs will periodically check this table for work. Any process present will be picked up and completed as resources are available.
    * Key field: process\_id
  * Spark Job
    * This table tracks each spark job launched by Intellio for processing, including its name, size, job id, heartbeat, and current process.
    * A job will remain in this table and update its heartbeat timestamp every 30 seconds for as long as it is running. 
    * Key field: Job\_id
* **System Provided**
  * Workflow
    * This table tracks the processes that should be enqueued and released following each process type
    * Each row has a completed process type, a status \(Pass or Fail\), a list of process types to enqueue after completion, and a list of process types to check after completion. Some records have additional subtype and cloud parameters can can further refine the processing done. 
    * Key Field: completed\_process/completed\_process\_subtype/status\_code/refresh\_type/ingestion\_type/cloud
  * Wait Type \(wait\_type\)
    * This table tracks all of the various reasons a process may need to wait before it can be run
    * Each row contains a process type, a refresh type, and a query that will be run to determine if a wait is needed.
    * Each query is tokenized with &lt;source\_id&gt;, &lt;input\_id&gt;, or &lt;process\_id&gt; which will be replaced with the value of the process in question when being executed.
    * For each query, a null result means that the process does not need to wait for that specific reason, however a process may have multiple wait conditions. It must pass them all in order to be enqueued.

#### Functions

* Workflow Enqueue \(prc\_w\_trigger\_enqueue\)
  * This function takes a process ID and a status code as arguments. It is called from the process end function.
  * Based on the supplied process id and status code, the function will check the workflow table to determine which processes should be inserted into the workflow queue
  * Logic in the function will dictate which parameters are stored on each workflow queue record. This varies by process type.
  * This function ends after writing records to the workflow queue table.
* Workflow Release \(prc\_w\_trigger\_release\)
  * This function takes a process ID and a status code as arguments. It is called from the Core after running the workflow enqueue function.
  * Based on the supplied process id and status code, the function will check the workflow table to determine which processes should be checked for release from the workflow queue.
  * For each process that needs to be checked, the function will retrieve the applicable queries from the wait type table based on process type and refresh type. 
  * If the function does not find any wait reasons, it will remove the record from the workflow queue table and pass the process on to the process enqueueing function.
  * If the function does find applicable wait reasons, it will update the record in the workflow queue table to reflect these reasons.
  * This function ends after running all applicable wait type queries against each applicable process.
* Process Enqueue \(prc\_process\_enqueue\)
  * This function takes a number of arguments that vary from process to process, it only operates on a single process per function run. It is called from the workflow release function.
  * The process will update input statuses and generate queries as needed. It will then insert a record to the process table for a spark cluster to pick up and run.
  * The process is finished after inserting a record to the process queue.
* Get Next \(prc\_process\_get\_next\)
  * Spark jobs will call this function when they are ready for work.
  * The function takes a spark job id as an argument
  * Based on the spark job size, it will be matched with a process from the process table. The job will then run that process.
  * If there is no work to be done, the spark job will be told to wait and try again.
  * The function ends when it returns a new process id to the spark job, or when it tells the spark job to wait for more work.
* Process End \(prc\_process\_end\)
  * This function takes a process id and a status code as arguments. Is it called by the spark job when it has finished the process
  * It will move the process from the process table to process history. It then calls the Workflow Enqueue function. Bringing us full circle.

### Logging

* Agent Log \(log.agent\)
  * This table tracks all logs from the agent, including the agent code, timestamp, and message.
  * These logs can be seen in the agent tab of the UI.
* Actor Log \(log.actor\_log\)
  * This table tracks all logs from the core, sparky and some database functions. They include the log id, a timestamp, a message, and a severity.
  * These logs can be seen in the process screen of the UI.
* API Log \(log.api\_log\)
  * This table tracks all errors from the API. They include the timestamp, submitted userid, request body and headers, and the server error message.
  * Only errors are tracked in this table
  * These logs cannot currently be seen in the UI.

Tables containing runtime metadata are the following:

![Process Flow Diagram](../.gitbook/assets/image%20%28280%29.png)

## Useful Queries

This section lists out some metadata queries that have been useful on prior RAP implementations.

* To see process logs
  * SELECT \* FROM log.actor\_log a JOIN meta.process\_history p ON a.log\_id = p.log\_id WHERE process\_id = &lt;process\_id&gt;
* To see the reason for your api error
  * SELECT \* FROM log.api\_log WHERE user\_id = &lt;user\_id@domain.com&gt;
* To check if your source is scheduled to run
  * SELECT \* FROM meta.schedule\_run WHERE source\_id = &lt;source\_id&gt;

### 

