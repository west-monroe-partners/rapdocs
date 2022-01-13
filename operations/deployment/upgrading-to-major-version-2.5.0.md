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

Azure specific: A script to change storage location paths will need to be ran in Databricks. The script will be auto-generated in a notebook in the Shared location. Make sure this script is ran before any processing is done in the environment or there will be failures in Enrichment and Refresh.

### Rollback to 2.4.3

{% hint style="danger" %}
If any sources process on 2.5.0 and a rollback to 2.4.3 is done, then the source will no longer work on 2.4.3 and will need source data cleared and new inputs pulled in
{% endhint %}

