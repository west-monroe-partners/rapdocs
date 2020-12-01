---
description: >-
  Areas within DataOps where data is stored throughout the data processing life
  cycle.
---

# Data Storage

![Data locations below each step of the logical data flow.](../.gitbook/assets/2.0-process-steps.jpg)

## Data Processing

Intellio DataOps does not create data. Intellio DataOps copies, moves, processes, transforms, and performs calculations on data. The location where these actions take place through the logical data flow are raw data, data lake, data hub, and data warehouse.

### Raw Data

The first steps in the logical data flow creates a copy of the raw the data in a location that is accessible for the processing steps. In Ingest and Parse data is copied and taken stock of \(size, tabular format\). This data is stored in the data lake location within the Microsoft Azure or AWS infrastructure based upon the implementation. 

### Data Lake

In the CDC step the data lands in the core processing location of Intellio DataOps. The Data Lake at this step ensures all data is appropriately transformed and is in a uniform format with the appropriate documentation, metadata, and format so that the following steps can focus strictly on processing. 

### Data Hub

The Data Hub are tables that contain the final processed data for each source.  Each table maps one-to-one with configured data sources, and those tables are automatically generated and maintained by DataOps.  This layer can be accessed via SQL syntax and is ideal to be used as an exposure point for data exploration purposes and sharing data with other systems.  For implementations intended only for data exploration or sharing purposes, this could very well be the end of the data flow for DataOps if requirements do not dictate a reporting need.

The Data Hub can be queried directly via Databricks or Hive Tables depending on if the infrastructure is AWS or Microsoft Azure.

### Data Warehouse

The Data Warehouse layer is the exposure layer for curated and cleansed data.  This layer is more tightly maintained than the Data Hub, and is controlled in DataOps by configured output mappings.  This is the layer intended to be exposed for enterprise reporting and analytics purposes.

The data warehouse will generally reside on a data storage technology outside of DataOps, such as SQL Server \(and its various Azure counterparts\) or Snowflake.

To generate the Data Warehouse in Intellio DataOps UI the user utilizes the Outputs UI and sets the appropriate column \(and/or filtering\) mapping.

The Outputs Mapping Tab dictates how the Output data is mapped to the end Data Warehouse location.

