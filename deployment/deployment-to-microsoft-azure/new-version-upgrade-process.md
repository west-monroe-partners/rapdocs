# !! New Version Upgrade Process

**Goal:** This document details the steps to upgrade Azure-based Intellio DataOps deployments to a new version.

**Process:**

1\) Confirm that no Intellio DataOps processes are in progress before starting the deployment. Run the query below on the PostgreSQL metastore and ensure that it returns zero results before proceeding with step \#2.

```text
SELECT * FROM meta.process
```

2\) If the DataOps storage account restricts public access \(does not Allow access from All Networks\), temporarily change the networking settings on the storage account to Allow access from All Networks and Save.

![](../../.gitbook/assets/image%20%28309%29%20%282%29%20%285%29.png)

3\)  Terraform Cloud, navigate to the appropriate workspace and then click "Variables".

![](../../.gitbook/assets/image%20%28313%29%20%281%29.png)

4\) Update the "imageVersion" variable with the new version of Intellio DataOps. Queue the Terraform plan, providing a "Reason for queueing plan". 

![](../../.gitbook/assets/image%20%28316%29%20%281%29.png)

5\) The plan should immediately launch, wait for the plan to finish. If the plan succeeds and the proposed resources changes align with expectations, confirm the plan to launch the Apply phase.

* If the Plan or Apply phases return error messages, please engage with the West Monroe team to troubleshoot.

![](../../.gitbook/assets/image%20%28312%29%20%281%29.png)

6\) After the Apply phase runs successfully, navigate to the Deployment container logs and confirm the final message is:

```text
INFO  Azure.AzureDeployActor - Deployment complete
```

7\) Navigate to the Intellio DataOps UI and confirm that the hover-over on the Menu tab indicates the newest version of the software.

![](../../.gitbook/assets/image%20%28325%29%20%281%29%20%281%29.png)







