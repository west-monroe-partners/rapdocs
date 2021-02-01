---
description: How to handle potential data errors in DataOps.
---

# !! Handling Data Validation Issues

In any data integration project, bad or non-conforming data is something that the development team will most likely encounter.  Data architects need to ensure an approach to handle those errors is part of the design to ensure that errors are caught and rerouted to the appropriate place.  This section suggests an approach that could be leveraged to capture and route off errors and warnings to a separate location for further analysis.

### Creating Validation Rules

The way that records are marked with Warn or Fail statuses are through the configuration of Validation Rules.  Refer to the [Rules section](../configuring-the-data-integration-process/source-configuration/enrichment-rule-configuration.md) in the documentation for detailed information about how to configure Validation rules.

The guideline for defining Validation Rules are to configure rules related to critical fields / checks to raise Failure status and all other rules to raise Warning status.

Some examples of validation checks that may warrant a Failure status are the following:

* Blank / NULL primary key fields
* Blank / NULL transaction date value \(if date value is a key data slicer for analysis\)

Some examples of validation checks that may warrant a Warning status are the following:

* Transactions with values that may be considered out-of-bounds for what they represent
* Population of attribute fields whose omission limits analysis but doesn't necessarily result in incorrect data \(i.e., customer or vendor name / code is missing\)

### Routing Output Data

Once data is flagged with an Error or a Warning status, those records should be routed off to a common error table for further analysis.  The recommended approach \(diagrammed below\) is the following:

* For records with a Passed status, data flows through to the target system
* For records raising a Warning status, data still flows through to the target system but is also routed to the error table
* For records raising an Error status, data does not flow through to the target system and is redirected to the error table

![Logical flow](../.gitbook/assets/image%20%28332%29.png)

![Data flow](../.gitbook/assets/image%20%28331%29.png)

The common error table should contain the following fields \(these should all be direct mappings\):

* **s\_source\_id:**  Source ID where the error record comes from/
* **s\_input\_id:**  Input ID related to the dataset where the error record originates.
* **s\_validation\_status\_code:**  Status code of the record \('Fail' or 'Warn'\)
* **s\_key:**  Key value of the record \(if source is a keyed source\).  This can be left blank for non-keyed sources.
* **s\_row\_id:**  Row ID of the errored record.

