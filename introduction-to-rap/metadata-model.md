---
description: >-
  An overview of the configuration metadata tables in RAP, as well as some
  useful query patterns that can be used to quickly get configuration
  information.
---

# Metadata and Business Data Model

All data residing on the PostgreSQL database is organized into 3 main schemas.

TODO:  finish writing an intro - what broad strokes of data can you get from querying data directly, benefits of being able to understand metadata model \(SSIS / Informatica PowerCenter / other traditional ETL tools have queryable metadata layer as well\)

TODO: add useful query patterns somewhere 

### Configuration and Runtime Metadata \(stage\)

TODO: Add an intro, add an ERD, discuss concept of source\_id vs input\_id vs landing\_id \(add diagram for this as well\)

The **stage** schema consists of both configuration metadata and processing metadata.  All metadata that defines what each source is, how they get processed and the outputs they get written to are stored in this schema.  All metadata around each process and data that flows through RAP are also stored in this schema.

Tables containing source configuration metadata are the following:

* **source**:  Represents a single piece of source data and all the configuration metadata related to that source.  This is analogous to the Source page in the RAP UI.  This can be a type of flat file, database table, etc.
* **source\_dependency**:  Lists out the dependencies between sources and the lag intervals allowed for those dependencies.  This table is used to determine whether an input should wait for updated data on another source before running Validation & Enrichment processing.
* **output**:  A single output generated from RAP, whether that is a CSV file, a database table, etc.
* **output\_column**:  Lists out the fields \(and associated datatypes\) for every output.
* **output\_source**:  Lists out which outputs each source will get written to.
* **output\_source\_column**:  Contains the source to target mapping for each **output\_source**.
* **connection**:  Contains information about each connection for file and database sources / targets.  This contains database connection credentials / information for database connections and folder paths for file connections.

Tables containing runtime metadata are the following:

* **input**:  Represents a single pull of data, whether that is a single pull of a table, a CSV file, etc.
* **landing**:  Represents a "partition" of an input.  This is the smallest unit of data that is processed by RAP.  Keyed sources are one-to-one between inputs and landings, but time series sources can be split up into multiple landings to allow for higher parallelism and overall better performance processing large chunks of data.
* **output\_send**:  Represents a single instance of an output that is generated.  One output\_send record is generated each time a source dataset is written for an output.  For example, in the instance where 5 sources write to the same output in a given day, 5 output\_send records are generated on that same day for that output.
* **dependency\_queue**:  Lists out all the inputs / sources that have not had their wait conditions satisfied yet and are in a waiting status.  Metadata is also stored that shows which source is blocking the dependency from being cleared.
* **process\_batch**:  Lists out each batch of data that is either in progress or ready to run.  What each batch represents depends on the processing step.  For staging though V&E, each batch will be a single input going through one step of processing \(staging, cdc, validation\).  On the output side, each batch will be a single instance of an output being written out.  Each output batch can contain one or more output\_send records.  These records are moved over to the process\_batch\_history table after the batch completes execution.
* **process**:  Lists out each unit of work being done within batches that are in-progress or ready to run.  This generally is one-to-one with landings through the validation step.  The RAP orchestrator uses this as its processing queue table and pulls unprocessed records off this queue as processing slots open up.
* **process\_batch\_history**:  Lists out the history of batches that have already ran and completed execution.
* **process\_history**:  List out the history of processes that have already ran and completed execution.

### Log Data \(log\)

The **log** schema is used to capture messages raised by each actor withing RAP.  Most of the messages exposed through the UI are logged in this schema.  Tables stored here in combination with log files written on the servers for the RAP orchestrator and on-premise Agents can be used for troubleshooting errors.

TODO:  list out tables

### Working Data \(work\)

The **work** schema is used for intermediate processing by RAP.  All data in this schema is intended to be used internally for RAP processing only and cannot be directly exposed via an output.

The different types of tables in this schema are the following:

TODO:  list them out \(lookup and intermediate tables\), what do \#'s in table names mean

### Processed Data \(data\)

The **data** schema contains all processed data that is ready to be used for lookups or output to an external destination.  This can be thought of as the data hub layer, as data cleansing and enrichments are already complete, and the last step is to write that data out to a reporting warehouse or flat files to an external system.

The different types of tables in this schema are the following:

TODO:  list out abbreviations, what do \#'s in table names mean

