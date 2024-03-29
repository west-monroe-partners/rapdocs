# Upgrading to Major Version 2.5.0

{% hint style="warning" %}
This guide assumes environments are operating on version 2.4.3
{% endhint %}

### Deployment Requirements

{% hint style="danger" %}
This release will not support import/export between 2.4.3 and 2.5.0. Please take this into consideration when upgrading environments to 2.5.0.
{% endhint %}

* AWS
  * Databricks workspace must be on E2 architecture. If the Databricks workspace is not on E2, follow this guide to migrate to E2 before the 2.5.0 deployment is ran: [aws-migrate-legacy-databricks-to-e2-workspace.md](aws-migrate-legacy-databricks-to-e2-workspace.md "mention"). If you are unsure that if your Databricks workspace is on E2 or not, check Terraform variables for existence of a variable called "databricksAccountId". If it is not defined, then your workspace is not on E2.
  * For private facing environments, the UI will now be deployed on a ECS container instead of S3 static website bucket. The ECS container will be attached to the ALB that currently serves the API via a new target group - similar to how the API container is currently set up. There may be a need to edit the security group that is attached to the ALB to allow VPN or RDP traffic to the ALB. Please reach out to West Monroe infrastructure team for guidance on updating networking for the new UI container.
  * Infrastructure is updated using "master-2.4.3" branch from Intellio infrastructure repository in Github. There is a Databricks provider upgrade that needs to be applied so rollback to 2.4.3 can be possible.

### Terraform Variable Additions

| Variable         | Description                                                                                                             |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------- |
| usageAuth0Secret | Must acquire from West Monroe deployment resource. Secret that allows Usage Agent to call to West Monroe API (REQUIRED) |
| usagePassword    | Password for usage user in Postgres database (REQUIRED)                                                                 |

Optional variables have been added to provide more flexibility with networking in AWS. These variables are: existingVpcId, existingInternetGatewayId, existingNATGatewayId, existingPublicRouteTableId, existingPrivateRouteTableId, existingWebAZ1Id, existingWebAZ2Id, existingAppAZ1Id, existingAppAZ2Id, existingDbAZ1Id, existingDBAZ2Id, existingDatabricksAZ1Id, existingDatabricksAZ2Id. Entering the AWS resource ID for the specific resource into the matching variable will have Terraform reference the existing VPC, gateway, route table, or subnet. For more info, please examine the variable.tf file in aws/main-deployment/

Optional variables have been added to provide more flexibility with container CPU and memory in AWS and Azure. These variables are: apiCPU, apiMemory, coreCPU, coreMemory, agentCPU, agentMemory. Adding these variables will override the default container instance sizing that Terraform uses. For more info, please examine the variable.tf file in aws/main-deployment/ or azure/

### Post Deployment

For AWS, you may need to run the Deployment service again if it fails from running before the Terraform apply finishes. Update the service to 1 task desired, or stop the deployment task if it's still up and another will start.

When deployment finishes and the API is up and running each new Cluster Configuration in the environment will need to be opened and saved, so that a Job is added in Databricks and Job ID is generated.

{% hint style="warning" %}
Make sure each cluster configuration has been opened and saved before running any jobs in IDO. If the cluster configuration does not have a Job Id on the cluster configurations page, **all processes that try to run using it will fail**.
{% endhint %}

If a custom cluster configuration was migrated and it contained cluster specific elements like spark\_conf or libraries, you will see an ACTION REQUIRED message in the cluster configuration name. Please look at the description to find the elements that you will need to add to the new cluster configuration.

* Azure
  * A script to change storage location paths will need to be ran in Databricks. The script will be auto-generated in a notebook in the Shared location. Make sure this script is ran before any processing is done in the environment or there will be failures in Enrichment and Refresh.
  * Any delta lake output tables that were created prior to 2.5.0 will need to be dropped and recreated in IDO. If your environment was using delta lake output, please follow the following steps
    * Navigate to the Key Vault in the IDO resource resource group, access the private secret, and copy the value at key "azure-storage-account-key".
    * Navigate to Databricks and create a new cluster or alter an existing cluster, add to Spark config the key "fs.azure.account.key.\<datalake-account-name>.blob.core.windows.net" and value the value from the previous step. For example, if your environment was "prod" and client name "ido", then the config to add would look like ![](<../../../.gitbook/assets/image (380) (1) (1) (1).png>)
    * Create a notebook in Databricks and attach the cluster with the config key, then drop the delta lake table(s) that was created prior to 2.5.0.
    * Once the delta lake tables are cleared out, edit/delete the cluster to remove the config key from the environment
    * Go to IDO and rerun output on any affected source to recreate the delta lake tables.

## Migrating legacy custom cluster configurations in version 2.5

All source using custom cluster configurations will need to to be manually migrated after 2.5 release deployment. Migration steps:

1. Open source setttings, scroll down to Parameters->Performance & Cost->Custom Cluster Params and save the json value
2. &#x20;Using saved json config and databricks' [documentation for custom job & cluster configuration](https://docs.databricks.com/dev-tools/api/latest/jobs.html) as a reference, create new custom cluster configuration in UI by going to top level menu and clicking Cluster Configurations:

![](<../../../.gitbook/assets/image (375).png>)

3\. After validating and saving cluster configuration, create Process Configuration with the new Cluster Configuration as default cluster

4\. Go back to the original source setting screen and update process configuration with the one you  created in step 3:

![](<../../../.gitbook/assets/image (374).png>)

5\. Save the source settings

## Rollback to 2.4.3

{% hint style="danger" %}
If any sources process on 2.5.0 and a rollback to 2.4.3 is done, then the source will no longer work on 2.4.3 and will need source data cleared and new inputs pulled in
{% endhint %}

Revert the GitHub pull request that was made to upgrade infrastructure to 2.5.0

Restore Postgres database to point in time before deployment was done

* Azure
  * Restore name should be the original database name with -restore at the end, i.e. prod-database-ido-restore
  * Once restore finishes, wait 10 minutes to get a restore point on the restored database, and delete the original database
  * Restore restored database into a new database with the original database name, i.e. prod-database-ido
* AWS
  * Restore name should be the original database name with -restore at the end, i.e. prod-database-ido-restore
  * Delete the original database
  * Rename restored database to the original database name, i.e. prod-database-ido

Change imageVersion variable in Terraform to 2.4.3

Plan and apply 2.4.3 infrastructure in Terraform

Once apply runs, make sure Deployment container runs successfully and that 2.4.3 is displayed in the UI

* Azure
  * Rerun storage location script from the post deployment step but flip paths around&#x20;
