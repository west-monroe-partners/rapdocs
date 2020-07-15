---
description: >-
  Areas within RAP where data is stored throughout the data processing life
  cycle.
---

# Data Storage

![](../.gitbook/assets/2.0-process-steps.jpg)

### Data Lake

The data lake consists of a Raw data landing area and Transformation area used for RAP's processing steps.

TODO - discuss Raw vs. Transform storage

### Data Hub

The data hub consists of Hive tables that contain the final processed data for each source.  Each table maps one-to-one with configured data sources, and those tables automatically generated and maintained by RAP.  This layer can be accessed via SQL syntax and is ideal to be used as an exposure point for data exploration purposes and sharing data with other systems.  For implementations intended for data exploration or sharing purposes, this could very well be the end of the data flow for RAP if requirements do not dictate a reporting need.

TODO - how to connect / query?

### Data Warehouse

The Data Warehouse layer is the exposure layer for curated and cleansed data.  This layer is more tightly maintained than the Data Hub, and is controlled in RAP by configured output mappings.  This is the layer intended to be exposed for enterprise reporting and analytics purposes.

The data warehouse will generally reside on a data storage technology outside of RAP, such as SQL Server \(and its various Azure counterparts\) or Snowflake.

