---
description: >-
  The RAP ecosystem follows the data flow from raw data to organized data
  warehouse.
---

# How it Works

## The Logical Data Flow

![The RAP Data Flow and corresponding storage locations](../../.gitbook/assets/rap-logical-data-flow-new.png)

As seen in the above data flow diagram, RAP intakes raw data and outputs an organized data warehouse. The Logical Data Flow consists of eight steps: Ingest, Parse, CDC, Enrich, Refresh, Re-Calculate, Output, and Post-Process. The Data Storage Flow consists of four main structures: Raw Data, Data Lake, Data Hub, and Data Warehouse. Though not shown in the diagram, each step in each flow consists of generated and configuration meta data to aid the user in process management and configurations respectively. 

To illustrate this introductory overview of the logical data flow we will focus in on the four data structures \(Lake/Raw, Lake/Transform, Data Hub, Data Warehouse\) and a selection of key steps.

### RAP Data Lake / Raw

RAP does not generate data. RAP relies on external **raw data** sources, and RAP supports many types of data sources such as flat CSV Files, databases or data warehouses. This raw data exists somewhere and the first step of the logical data flow \(ingest\) relies on appropriately copying this raw data to the Raw area of the **Data Lake**.

{% hint style="info" %}
#### External Data Sources - ERP, CRM, HR, FIN, etc.

RAP does not generate or store business data, rather it collects this information from various **External Data Sources**. The nature of these sources can vary, and RAP has existing modules built for the most common data stores used by enterprise organizations.
{% endhint %}

#### Ingest

Ingest is the first step of the logical data flow and is the process by which data enters RAP. The **ingest** and **parse** steps appropriately copy raw data and convert this raw data into workable formats. The ingest process is facilitated by the Connections and Sources screens in the RAP interface. 

### RAP Data Lake / Transform

RAP imports the appropriately transformed raw data from source systems into an indexed **Data Lake**, which utilizes Amazon's S3 or Microsoft Azure's Data Lake Gen2 file storage technology. As RAP imports the data into the Data Lake, it identifies and stores information such as source database, original file location, and location in the Data Lake within the metadata structure, resulting in a self-managing Data Lake, useful for data scientists to access the raw information gathered on a scheduled basis.

#### Enrichment

**Enrichment \(Enrich\)** is the step of the logical data flow that applies data quality checks and executes business logic. This happens within the Data Hub, where user-specified configuration rules drive transformations and processing in several automated steps. The Enrichment rules represent the majority of the logic and structure of data processing within RAP, and provide a flexible, yet guided framework for data management. The **Refresh** and **Recalculate** steps are also tightly coupled to this process, as those steps involve ensuring that historical data that needs to have their enriched fields kept current will be updated.

The Enrichment step is facilitated by the Enrichment tab within the Source screen in the RAP interface. 

{% hint style="info" %}
**Enrichments Execution**

RAP supports many types of enrichments, and not all types of business logic can be executed at the same time. Based on the type of enrichment, calculation, or aggregation the enrichment will execute at a different, appropriate time in the logical data flow or data processing actions. RAP's interface keeps the details of these executions behind the scenes and user friendly.
{% endhint %}

### RAP Data Hub

The **Data Hub** consists of a Hive table optimized to process and store data for RAP. As data moves through processing steps, RAP automatically and continuously modifies the underlying table structures to ensure optimal performance.

#### Output

The **Output** step of the logical data flow and maps transformed data from the Data Hub to a Data Warehouse or 3rd party. Output typically consists of very limited transformation logic, instead focusing on simple mappings of data fields from the Data Hub to the final location.

The Output step is facilitated by the connections and outputs screens in the RAP interface.

### RAP-Managed Data Warehouse

The **Data Warehouse** data structure is the final of the data structures in the RAP data processes. The Data Warehouse focuses on optimized access for BI tools and analytics. RAP supports most column-oriented storage technologies, such as SQL Server, Snowflake, Parquet, etc. and can fully manage the table structures, data models, and maintenance operations within the Warehouse. RAP can also output data into a manually managed, existing data warehouse if needed.

#### Post-Process

The **Post-Process** step of the logical data flow runs any aggregations or calculations that need to cut across multiple sources. This step is intended for pre-aggregating data when needed for performance reasons for reporting consumption.

### RAP Metadata Structure

RAP uses a **Metadata Structure** to manage information about configuration, process tracking, and data tracking throughout the entire platform. RAP utilizes a proprietary table structure to relate all these data points, and stores this information in a Postgres database for easy access.

## Historical Framework \(RAP 1.X\)

RAP's 1.x framework was viewed as four storage structures and four data processing steps. The four storage structures map one to one between the two frameworks, but the eight steps of the logical data flow only roughly map into the four data processing steps. For reference the [historical framework](../../historical-reference/components-and-concepts.md) section is provided.

