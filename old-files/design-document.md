# Design Document

## 1. Design Considerations

### 1.1 Standardization

RAP simplifies the ETL process by eliminating custom ETL pipelines for each data source. Instead, RAP accommodates any data source by leveraging a standardized data model and ETL code.

### 1.2 Simplicity

Because of the standardized data model and ETL process, the complexity of a traditional ETL process disappears. RAP manages work tables for you, reducing code, and eliminating tedious tasks such as column mapping and schema management.

### 1.3 Scalability

RAP scales well through distributed concurrency for core processing and a unique deployment model on PostgreSQL, enabling data volumes to scale up to Terabytes per day in throughput. Unlimited scalability can be achieved by moving to a distributed computing platform such as a combination of Cassandra and HDFS, however the only currently supported core database option is PostgreSQL\*.

> \*Note: RAP supports all data sources/destinations – this only applies to the core OLTP storage system.

## 2. RAP Processing Overview

RAP ingests high volumes of data from a variety of sources and makes it available for easy consumption by customizable outputs while enforcing data quality constraints. RAP accomplishes this through four processing phases: Input, Staging, Validation/Enrichment, and Output.![](/images/design_doc.png)

Figure 2a

This document details the critical elements involved in the RAP processing phases.

## 3. Key Platform Elements

### 3.1 Source

A Source represents some connection of raw data that we would like to bring into and through the platform. Sources are differentiated via their source path parameter, which can be anything from an FTP folder path, API call, local system file path, HTTP address, etc.

#### **3.1.1 Source Types**

**Time Series:** Time Series Sources represent the lowest level of detail in many data models. As the name suggests, Time Series Sources typically represent a transaction, event, or data point uniquely identified by the time it occurred, and the data contained in the row. Once written, this data never gets modified or updated. Because of this, RAP can optimize its processing, resulting in massive overall performance boosts due to Time Series Sources typically representing the highest volume of data in ETL systems. In traditional star schema data models, this data would act as the base for Fact tables.

**Keyed:** Keyed Sources have a unique \(primary/business\) key to identify a record. These Sources represent attributes which require updating over time, such as account records, customer information, product information, etc. Traditionally analogous to Dimensions in star-schema models, these Sources act as the lookups/joins between other disparate Sources and concepts. In RAP, Keyed Sources get special treatment versus Time Series Sources – they undergo CDC \(Change Data Capture\) processes, with full support for either Type 1 or Type 2 slowly changing dimensions.

#### **3.1.2 Input Types**

Input type refers to the method by which RAP attains the data source. RAP currently supports 4 input types: File Pull, File Push, Platform Source, and Table. In a future state, RAP will also support FTP Pull.

| Input Type | Description |
| :--- | :--- |
| File Pull | Data source is a flat file to be ingested at a scheduled time and cadence. |
| FTP Pull | Brings a flat file into the platform at a scheduled time and cadence, but instead of a local file path, a FTP server address and file path defines the Input’s location. |
| File Push | Monitors file/folder paths, immediately beginning the processing flow once files appear in the monitored path\(s\). |
| Platform Source | In the event that an Output needs to be represented with a different grain, the Platform Source type reprocesses an existing data source at the desired grain. |
| Table Pull | Brings data from a database table into the platform. A SQL query extracts data from the database and the platform creates a flat file with the query results. Runs on a schedule. |

### 3.2 Validation & Enrichment Rules

#### **3.2.1 Validation Rule:**

A Validation Rule uses templated logic, based on SQL WHERE statements, to enforce data quality constraints on the ingested data. For example, a rule may enforce that all values in a column related to Product Sales must be greater than zero.

RAP supports two types of Validation rules: Fail and Warn. Records that don’t satisfy the validation criteria get flagged with either a Fail or Warn status. These flags serve as the basis for filtering out data during Output creation. Additionally, a link to the triggered rule is added to the infringing record for easy reference downstream.

#### **3.2.2 Enrichment Rule:**

RAP takes a unique approach to enriching data from a single source and combining data from multiple sources. Under the hood, RAP operates on a completely de-normalized, or flat, data model. Because of this model, the enrichment rules focus on only two types of processes: calculated columns and lookups from Keyed Sources.

### 3.3 Output

| Output Type | Description |
| :--- | :--- |
| Flat File | Standard flat file \(comma-separated values, text, Excel\), created in a standard file system directory. |
| Table | Database table, created in a specified database. |
| Platform Source | Used for reprocessing previously ingested data at a different grain. |

Upon completion of the Validation phase, the platform builds an Output file or table based on Output configuration parameters. The Output phase moves the data from the Staging table to the specified Output location.

## 4. Configuration

RAP data processing is highly parameterized, eliminating the need for custom ETL development. This shift can decrease time spent on data integration implementation by over 60%. Once the Source, Validation rules, Enrichment rules, and Outputs are configured properly, RAP takes care of the rest behind the scenes. Below is an overview on how to configure each phase. For additional detail on configurations, refer to the **RAP User Manual.**

### 4.1 Source

Source configuration provides the RAP Platform with the information it needs to bring the data into the platform by providing the Input Type \(see Source → Input Types\) and the location of the source data, described below:

| Source Type | Location |
| :--- | :--- |
| Flat File | File path, file name pattern |
| Table | Source database, SQL query returning desired data. |
| FTP | FTP Server, file name pattern. |

Once the platform obtains the desired data source, it parses the data, capturing metadata about the file/table, then writes the data into the platform’s standard data model.

### 4.2 Validation & Enrichment Rules

Validation & Enrichment Rule templates serve as standardized blueprints for source-specific Validation & Enrichment Rules. Templates solely consist of a parameterized SQL WHERE statement identifying the records of the data source that do not satisfy the rule’s logic.

Validation Rule configuration requires selecting a template, assigning source fields to replace the parameters, and selecting a rule type \(see Validation & Enrichment section of Key Terms – Platform Elements\).

Like Validation Rule configuration, Enrichment Rule configuration requires selecting a template and assigning source fields to replace the parameters. The configuration builds the enriched attribute by supplying it with a name, data type, and enrichment type. Enrichment type describes two operations RAP supports to create a new column: calculated column based on SQL SELECT logic and integrating a Keyed source via Lookup.

### 4.3 Output

Output configuration allows the Platform to export validated data to its final destination, by providing the desired Output location. Each Output type has slightly different location metadata:

| Source Type | Location |
| :--- | :--- |
| Flat File | Output path folder, file name |
| Table | Schema name, table name |
| Platform Source | In the event that an Output needs to be represented with a different grain, the Platform Source type reprocesses an existing data source at the desired grain. |

Once the Platform knows where to create the Output, the configuration details how to create the Output by providing desired column names, data types, and source-target mappings.

### 4.4 Schedule

The Table Pull and File Pull Input types rely on a schedule. RAP uses Cron \(using Cron4s format\) scheduling to facilitate schedule executions. When the user-defined scheduled time arrives, RAP ingests the data source. If the source fails to be ingested, the source will be rescheduled based on user-defined rescheduling logic.

## 5. Monitoring

RAP uses processing metadata to monitor all phases, displaying these metadata points on the front end user interface. Phases are tracked with statuses including pending, ready, in-progress, success, and failure, as well as the duration. The staging phase metadata captures the number of records passing through the phase, and validation & enrichment metadata captures the number of records that pass, fail, or warn. The status of these processing events can be communicated via email alerts.

## 6. Deployment and Metadata Migration

Metadata configurations drive all phases of RAP data processing, so deployment to a different server instance is as simple as migrating the metadata. RAP supports a full suite of metadata migration functionality, which migrates the metadata of sources, validation rules, outputs, and output-source mappings. RAP exports metadata from one instance as a JSON file, and imports it into another instance. These functions can be customized to update existing instances, insert metadata into new or existing instances, or simply compare the differences between two instances.

## 7. Agent

To access source data that sits behind a firewall, RAP comes with an Agent that can be installed as a service on any Linux-based or Windows machine. The Agent performs the Input processing phase, bringing the source data into RAP, where subsequent processing phases are executed. The initial platform deployment includes full agent configuration.  


