---
description: Options for modeling output data coming out of DataOps.
---

# !! Output Data Modeling

Once data is in DataOps, there should be a path to expose that data externally, whether it's through a data hub, data warehouse, reporting tool, or various flat files that feed into other systems.

Flat file outputs feeding into other systems are highly dependent on the input requirements of those systems, and generally those output layouts will be defined by those system requirements.  For the purposes of this section, the focus will be on modeling for an output database to feed into a reporting tool or layer.

### Background

The [Kimball star schema](https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/dimensional-modeling-techniques/) has been the most widely used approach for modeling the user-exposed reporting schema in data warehouses for several decades.  It provides a great balance of model simplicity and query performance on traditional relational databases.  However, with the more modern technologies that exist today \(primarily columnar compression and columnstore indexes, as well as more advanced modeling functionality in reporting tools\), the star schema as originally defined may no longer be the optimal solution in most cases.  Rather, alternative takes on the Kimball star schema may be a better answer.

{% hint style="info" %}
**NOTE:**  None of the modeling approaches suggested here eliminate the need to understand the concepts of the Kimball methodology.  Rather, the approaches discussed here are an extension of that methodology to better align the physical representation of the data model to capabilities in storage and reporting technologies available today.
{% endhint %}

### "One Big Table" Model

The primary data modeling methodology that DataOps is designed for is the "One Big Table" model.  As it's name suggests, the idea is to encapsulate as much of the reporting data model as possible into a single \(sparsely populated\) flat table.  This approach takes advantage of the columnar compression offered in modern relational databases and other large-scale cloud storage offerings \(such as Snowflake and Azure Synapse, for example\).  A single big table can be as performant as a traditional normalized schema, as well as use less storage than a traditional rowstore star schema and be much less complex to use.

{% hint style="info" %}
**NOTE**:  The One Big Table approach should only be used for outputs going to columnar compressed tables or to Virtual Outputs.  Outputting to a rowstore-oriented technology can lead to excessive I/O and storage consumption.
{% endhint %}

The methodology that needs to be followed with this approach is the following:

1. Determine logical dimensions and facts, as well as relationships between dimensions and facts.  Note that this step still leverages traditional Kimball methodology for modeling.
2. Once logical dimensions / facts and associated relationships are determined, denormalize all dimensional attributes into each fact table.
3. Merge all denormalized fact entities into a single large entity.
   1. Any shared dimensional attributes \(i.e., conformed dimensions\) should use the same field across all relevant fact grains.
   2. All measures should use fields that are _**not**_ shared with any other fact grain.

![An example of a star schema collapsed into One Big Table.](../.gitbook/assets/image%20%28259%29.png)

### 

### Loose Dimensional Data Model

The loose dimensional model concept is a variation of the traditional star schema.  DataOps is able to conform to a loose interpretation of the Kimball star schema, allowing for dimensions to be leveraged while still using some of the benefits from the flat data modeling approach.

The primary way that a loose dimensional model deviates from a traditional star schema is the lack of surrogate key generation.  As a result, dimension tables are tied to the fact tables on either a single natural key or a concatenated composite key.  The primary benefit is that it decouples the dependencies between dimension and fact tables.  There is no need to handle early-arriving facts separately, since the need to generate a placeholder dimension key is eliminated.  Most modern reporting tools on the market today are able to handle the loose relationships between fact and dimension tables with this approach, meaning we don't need to deal with the complexity of handling early-arriving facts within DataOps.

The general recommendation for DataOps is to stick to the "One Big Table" model as much as possible, but if requirements determine that a more traditional star-like schema is needed, the loose dimensional model is also a viable option.

For additional information about the loose dimensional data model and the rationale behind the approach, please refer to [this resource](https://www.westmonroepartners.com/perspectives/resource/data-modeling-approach-leverage-analytics-reporting) on our website.

### Hybrid Model

In some cases, a hybrid approach between the One Big Table data modeling approach and the loose dimensional data modeling approach may be the right design.  As the One Big Table design tracks dimensional attributes as of a single point in time by default, dimensional tables may need to be created when those attributes need to be kept current and reprocessing affected records would be prohibitively expensive.  In this scenario, the needed dimension table\(s\) relate directly to the One Big Table, resulting in a physical data model looking like a star schema with only a single center node.

