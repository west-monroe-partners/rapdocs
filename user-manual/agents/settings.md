# Settings

## Settings Tab

The Agent settings Tab allows a user to configure metadata that the Agent uses for system configuration. The parameters control heartbeat intervals, auto-updating, security, Agent concurrency, and plugin configurations.

After the Agent settings are saved, the settings can be accessed at any time through the Agent screen.&#x20;

![](<../../.gitbook/assets/image (353).png>)

## Base Parameters

{% hint style="info" %}
Asterisks (\*) mean the Parameter is mandatory and must be specified by users.
{% endhint %}

* **Name\*:** Name of the Agent. The name must be unique. It will be displayed on the Agents screen when browsing Agents, and will be shown on Connections that the Agent is connected to.
* **Description\*: **Description of the Agent
* **Code\*: **Agent code of the Agent. This will be the backend identifier of the Agent and is needed during MSI install. Recommended to be lowercase, alphanumeric, and with underscores instead of whitespace.
* **Active\*: **If set to Active, the Agent will trigger Ingestions that's it's configured to run
* **Default Agent\*: **If set to True, the Agent will be auto populated when configuring a new Connection.
* **Region\*: **AWS region for AWS cloud environments. Azure region for Azure cloud environments
* **Machine Guid\*: **Machine GUID for the host server that the Agent is being installed on. More information on finding Machine GUID can be found in the Installing a New Agent guide.
* **S3LandingPath\*: **Path to datalake bucket in AWS environments. In Azure environments, this parameter will not show up.
* **AutoUpdate: **If set to True, the on-premise Agent will auto-update when a new IDO version is deployed in the environment
* **MaxResources**: Controls how many Ingestion processes the Agent can run at once. Default is 4.
* **AkkaStreamTimeout: **JDBC Connection timeout in seconds
* **CheckDeltaInterval: **Main heartbeat of the Agent in seconds. The interval in which Agent configuration changes are checked, such as schedule, source query, and other applicable Ingestion parameters.
* **CheckPushFilesInterval: **Heartbeat for the file watcher. Every time this interval passes, all file watcher locations are checked for potential files to ingest.
* **Plugins: **JSON array of Plugin metadata for the Agent. Example of a value would be: \[{"type":"oracle","files":"s3://development-agentjar-wmp/oracle/"}]
