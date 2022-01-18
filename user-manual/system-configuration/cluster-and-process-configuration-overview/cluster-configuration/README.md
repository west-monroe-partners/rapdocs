---
description: >-
  Cluster configurations allow users to select specific settings for a cluster.
  Several sources can then be linked to a cluster.
---

# Cluster Configuration

## Clusters List

Cluster configuration was previously found under the Source settings tab within the parameter table (<2.5.0). It has now moved into its own page which is accessed from the main menu; simply click on **System Configurations** and select **Cluster Configurations**.

![System Configurations on the main menu](../../../../.gitbook/assets/cluster\_001.png) ![Select Cluster Configurations](../../../../.gitbook/assets/cluster\_002.png)

The cluster configurations table shows all of the major details on any existing clusters. Users can filter by cluster names and descriptions as well as sort by column value.&#x20;

If a cluster has an associated job ID, users can view the job details or start a new job run. To start a new job run, click on the **launch icon** under the **Start** column. The view job details, click on the **ID** number under the **Job ID** column.&#x20;

Clicking any other column will direct users to the settings page of the cluster. To make a new cluster, hit the **NEW CLUSTER** button in the top right corner.

![Cluster configurations listed](../../../../.gitbook/assets/cluster\_003.png)

## Settings

The cluster settings page allows users to create and update cluster configurations for their sources. The example below shows the default settings for a new cluster configuration.

![New cluster configuration settings page](../../../../.gitbook/assets/cluster\_004.png)

* **Name\*:** A unique name.
* **Description\*:** A one sentence summary describing the cluster.
* **Default Cluster Configuration:** A flag that marks the cluster as the default. Once active, toggle is disabled until another default cluster is selected.
* **Cluster Type\*:** Create either a new Job or a new Job from sparky-pool (default) or user specified pool.
* **Scale Mode\*:** The number of workers can be automatically managed by Databricks or can be a fixed value.
* **Job Task Type\*:** Jobs will either execute a custom notebook in Databricks or Intellio Sparky Jar will be used.
* **Notebook Path\*:** The full file path to the custom notebook. Only required when custom notebook job task type is selected.

{% hint style="info" %}
Selecting DataOps jar as Job Task Type will use the Intellio Sparky Jar for all process types except custom.
{% endhint %}

## Advanced Parameters

Depending on the selections made in the required parameters section, the advanced parameters section will provide various sub-settings to help configurators tune jobs to their needs.&#x20;

Descriptions for each are included in the UI.  Please refer to the Databricks [documentation ](https://docs.databricks.com/dev-tools/api/2.0/jobs.html#newcluster)for in-depth details. Please submit a feature request support ticket if the descriptions in the UI do not adequately explain the functionality of a specific parameter.

{% hint style="info" %}
Any user modified parameters will be displayed as bold
{% endhint %}

