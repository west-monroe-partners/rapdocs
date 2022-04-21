# Cluster Configuration for Custom Processing Steps

Custom processing steps (Custom Ingestion, Custom Parse, Custom Post-Output) require user to select cluster configuration for each custom step. This custom cluster configuration defines cluster and job parameters and specifies the custom Databricks notebook that will be executed.



## Configure Cluster for Custom Ingestion Source

Sources configured to use Custom Ingestion (**Custom** connection type) on the [**Source Settings**](../../../source-configuration/source-details.md) **** page require user to select Cluster Configuration for the Custom Ingestion step:

![](<../../../../.gitbook/assets/image (390) (2).png>)

After the **Custom** connection type is selected, a **Custom Ingest** **Cluster Config** dropdown will appear. Simply click on the dropdown to select an existing cluster configuration for that source.

Users can also create a new cluster configuration by click on the _open in new_ icon. This will open a separate tab to a new cluster configuration. After creating the new cluster, users have the option to refresh the cluster list on the [**Source Settings**](../../../source-configuration/source-details.md) page. Refreshing the list will show their newly created cluster configuration

Users may also edit an existing cluster by selecting a cluster configuration from the dropdown and then clicking the _edit_ icon. This will open a new tab to the existing cluster. After edits are saved, users are once again prompted to refresh the cluster list.

## Configure Cluster for Custom Parse Source

Sources configured to use Custom Parse on the [**Source Settings**](../../../source-configuration/source-details.md) **** page require user to select a **Custom Parse Cluster Config** for the Custom Parse step:

![](<../../../../.gitbook/assets/image (381) (1) (1) (1).png>)



## Configure Cluster for Custom Post-Output

Sources configured to use Custom Post Output on the [**Output Settings**](../../../output-configuration/output-details.md) **** page require user to select a **Custom Post Output Cluster Configuration** for the Custom Post Output step:

![](<../../../../.gitbook/assets/image (385) (1) (1).png>)
