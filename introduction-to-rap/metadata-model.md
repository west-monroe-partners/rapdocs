---
description: >-
  An overview of the configuration metadata tables in RAP, as well as some
  useful query patterns that can be leveraged to quickly get configuration
  information.
---

# !! Metadata Model

As a metadata driven tool, Intellio has a broad interconnected set of metadata tables that store and relate the various aspects of the platform. An understanding of the backend tables can be helpful for diagnosing issues and creating efficient platform configurations. The metadata tables can be broken into three major categories:

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

## Processing Metadata

The **stage** schema consists of both configuration metadata and processing metadata.  All metadata that defines what each source is, how they get processed and the outputs they get written to are stored in this schema.  All metadata around each process and data that flows through RAP are also stored in this schema.

Tables containing source configuration metadata are the following:

* **source**:  Represents a single piece of source data and all the configuration metadata related to that source.  This is analogous to the Source page in the RAP UI.  This can be a type of flat file, database table, etc.
* **source\_dependency**:  Lists out the dependencies between sources and the lag intervals allowed for those dependencies.  This table is used to determine whether an input should wait for updated data on another source before running Validation & Enrichment processing.
* **output**:  A single output generated from RAP, whether that is a CSV file, a database table, etc.
* **output\_column**:  Lists out the fields \(and associated datatypes\) for every output.
* **output\_source**:  Lists out which outputs each source will get written to.
* **output\_source\_column**:  Contains the source to target mapping for each **output\_source**.
* **connection**:  Contains information about each connection for file and database sources / targets.  This contains database connection credentials / information for database connections and folder paths for file connections.
* ~~**lookup**:  Contains the list of sources that need lookup tables built.  This is required for support of lookups not on the primary key \(i.e., not using s\_key\).~~

Tables containing runtime metadata are the following:

![Process Flow Diagram](../.gitbook/assets/image%20%28280%29.png)

* **input**:  Represents a single pull of data, whether that is a single pull of a table, a CSV file, etc.
* **landing**:  Represents a "partition" of an input.  This is the smallest unit of data that is processed by RAP.  Keyed sources are one-to-one between inputs and landings, but time series sources can be split up into multiple landings to allow for higher parallelism and overall better performance processing large chunks of data.
* **output\_send**:  Represents a single instance of an output that is generated.  One output\_send record is generated each time a source dataset is written for an output.  For example, in the instance where 5 sources write to the same output in a given day, 5 output\_send records are generated on that same day for that output.
* **dependency\_queue**:  Lists out all the inputs / sources that have not had their wait conditions satisfied yet and are in a waiting status.  Metadata is also stored that shows which source is blocking the dependency from being cleared.
* **process\_batch**:  Lists out each batch of data that is either in progress or ready to run.  What each batch represents depends on the processing step.  For staging though V&E, each batch will be a single input going through one step of processing \(staging, cdc, validation\).  On the output side, each batch will be a single instance of an output being written out.  Each output batch can contain one or more output\_send records.  These records are moved over to the process\_batch\_history table after the batch completes execution.
* **process**:  Lists out each unit of work being done within batches that are in-progress or ready to run.  This generally is one-to-one with landings through the validation step.  The RAP orchestrator uses this as its processing queue table and pulls unprocessed records off this queue as processing slots open up.
* **process\_batch\_history**:  Lists out the history of batches that have already ran and completed execution.
* **process\_history**:  List out the history of processes that have already ran and completed execution.
* ~~**lookup\_table**:  List of all the active lookup tables in the work schema.~~

### Log Data \(log\)

The **log** schema is used to capture messages raised by each actor withing RAP.  Most of the messages exposed through the UI are logged in this schema.  Tables stored here in combination with log files written on the servers for the RAP orchestrator and on-premise Agents can be used for troubleshooting errors.

* **actor\_log**:  Contains log messages generated from the various actors maintained by the Orchestrators.  These correspond to messages viewable on the Inputs and Process pages.
* **agent**: Contains log messages generated by the various on-premise RAP Agents.  These are the same log messages visible on the Agent tab on the RAP UI.
* **staging\_error**:  Contains the error messages and associated raw record that cause the Staging step to fail.

### Useful Queries

This section lists out some metadata queries that have been useful on prior RAP implementations.

TODO - put random helpful queries here

TODO: add useful query patterns somewhere 

TODO: refer to pyramid slide

### 

