# Intellio Processing Units

This section provides an overview of Intellio Processing Units (IPU) and how they are calculated to track usage of the platform for licensing.

## Intellio Processing Units Introduction

In order to best align with the licensing and cost model of the underlying services leverage by Intellio DataOps (e.g. Databricks, AWS/Azure, etc.), the product also adheres to a usage-based cost model which seeks to accurately align value and efficiency gained to price paid for the services.

Cloud providers such as Azure, AWS, Databricks, and Snowflake charge customers based off of the CPU and/or memory of the underlying infrastructure multiplied by the duration those resources are allocated to the customer's use. This aligns well with customer value, as their core services focus on simplifying the management and operations of those infrastructure resources, or the IT Ops around keeping those resources running reliably.

Although Intellio DataOps also provides services to help customers manage their infrastructure resources, the primary value is derived from data engineer development and operations workflow efficiency rather than improving or optimizing the infrastructure the platform runs on.

Because accurately tracking engineering time saved versus an alternative data architecture, methodology, and/or toolsets is not feasible, a surrogate estimation measurement must be used with as many input data elements as possible to ensure clear alignment between product pricing and value.

This measurement of data engineering efficiency for Intellio DataOps (IDO) is called an Intellio Processing Unit (IPU) and leverages the highly detailed metadata information on user configurations and process tracking to estimate the level of automation and number of processes managed by Intellio DataOps.

## Components of Calculation

All components of the IPU calculation are based on IDO processes.

A IDO process is an atomic unit of operation or calculation within the IDO meta-structure, with processes being generated automatically by the system based off the configurations and logic defined by the end users.

Please see the [Data Processing Engine](../logical-architecture-overview/data-processing-engine/) section for more information on processes.

The four major components which influence IPU consumption in order of impact are:

1. Flat base weight of each process, with different base weights by process type
2. Added flat weight for Refresh and Output process types by Source Refresh Type
3. Added calculated complexity weight for processes leveraging IDO Rules or Output Mappings
4. Logarithmic volume weight for Change Data Capture and Refresh process Types&#x20;

{% hint style="success" %}
IPUs are only charged for _successful_ processes. Failed processes have zero IPU cost.
{% endhint %}

The total IPUs for a single process are as follows:

([\<Base Weight>](intellio-processing-units.md#base-weight-by-process-type) of Process Type) +&#x20;

(IF Process Type IN (Refresh, Output) THEN [\<Refresh Type Weight>](intellio-processing-units.md#undefined) ELSE 0) +

(IF Process Type IN (Enrichment, Recalculate) THEN [\<Rules Weight>](intellio-processing-units.md#rules-weight) ELSE 0) +

(IF Process Type IN (Output) THEN [\<Output Mapping Weight>](intellio-processing-units.md#output-mapping-weight) ELSE 0) +&#x20;

(IF Process Type = Capture Data Changes THEN [\<Input Volume Weight>](intellio-processing-units.md#undefined) ELSE 0) +

(IF Process Type = Refresh THEN [\<Hub Table Volume Weight>](intellio-processing-units.md#undefined) ELSE 0)

### Controlling IPU consumption

In practical terms, users will see increased IPU usage when they:

1. Increase the number of runs/refreshes the systems processes monthly
2. Add more Sources, Rules, Outputs, and Output Mappings
3. Use primarily Key Refresh Type
4. Switch Rules from Snapshot to Keep Current
5. Pulling all data from source systems rather than incremental

Conversely, users can decrease their IPU usage by:

1. Moving tables which change slowly to a less-frequent refresh cadence
2. Leveraging Full, None, Sequence, or Timestamp Refresh Type for small volume or tables which can support these alternative Refresh Types
3. Leveraging Hard Dependencies rather than Keep Current to avoid Recalculation processes
4. Use dynamically injectable [tokens ](../user-manual/source-configuration/source-details.md#connection-type-specific-parameters)in the Source Query configuration to only pull incremental data, rather than the full table for each Input
5. For deployed/finalized Sources, disable optional processes such as Data Profiling which may not be used actively outside of initial development workflows

### Base Weight

The following table provides an overview of the base weights for each process:

| Process Name                                     | Base Weight |
| ------------------------------------------------ | ----------- |
| capture\_data\_changes                           | 2           |
| <p>manual_reset_all_<br>capture_data_changes</p> | 2           |
| <p>manual_reset_all_<br>processing_from_cdc</p>  | 20          |
| <p>manual_reset_capture_<br>data_changes</p>     | 2           |
| custom\_ingestion                                | 5           |
| custom\_parse                                    | 5           |
| custom\_post\_output                             | 5           |
| <p>manual_reset_custom_<br>parse</p>             | 5           |
| input\_delete                                    | 3           |
| enrichment                                       | 1           |
| <p>manual_reset_all_<br>enrichment</p>           | 1           |
| <p>manual_reset_<br>enrichment</p>               | 1           |
| import                                           | 10          |
| ingestion                                        | 1           |
| loopback\_ingestion                              | 1           |
| sparky\_ingestion                                | 1           |
| cleanup                                          | 0.5         |
| meta\_monitor\_refresh                           | 0.5         |
| manual\_reset\_all\_output                       | 1           |
| manual\_reset\_output                            | 1           |
| output                                           | 1           |
| manual\_reset\_parse                             | 2           |
| <p>manual_reset_sparky_<br>parse</p>             | 2           |
| parse                                            | 2           |
| sparky\_parse                                    | 2           |
| data\_profile                                    | 1           |
| attribute\_recalculation                         | 1           |
| <p>manual_attribute_<br>recalculation</p>        | 1           |
| refresh                                          | 1           |

These weights will remain largely static over time as long as the definition/scope of the process does not change.

An example of this type of change is a 2.5.0 feature enhancement. The rollback process was eliminated and manual reset change data capture for Keyed sources was completely refactored after feedback of long run times.

The same logical operation that used to generate 2-4 processes per Input to reset CDC for an entire source (resulting in potentially hundreds or even thousands of processes) now generates just one intelligent and process-intensive process.

As a result, we gave this large, manual, and complex operation (manual\_reset\_all\_\
processing\_from\_cdc) a very high base cost (20) to reflect the change. Despite this high base cost, this universally results in an overall reduction in IPU consumption and complexity of operating vs the old version.

### Refresh Type Weight

Intellio DataOps generates different workflows, process chains, and process sub-types based off the Refresh Type configuration.

Specifically, Sources configured as Keyed can generate the most complex and challenging workflows. To account for this, additional IPUs are generated based off what style of refresh is configured and is allocated to both Refresh and Output.

These weights are applied to every Refresh and Output operation:

| Refresh Type | Weight |
| ------------ | ------ |
| Key          | 1      |
| Timestamp    | 0.5    |
| Sequence     | 0.5    |
| Full         | 0.2    |
| None         | 0.1    |

### Rules Weight

For processes which leverage configured IDO rules, IPUs are calculated based off the number of and complexity of the rules compiled and executed as part of the process.

The following weights are applied to all Enrichment and Attribute Recalculation processes:

* Rule with compiled length <= 250 characters = +0.03 weight
* Rule with compiled length > 250 characters = +0.08 weight
  * See compiled expression under meta.enrichment -> expression\_parsed
  * Compiled expressions are used to normalize against any non-primary relation traversal syntax differences and source name lengths.
  * Compile expressions ensure users are not punished for using descriptive object names or long-form syntax for their business logic
* Expressions that include an aggregate function over a MANY relation traversal = +0.05 weight
* Expressions that include a window function = +0.05 weight

### Output Mapping Weight

Similar to rules, IPUs are calculated based off the number of and complexity of the mappings configured and executed as part of the process.

The following weights are applied to all Output processes:

* Base Mapping Weight(i.e. \[This].mycolumn) = +0.01 weight
* Mappings including a traversal through a relation = +0.03 weight
* Aggregate Function Mappings = +0.05 weight

### Input and Hub Table Volume Weight

Because cluster tuning and performance optimization becomes a major focus and feature set of Intellio DataOps once volumes move into the 100MB+ range per source, IDO also allocates IPUs based on the volume of data processed.

Because IDO does not process the data (Databricks processes the data) linear weight scaling does not align to business value or feature usage.

After analyzing numerous models, the following formula correlates volume to feature value:

![](<../.gitbook/assets/image (387) (1) (1).png>)

To illustrate how this complex formula materializes, the following table illustrates samples across various data volumes:

| Data Volume | Weight |
| ----------- | ------ |
| 1 KB        | 0.04   |
| 1 MB        | 0.32   |
| 10 MB       | 0.64   |
| 100 MB      | 1.28   |
| 1 GB        | 2.56   |
| 10 GB       | 5.12   |

As you can see, the weight for the IPU calculation doubles for each order of magnitude of data.

This results in a negligible IPU weight at lower volumes, and an increasing, but not exponential weight as the processing moves into the Big Data territory where infrastructure optimization becomes a critical operational consideration and feature.

This weight is applied to each Input File Size and attached to the Change Data Capture process.

This weight is also applied to the Hub Table Size and attached to the Refresh process.

While Hub Table Size is often difficult to optimize, Input File Size can be often reduced by applying upstream filtering and limiting the data ingested into IDO with each Input generated.

## Summary

While intimidating at first, the IPU weighting system is necessarily complex to capture all the various ways Data Engineers may leverage the features within IDO, while also avoiding overly burdensome costs when specific corner cases or scale-out solutions are implemented.

Any changes to the weighting system will be tested thoroughly to avoid any changes to expected monthly costs for existing customers, and will primarily be implemented to reduce IPU costs for existing inefficient workflows or processes, or as part of a net-new feature release that will not impact existing Source configurations.
