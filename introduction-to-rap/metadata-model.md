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

* source
* source\_dependency
* connection

Runtime Metadata tables:

* input
* landing
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

