---
description: >-
  Process configurations allow users to create specific process configurations.
  Multiple sources can be linked to one process configuration.
---

# Process Configuration

## Process Configurations List

Process configuration was previously found under the Source Settings tab within the parameter table (<2.5.0). It has now moved into its own page which is accessed from the main menu; simply click on **System Configurations** and select **Process Configurations**.

![System Configurations on the main menu](../../../../.gitbook/assets/cluster\_001.png) ![Select Process Configurations](../../../../.gitbook/assets/process\_configs\_001.png)

The process configurations table displays all of the high level details on existing process configurations. Simply hover over and click on any listed process configuration to be taken to its settings page

To make a new process configuration, click on the **NEW PROCESS CONFIGURATION** button in the top right corner.

![Process Configurations list](../../../../.gitbook/assets/process\_configs\_002.png)

## Settings

The process settings page allows users to create and update process configurations for their sources. The example below shows the default settings for a new process configuration.

![New Process Configurations Settings page](../../../../.gitbook/assets/process\_configs\_003.png)

* **Name\*:** A unique name.
* **Description:** A one sentence summary describing the process configuration.
* **Cluster Config\*:** The [**Cluster Configuration**](../cluster-configuration/) that the process configuration will be using. Default is the currently flagged default cluster.
* **Default Process Configuration:** Flag that marks one process configuration as the default. In order to remove default flag, another process configuration must be flagged as default.

### Process Overrides&#x20;

Process overrides allow users to assign other cluster configurations for specific process types.&#x20;

To add an override, simply hit the **ADD OVERRIDE** button. Users will then select a process type from the dropdown.

{% hint style="info" %}
Multiple overrides can be assigned to one process configuration.
{% endhint %}

![Process override dropdown](../../../../.gitbook/assets/process\_configs\_004.png)

The default cluster configuration will be automatically selected for any new overrides. To select a different cluster configuration, simply click the dropdown and choose a different option.&#x20;

****[**Cluster Configurations**](../cluster-configuration/) can be created and updated by clicking the _open in new window_ or _edit_ icons next to the dropdown. This will open a new tab where users can edit/create cluster configurations. Once changes have been made, users will be prompted to refresh the cluster configuration dropdown.

## Adding a Process Configuration to a Source

Process configurations can be easily attached to sources via the [**Source Settings**](../../../source-configuration/source-details.md) tab. Simply navigate to a Source's settings page and locate the **Process Config** dropdown.

{% hint style="info" %}
The process config dropdown will automatically default to flagged default process configuration.
{% endhint %}

Users can also edit/create process configurations from the source settings tabs by clicking the _open in new window_ or _edit_ icons next to the dropdown.

![Selecting a Process Configuration from the Source Settings page](../../../../.gitbook/assets/process\_configs\_005.png)
