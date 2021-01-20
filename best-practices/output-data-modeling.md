---
description: Options for modeling output data coming out of RAP.
---

# !! Output Data Modeling

Once data is in RAP, there should be a path to expose that data externally, whether it's through a data hub, data warehouse, reporting tool, or various flat files that feed into other systems.

Flat file outputs feeding into other systems are highly dependent on the input requirements of those systems, and generally those outputs will be defined by those system requirements.  For the purposes of this section, the focus will be on modeling for an output database to feed into a reporting tool or layer.

### Background

The [star schema](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/) has been the most widely used approach for modeling the user-exposed reporting schema in data warehouses for several decades.  It provides a great balance of model simplicity and query performance on relational databases.  However, with the more modern technologies that exist today \(primarily columnar compression and columnstore indexes\), the star schema as it exists may no longer be the optimal solution in many cases.

### Flat Data Model

The primary data model RAP is designed for is the flat data model.  The flat data model takes advantage of the colulumnar compression offered in relational databases and other large-scale cloud storage offerings \(such as Snowflake and Azure Synapse\).  A flat data model can be as performant as a traditional normalized schema, as well as use less storage than a traditional rowstore star schema through columnar compression.

{% hint style="info" %}
**NOTE**:  Flat data models should only be used for outputs going to columnar compressed tables or to RAP Virtual Outputs.  Outputting to a rowstore-oriented technology can lead to excessive I/O and storage consumption.
{% endhint %}

TODO - add details about what this is, show visually how this looks like

TODO - add use cases where this works

TODO - discuss union concept, add viz

![An example of a star schema collapsed into a single flat table.](../.gitbook/assets/image%20%28259%29.png)

### 

### Loose Dimensional Data Model

The loose dimensional model concept is a variation of the traditional star schema.  RAP is able to conform to a loose interpretation, allowing for dimensions to be leveraged while still using some of the benefits from the flat data modeling approach.

The one way that a loose dimensional model deviates from a star schema is the lack of surrogate key generation.  As a result, dimension tables are tied to the fact tables on either a single natural key or a concatenated composite key.  The primary benefit is that it decouples the dependencies between dimension and fact tables.  There is no need to handle early-arriving facts separately, since the need to generate a placeholder dimension key is eliminated.

The general recommendation for RAP is to stick to the flat data model as much as possible, but if requirements determine that a more traditional star schema is needed, the loose dimensional model is also an option.

TODO - add rough diagram

TODO - add use cases where this works

### Hybrid Model

In many cases, a hybrid approach between the flat data modeling approach and the loose dimensional data modeling approach can be the right design.

TODO - describe when to roll attributes into flat model and when to normalize to separate table

TODO - add use cases where this works

