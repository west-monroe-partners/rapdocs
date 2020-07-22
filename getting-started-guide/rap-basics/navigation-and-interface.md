---
description: The RAP interface consists of six primary screens
---

# !!Navigation and Interface

## Navigation Menu

Upon login, users are directed to the Sources screen. The Navigation Menu, located in the top left corner of the screen, enables navigation between RAP screens from any of the currently displayed screens.

![Left-Hand Navigation menu opened](../../.gitbook/assets/image%20%2875%29.png)

## !!Primary Screens 

!!include screenshots of the of the different primary screens in addition to the links!!

### Sources

The [Sources](../../configuring-the-data-integration-process/source-configuration/) Screen controls loading data from external systems into RAP, assesses quality via Validation Rules, and transforms data using Enrichment Rules. Additionally, it contains the Inputs tab to track the progress of individual file processing and enable restart of any failed or mis-configured processing tasks. Lastly, it contains the Data Viewer tab, allowing users to view and query the data for this Source stored within the Data Hub.

![](../../.gitbook/assets/close-up-logical-data-flow.png)

When we think of the logical data flow, we can consider the Source Screen controls to be the manager from raw data ingestion all the way to output. Further description of how Sources works with the logical data flow can be seen [here](navigation-and-interface.md#sources-details-the-logical-data-flow).

### Process

The Process screen shows the progress and outcome of all execution tasks through the platform, including sub-processes contained within the four standard processing steps of Input, Staging, Validation and Enrichment and Output.

### Outputs

The [Outputs](../../configuring-the-data-integration-process/output-configuration/) screen controls loading data from the Data Hub to the Data Warehouse layers. Source-target mapping, logs, and other output details can be viewed here, as well as historical outputs.

### Connections

The [Connections ](../../configuring-the-data-integration-process/connections-configuration.md)screen controls the connections to the External Source Systems and Data Warehouses. Connections can be used for both Sources and Outputs.

### Validation and Enrichment Templates

The [Validation and Enrichment Templates](../../configuring-the-data-integration-process/validation-and-enrichment-rule-templates.md) screen shows all of the available re-usable templates for creating similar Validation Rules and Enrichment Rules to multiple Sources.

### Source Dashboard

The Source Dashboard screen provides an over-time view of the outcome of each sources end-to-end processing.

## Sources Details - The Logical Data Flow

Sources can be thought of as managing the entire logical data flow. Sources work with connections and other sections of RAP to complete this flow. 

![Tab navigation within a RAP Source](../../.gitbook/assets/sources-header.png)

Within Sources there are seven tabs: Settings, Dependencies, Relations, Enrichments, Inputs, Process, and Data View. In depth information of the Sources location is found [here](../../configuring-the-data-integration-process/source-configuration/), what is provided here is an overview of how the Logical Data Flow seen in the [How it Works](how-it-works-2.md#the-data-flow) section is applied to the Sources interface.

### Input

The Input step moves data into RAP's Data Lake. The settings of a source requires a connection. We utilize the **Connections** interface of RAP to specify this input location, and then select this connection where prompted.

### Staging

Staging reads data from the Data Lake into RAP's internal Data Hub. This step occurs in the settings tab of a source, near the same UI as where the input parameter is entered.

### Validation and Enrichment

Validation and Enrichment occurs within the Data Hub. Validation and Enrichment occurs in several locations of the sources interface. The **Inputs** tab provides a check on if the data is successfully uploaded into RAP. The **Relations** tab allows for rules similar to merging to take place with the data. The **Enrichments** tab allows for different data manipulations and calculations to occur. Finally the **Data Viewer** allows for a visual of the data in tabular format.

### Output

The Output step maps the transformed data from the Data Hub to a Data Warehouse or 3rd party location. Again the **Connections** interface of RAP is used to set up the output location and then within the source settings the output is specified.





