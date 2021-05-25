---
description: >-
  A connection holds the credentials, network locations, and any other
  parameters required to access data in the location where it is either
  generated or staged for ingestion by Intellio DataOps
---

# Connections

## Connections

The connections home page enables users to quickly search and access connections already configured in the DataOps platform.

Only Connections marked as Active are shown here, unless the Active Only toggle is set to off.

![](../.gitbook/assets/image%20%28346%29.png)

## Connection Settings

The connection settings page enables users to provide the necessary parameters to enable DataOps to access the system.

![](../.gitbook/assets/image%20%28347%29.png)

* **Name\*:** A unique name
* **Description\*:** A description
* **Uses Agent\*:** A visual indicator showing whether or not this connection will use an Agent, and the configuration of which [Agent ](../logical-architecture-overview/rap-agent.md)will be used
* **Active:** Allows users to disable the connection without deleting the configuration
* **Connection Direction:** Specifies if this connection is used to ingest or output data
* **Connection Type:** Specifies the format or location style of the source or target data. ****Depending on the Type selected, the remaining parameters will change

## Custom

Used in the [SDK ](sdk/)as part of [Custom Ingestion](sdk/custom-ingestion.md).

Parameters here are optional, as not all custom ingestion notebooks require parameterization.

These parameters should be a JSON object in format of  
{"key1": "value1", "key2": "value2"}

For connections that do not need any parameters, enter an empty JSON object {}

* **Public Connection Parameters\*:**  Passed in as plain-text to the custom ingestion session
* **Private Connection Parameters\*:** These parameters are stored on save into the [Databricks Secrets](https://docs.databricks.com/security/secrets/index.html) of the respective cloud service

## File

* **Storage Technology\*:** Specifies the type of file storage Agent/Databricks will attempt to access
* **File Path\*:** The folder/container path for DataOps to access when pulling or generating files

## Table

* **Driver\*:** Which JDBC driver should be used

## Parameters

The parameters section will change dynamically based on the required selections above, are typically optional to configure, and are used for advanced configuration or specifications.



