---
description: How to most efficiently perform pre-aggregations within DataOps.
---

# !! Pre-Calculating Aggregate Data

In most scenarios, the lowest grain of transactional data will be output by the data processing layer, and the reporting tool will be responsible for any dynamic aggregations that need to be performed in response to end-user queries.  However, some reporting scenarios may call for aggregating data within the data processing layer \(whether that is to calculate an intermediate value or creating an aggregate table for reporting performance needs\).  This section provides guidance on when aggregations within DataOps may be appropriate, as well as guidelines on how those aggregations should be configured.

### When to aggregate in DataOps?

Some examples of when aggregations in DataOps may be used include the following:

* Calculating the denominator of a percentage calculation \(ex: for a percentage of sales calculation\)
* Calculating a running total

Some examples where aggregations should **not** be done in DataOps are the following:

* Simple aggregations for the sole purpose of an output grain change or a lookup from another source. Generally, the lowest grain should be output to the destination, and any higher-grain lookups can be handled via X:Many lookup patterns.
* Aggregating a lower grain measure up to a higher grain for the sole purpose of exposing that derivation. This should generally be handled by the downstream reporting / exposure tool instead.

### How to aggregate data in DataOps?

#### Window Functions

The primary method to aggregate data in the context of a single source is to use a window function.  This is implemented though an enrichment rule.  Note that when using this method, the same value will be repeated across all records within each partition \(the one exception being running totals\).

Some example use cases include the following:

* Sum on current grain as part of an intermediate calculation
  * Ex: For a % of sales calculation of a Sales Order for a Customer, the denominator needs to be calculated as **SUM\(\[This\].sales\_amount\) OVER \(PARTITION BY \[This\].customer\_id\)**
* Calculating a running total as part of an intermediate calculation\
  * Ex: **SUM\(\[This\].invoice\_commission\) OVER \(PARTITION BY \[This\].UniqTransHead\)**

![Sample window function enrichment rule](../.gitbook/assets/image%20%28326%29.png)

#### X:Many Relation Aggregations

Another common pattern is to aggregate by traversing to the Many side of a One-to-Many or Many-to-Many relationship.  For example, a sum of Sales Order Lines may need to be rolled up to the Sales Order Header grain for a calculation.  When done in this manner, DataOps is able to determine the right grain to aggregate to automatically, as it will use the lowest aggregation grain needed to retain the grain of the driving side of the relation \(in our example, Line data will be aggregated to the Header grain\).

Information about how this pattern works are covered in detail in the [Crossing X:Many Cardinality Relations section](crossing-x-many-cardinality-relations.md#getting-an-aggregated-value) of this document.

### Special Considerations

A few things to keep in mind when aggregating data in DataOps:

* Aggregations run in the context of an entire source \(not just the latest incremental data\).  Therefore, the existence of aggregate calculations in a source can significantly impact the amount of time to process incremental data \(especially when the incremental data volume is much less than the total data volume\).
* All aggregations need to run in the context of an entire source, all aggregations done through an enrichment rule will need to have the "Keep Current" recalculation mode selected.

