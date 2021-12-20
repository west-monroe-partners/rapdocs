---
description: >-
  Cluster configurations allow users to select specific settings for a cluster.
  Several sources can then be linked to a cluster.
---

# Cluster Configuration

## Clusters List

Cluster configuration was previously found under the Source settings tab within the parameter table (<2.5.0). It has now moved into its own page which is accessed from the main menu; simply click on **System Configurations** and select **Cluster Configurations**.

![](../../../../.gitbook/assets/cluster\_001.png) ![](../../../../.gitbook/assets/cluster\_002.png)

The cluster configurations table shows all of the major details on any existing clusters. Users can filter by cluster names and descriptions as well as sort by column value.&#x20;

If a cluster has an associated job ID, users can view the job details or start a new job run. To start a new job run, click on the **launch icon** under the **Start** column. The view job details, click on the **ID** number under the **Job ID** column.&#x20;

Click any other column will direct users to the settings page of the cluster. To make a new cluster, hit the **NEW CLUSTER** button in the top right corner.

![](../../../../.gitbook/assets/cluster\_003.png)

## Settings

The cluster settings page allows users to create and update cluster configurations for their sources. The example below shows the default settings for a new cluster configuration.

![](../../../../.gitbook/assets/cluster\_004.png)

* **Name\*:** A unique name.
* **Description\*:** A one sentence summary describing the cluster.
* **Default Cluster Configuration:** A flag that marks the cluster as the default. Once active, toggle is disabled until another default cluster is selected.
* **Cluster Type\*:** Create either a new Job or a new Job from the pool.
* **Scale Mode\*:** The number of works is automatically managed by DataBricks or is a fixed value.
* **Job Task Type\*:** Jobs with either execute a custom notebook or Intellio sparky will be used.
* **Notebook Path \*:** The fill file path to the custom notebook. Only required when custom notebook job task type is selected.

{% hint style="info" %}
The DataOPs jar will use sparky for all process types except custom
{% endhint %}

## Advanced Parameters

Depending on the selections made in the required parameters section, the advanced parameters section will provide various sub-settings to help configurators tune the jobs to their needs.&#x20;

Descriptions for each are included in the UI. Please submit a support ticket if the descriptions in the UI do not adequately explain the functionality of a specific parameter.

{% hint style="info" %}
Any user modified parameters will be marked with a bold font weight.
{% endhint %}

## Add a Cluster to a Source

Clusters can now be added to a source by selecting the **Custom** connection type on the [**Source Settings**](../../../source-configuration/source-details.md) **** page.

![](../../../../.gitbook/assets/cluster\_005.png)

After the **Custom** connection type is selected, a **Cluster Config** dropdown will appear. Simply click on the dropdown to select an existing cluster configuration for that source.

Users can also create a new cluster configuration by click on the _open in new_ icon. This will open a separate tab to a new cluster configuration. After creating the new cluster, users have the option to refresh the cluster list on the [**Source Settings**](../../../source-configuration/source-details.md) page. Refreshing the list will show their newly created cluster configuration

Users may also edit an existing cluster by selecting a cluster configuration from the dropdown and then clicking the _edit_ icon. This will open a new tab to the existing cluster. After edits are saved, users are once again prompted to refresh the cluster list.
