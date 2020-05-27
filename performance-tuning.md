---
description: Options available to leverage if the data loads in RAP are not meeting SLAs.
---

# Performance Tuning

In large-scale or complex implementations, performance issues can become a concern as the development phase of the implementation moves on.  This can happen whether using more traditional ETL tools with custom development approaches or when using accelerators like RAP.  This section focuses on approaches and tools that developers can use when encountering long-running processes or an overall load that is not meeting the agreed-upon SLAs for data loading.



TODO - rewrite for RAP 2.0

### Process History Tables

RAP stores the historical run times and generated queries for Validation & Enrichment \(which is normally the most expensive process for any source\) in the stage.process and stage.process\_history tables.  Analyzing the longest runtimes and associated V&E queries can lead to a path where inefficient enrichments can be tuned.

TODO - post some queries to pick out long-running processes, show how to get generated V&E and output queries, show how to get the query to compile for getting an explain plan

### Tuning Validation and Enrichment Rules

TODO - tuning methods based off the generated query, describe V&E query structure and CTEs, describe some common areas that can be tuned \(lookup overrides, duplicate lookups, eliminating lookups to lower-grain data, etc\)

### Lookup Overrides

**WARNING**:  Setting lookup overrides improperly can be detrimental to performance or lead to duplicate data being generated.  If leveraging this option, please make sure to read this section carefully and fully test your changes before rolling this out to a Production environment.

TODO - add table of types \(copy from Azure DevOps\)

