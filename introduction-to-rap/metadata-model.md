---
description: >-
  An overview of the configuration metadata tables in RAP, as well as some
  useful query patterns that can be used to quickly get configuration
  information.
---

# Configuration Metadata Model

All data residing on the PostgreSQL database is organized into 3 main schemas.

### Configuration and Runtime Metadata \(stage\)

TODO: Document main tables, add an ERD, discuss concept of source\_id vs input\_id vs landing\_id \(add diagram for this as well\)

Source Configuration tables:

* **source**:  Represents a single piece of source data and all the configuration metadata related to that source.  This is analogous to the Source page in the RAP UI.  This can be a type of flat file, database table, etc.
* **source\_dependency**:  Lists out the dependencies between sources and the lag intervals allowed for those dependencies.  This table determines whether an input should wait for updated data on anther source before running Validation & Enrichment processing.
* **output**:  A single output generated from RAP, whether that is a CSV file, a database table, etc.
* connection

Runtime Metadata tables:

* **input**:  Represents a single pull of data, whether that is a single pull of a table, a CSV file, etc.
* **landing**:  Represents a "partition" of an input.  Keyed sources are one-to-one between inputs and landings, but time series sources can be split up into multiple landings to allow for parallelism and better performance processing large chunks of data.
* output\_send
* dependency\_queue
* process\_batch
* process
* process\_batch\_history
* process\_history

### Log Data \(log\)

TODO: Document orchestrator log messages

### Working Data \(work\)

TODO: Document main types of tables \(data work tables, lookup tables\) and naming conventions

### Processed Data \(data\)

TODO: Document ts\_ vs. key\_ tables and naming conventions

