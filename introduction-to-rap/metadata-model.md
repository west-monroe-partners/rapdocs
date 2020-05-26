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
* **source\_dependency**:  Lists out the dependencies between sources and the lag intervals allowed for those dependencies.  This table is used to determine whether an input should wait for updated data on another source before running Validation & Enrichment processing.
* **output**:  A single output generated from RAP, whether that is a CSV file, a database table, etc.
* **connection**:  Contains information about each connection for file and database sources / targets.  This contains database connection credentials / information for database connections and folder paths for file connections.

Runtime Metadata tables:

* **input**:  Represents a single pull of data, whether that is a single pull of a table, a CSV file, etc.
* **landing**:  Represents a "partition" of an input.  Keyed sources are one-to-one between inputs and landings, but time series sources can be split up into multiple landings to allow for parallelism and better performance processing large chunks of data.
* **output\_send**:  Represents a single instance of an output that is generated.  One output\_send record is generated each time a source dataset is written for an output.  In the instance where 5 sources write to the same output in a given day, 5 output\_send records are generated on that same day for that output.
* **dependency\_queue**:  Lists out all the dependencies that have not had their wait conditions satisfied yet.  Metadata is also stored that shows which source is blocking the dependency from being cleared.
* process\_batch
* process
* process\_batch\_history
* process\_history

### Log Data \(log\)

TODO: Document orchestrator log messages

### Working Data \(work\)

The **work** schema is used for intermediate processing by RAP.

### Processed Data \(data\)

The **data** schema contains all processed data that is ready to be used for lookups or output to an external destination.

