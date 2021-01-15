---
description: How to handle potential data errors in DataOps.
---

# !! Handling Data Validation Issues

In any data integration project, bad or non-conforming data is something that the development team will most likely encounter.  Data architects need to ensure an approach to handle those errors is part of the design to ensure that errors are caught and rerouted to the appropriate place.  This section suggests an approach that could be leveraged to capture and route off errors and warnings to a separate location for further analysis.

### Creating Validation Rules

The way that records are marked with Warn or Fail statuses are through the configuration of Validation Rules.  Refer to the [Rules section](../configuring-the-data-integration-process/source-configuration/enrichment-rule-configuration.md) in the documentation for detailed information about how to configure Validation rules.

The guideline for defining Validation rules are to mark rules related to critical fields / checks as Fail and all other rule checks as Warn.

### Routing Output Data

Once data is flagged with an Error or a Warning status, those records should be routed off to a common error table for analysis.

![Logical flow](../.gitbook/assets/image%20%28332%29.png)

![Data flow](../.gitbook/assets/image%20%28331%29.png)

The common error table should contain the following fields:

TODO - list fields

