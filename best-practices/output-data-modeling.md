---
description: Options for modeling output data coming out of RAP.
---

# Output Data Modeling

Once data is in RAP, there should be a path to expose that data externally, whether it's through a data hub, data warehouse, reporting tool, or various flat files that feed into other systems.

Flat files outputs feeding into other systems are highly dependent on the input requirements of those systems, and generally those outputs will be defined by those system requirements.  For the purposes of this section, the focus will be on modeling for an output database to feed into a reporting tool or layer.

### Flat Data Model

The primary data model RAP is designed for is the flat data model.  With the advent of columnar databases and columnstore indexes in traditional relational databases, a flat data model can be as performant as a traditional normalized schema, as well as use less storage through columnar compression technologies.

TODO - add details about what this is, show visually how this looks like

### Loose Dimensional Data Model

In traditional data warehousing, the [star schema](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/) has been the most widely used approach for modeling the reporting schema in the data warehouses for decades.  RAP is able to conform to a loose interpretation of this approach, allowing for dimensions to be leveraged while still using some of the benefits from the flat data modeling approach.

TODO - add rough diagram

### Hybrid Model

In some cases, a hybrid approach between the flat data modeling approach and the dimensional data modeling approach can be the right approach.

TODO - describe when to roll attributes into flat model and when to normalize to separate table

