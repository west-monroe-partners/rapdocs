# Upgrading to Major Version 2.5.0

### Deployment Requirements

* AWS
  * Databricks workspace must be on E2 architecture. If the Databricks workspace is not on E2, follow this guide to migrate to E2 before the 2.5.0 deployment is ran: [aws-migrate-legacy-databricks-to-e2-workspace.md](aws-migrate-legacy-databricks-to-e2-workspace.md "mention"). If you are unsure that if your Databricks workspace is on E2 or not, check Terraform variables for existence of a variable called "databricksAccountId". If it is not defined, then your workspace is not on E2.
  * Infrastructure is updated using "master-2.4.3" branch from Intellio infrastructure repository in Github. There is a Databricks provider upgrade that needs to be applied so rollback to 2.4.3 can be possible.

### Terraform Variable Additions

| Variable         | Description                                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------------------------ |
| usageAuth0Secret | Must acquire from West Monroe deployment resource. Secret that allows Usage Agent to call to West Monroe API |
| usagePassword    | Password for usage user in Postgres database                                                                 |

### Post Deployment

When deployment finishes and the API is up and running each new Cluster Configuration in the environment will need to be opened and saved, so that a Job is added in Databricks and Job ID is generated

* Azure
  * A script to change storage location paths will need to be ran in Databricks. The script will be auto-generated in a notebook in the Shared location. Make sure this script is ran before any processing is done in the environment or there will be failures in Enrichment and Refresh.

### Rollback to 2.4.3

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
