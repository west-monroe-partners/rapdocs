---
description: Options available to leverage if the data loads in RAP are not meeting SLAs.
---

# Performance Tuning

In large-scale or complex reporting implementations, performance issues can become a concern as the development phase of the implementation moves on.  This can happen whether using more traditional ETL tools and custom development approaches or using RAP.  This section focuses on approaches and tools that developers can use when encountering long-running processes or an overall load that is not meeting the agreed-upon SLAs for data loading.

### Process History Tables

TODO - how to pick out long-running processes, get V&E query and tune

### Lookup Overrides

**WARNING**:  Setting lookup overrides improperly can be detrimental to performance or lead to duplicate data being generated.  If leveraging this option, please make sure to read this section carefully and fully test your changes before rolling this out to a Production environment.

TODO - add table of types \(copy from Azure DevOps\)

