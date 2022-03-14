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

(IF Process Type IN (Enrichment, Recalculate) THEN \<Rules Weight> ELSE 0) +

(IF Process Type IN (Output) THEN \<Output Mapping Weight> ELSE 0) +&#x20;

(IF Process Type = Capture Data Changes THEN \<Input Volume Weight> ELSE 0) +

(IF Process Type = Refresh THEN \<Hub Table Volume Weight> ELSE 0)

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

| Process Name                                     | Process Group | Base Weight |
| ------------------------------------------------ | ------------- | ----------- |
| capture\_data\_changes                           | cdc           | 2           |
| <p>manual_reset_all_<br>capture_data_changes</p> | cdc           | 2           |
| <p>manual_reset_all_<br>processing_from_cdc</p>  | cdc           | 20          |
| <p>manual_reset_capture_<br>data_changes</p>     | cdc           | 2           |
| custom\_ingestion                                | custom        | 5           |
| custom\_parse                                    | custom        | 5           |
| custom\_post\_output                             | custom        | 5           |
| <p>manual_reset_custom_<br>parse</p>             | custom        | 5           |
| input\_delete                                    | delete        | 3           |
| enrichment                                       | enrichment    | 1           |
| <p>manual_reset_all_<br>enrichment</p>           | enrichment    | 1           |
| <p>manual_reset_<br>enrichment</p>               | enrichment    | 1           |
| import                                           | import        | 10          |
| ingestion                                        | ingestion     | 1           |
| loopback\_ingestion                              | ingestion     | 1           |
| sparky\_ingestion                                | ingestion     | 1           |
| cleanup                                          | maitanence    | 0.5         |
| meta\_monitor\_refresh                           | maitanence    | 0.5         |
| manual\_reset\_all\_output                       | output        | 1           |
| manual\_reset\_output                            | output        | 1           |
| output                                           | output        | 1           |
| manual\_reset\_parse                             | parse         | 2           |
| <p>manual_reset_sparky_<br>parse</p>             | parse         | 2           |
| parse                                            | parse         | 2           |
| sparky\_parse                                    | parse         | 2           |
| data\_profile                                    | profile       | 1           |
| attribute\_recalculation                         | recalculate   | 1           |
| <p>manual_attribute_<br>recalculation</p>        | recalculate   | 1           |
| refresh                                          | refresh       | 1           |

These weights will remain largely static over time as long as the definition/scope of the process does not change.

An example of this type of change is a 2.5.0 feature enhancement. The rollback process was eliminated and manual reset change data capture for Keyed sources was completely refactored after feedback of long run times.

The same logical operation that used to generate 2-4 processes per Input to reset CDC for an entire source (resulting in potentially hundreds or even thousands of processes) now generates just one intelligent and process-intensive process.

As a result, we gave this large, manual, and complex operation (manual\_reset\_all\_\
processing\_from\_cdc) a very high base cost (20) to reflect the change. This universally results in an overall reduction in IPU consumption and complexity of operating vs the old version.

### Refresh Type Weight

Intellio DataOps generates different workflows, process chains, and process sub-types based off the Refresh Type configuration.

Specifically, Sources configured as Keyed can generate the most complex and challenging workflows. To account for this, additional IPUs are generated based off what style of refresh is configured and is allocated to both Refresh and Output

### Weighted Calculation of Complexity

Not all processes are created equally, with some simply passing the data forward to the next processing stage, and others which execute 1000s of lines of generated SQL against billions of rows of data.

Because of the rich metadata IDO stores about each and every code configuration at an extremely granular level, the complexity of each process can be estimated accurately by summarizing and then weighting the configuration applied to each process at time of execution.

The two major sub-components of complexity are:

1. Process Type Base Factor
2. Number of, type, and complexity of rules and/or mappings applied, if applicable

The calculation for Weighted Base Factor for a specific process can be broken down as:

Weighted Base Factor = (Process Type Base Factor) + SUM(Rules/Mappings weight)/100

#### Process Type Base Factor

Some process types require minimal configuration, but have high levels of automation when compared to alternative approaches, others require heavy configuration and provide heavy automation, while a few are extremely lightweight and are separate processes for application architecture purposes.

To account for this, each Process Type has a base factor to either increase or decrease the IPU calculation for each process successfully completed of that Process Type.

These Process Type Base Factors will be published and maintained in the meta.process\_type table starting in version 2.5.1.

#### Rules / Mappings Factors

Each configuration implementing business logic or transformations within IDO will increase the complexity of the codebase generated and managed by the platform. This factor is calculated using metadata about Source Rules and Output Mappings with the following weightings:

Rules:

* Rule with compiled length <= 250 characters = +3 weight
* Rule with compiled length > 250 characters = +8 weight
  * See compiled expression under meta.enrichment -> expression\_parsed
  * Compiled expressions are used to normalize against any non-primary relation traversal syntax differences and source name lengths.
  * Compile expressions ensure users are not punished for using descriptive object names or long-form syntax for their business logic
* Expressions that include an aggregate function over a MANY relation traversal = +5 weight
* Expressions that include a window function = +5 weight

Output Mappings:

* Base Mapping Weight(i.e. \[This].mycolumn) = +1 weight
* Mappings including a traversal through a relation = +3 weight
* Aggregate Function Mappings = +5 weight

Rules are only counted/added to the weight&#x20;
