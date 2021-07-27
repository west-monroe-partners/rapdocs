# Hotfix Upgrade Process

**Goal:** This document details the steps to upgrade Azure-based Intellio DataOps deployments to a new hotfix version.

**Process:**

Confirm that no Intellio DataOps processes are in progress before starting the deployment. Run the query below on the PostgreSQL metastore and ensure that it returns zero results before proceeding with step \#2.

```text
SELECT * FROM meta.process
```

Navigate to the Deployment container in your DataOps resource group in Azure Portal.

![](../../../.gitbook/assets/image%20%28371%29.png)

Start the container by clicking the "Start" button in the Overview section. If the container is running from a previous deployment, click Stop and then Start - or Restart.

The Deployment container should start running shortly. Watch the logs until you see the following message.

```text
INFO  Azure.AzureDeployActor - Deployment complete
```

Navigate to the Intellio DataOps UI and confirm that the hover-over on the Menu tab indicates the newest version of the software.

![](../../../.gitbook/assets/image%20%28325%29%20%281%29%20%281%29.png)







