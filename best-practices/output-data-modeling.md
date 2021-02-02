---
description: Options for modeling output data coming out of DataOps.
---

# !! Output Data Modeling

Once data is in DataOps, there should be a path to expose that data externally, whether it's through a data hub, data warehouse, reporting tool, or various flat files that feed into other systems.

Flat file outputs feeding into other systems are highly dependent on the input requirements of those systems, and generally those outputs will be defined by those system requirements.  For the purposes of this section, the focus will be on modeling for an output database to feed into a reporting tool or layer.

### Background

The [Kimball star schema](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/) has been the most widely used approach for modeling the user-exposed reporting schema in data warehouses for several decades.  It provides a great balance of model simplicity and query performance on traditional relational databases.  However, with the more modern technologies that exist today \(primarily columnar compression and columnstore indexes, as well as more advanced modeling functionality in reporting tools\), the star schema as originally defined may no longer be the optimal solution in most cases.

### "One Big Table" Model

TODO - update this section based on latest OBT content

The primary data model that DataOps is designed for is the "One Big Table" model.  As it's name suggests, the idea is to encapsulate as much of the reporting data model as possible into a single flat table.  This approach takes advantage of the columnar compression offered in modern relational databases and other large-scale cloud storage offerings \(such as Snowflake and Azure Synapse, for example\).  A single big table can be as performant as a traditional normalized schema, as well as use less storage than a traditional rowstore star schema and be much less complex to use.

{% hint style="info" %}
**NOTE**:  The One Big Table approach should only be used for outputs going to columnar compressed tables or to Virtual Outputs.  Outputting to a rowstore-oriented technology can lead to excessive I/O and storage consumption.
{% endhint %}

TODO - add details about what this is, show visually how this looks like

TODO - add use cases where this works

TODO - discuss union concept, add viz

![An example of a star schema collapsed into One Big Table.](../.gitbook/assets/image%20%28259%29.png)

### 

### Loose Dimensional Data Model

The loose dimensional model concept is a variation of the traditional star schema.  DataOps is able to conform to a loose interpretation of the Kimball star schema, allowing for dimensions to be leveraged while still using some of the benefits from the flat data modeling approach.

The one way that a loose dimensional model deviates from a traditional star schema is the lack of surrogate key generation.  As a result, dimension tables are tied to the fact tables on either a single natural key or a concatenated composite key.  The primary benefit is that it decouples the dependencies between dimension and fact tables.  There is no need to handle early-arriving facts separately, since the need to generate a placeholder dimension key is eliminated.  Most modern reporting tools on the market today are able to handle the loose relationships between fact and dimension tables with this approach, meaning we don't need to deal with the complexity of handling early-arriving facts within DataOps.

The general recommendation for DataOps is to stick to the flat data model as much as possible, but if requirements determine that a more traditional star schema is needed, the loose dimensional model is also an option.

For additional information about the loose dimensional data model and the rationale behind the approach, please refer to [this resource](https://www.westmonroepartners.com/perspectives/resource/data-modeling-approach-leverage-analytics-reporting) on our website.

### Hybrid Model

In many cases, a hybrid approach between the One Big Table modeling approach and the loose dimensional data modeling approach can be the right design.

TODO - describe when to roll attributes into flat model and when to normalize to separate table

TODO - add use cases where this works

