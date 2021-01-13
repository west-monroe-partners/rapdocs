---
description: How to handle potential data errors in DataOps.
---

# !! Handling Data Validation Issues

In any data integration project, bad or non-conforming data is something that the development team will most likely encounter.  Data architects need to ensure an approach to handle those errors is part of the design to ensure that errors are caught and rerouted to the appropriate place.  This section suggests an approach that could be leveraged to capture and route off errors and warnings to a separate location for further analysis.

### Setting the Error Level on Rules

TODO - discuss how to decide what to set, link to section in Configuration Guide, add screenshot

### Routing Output Data

Once data is flagged with an Error or a Warning status, those records should be routed off to a common error table for analysis.

![Logical flow](../.gitbook/assets/image%20%28332%29.png)

![Data flow](../.gitbook/assets/image%20%28331%29.png)

The common error table should contain the following fields:

TODO - list fields

