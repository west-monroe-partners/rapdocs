# AWS: Migrate Legacy Databricks to E2 Workspace

### Sign up for new Databricks account

All new accounts are created on the E2 platform. Sign up using your service account (might have to use a new email if it's already used on the legacy workspace) here [https://databricks.com/try-databricks?itm\_data=Homepage-HeroCTA-Trial](https://databricks.com/try-databricks?itm\_data=Homepage-HeroCTA-Trial)

### Update Terraform Variables

4 variables will need to be added to connect to the new Databricks E2 account

| Key                       | Description                                                                                                                                            | Example Value                       |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------- |
| databricksE2Enabled       | yes/no, enables E2 resources to be created                                                                                                             | yes                                 |
| databricksAccountId       | Main account id, found by going to [https://accounts.cloud.databricks.com/workspaces](https://accounts.cloud.databricks.com/workspaces) and logging in | 638396f1-xxxx-xxxx-9aab-ddf61xxxxxx |
| databricksAccountUser     | Email used to sign up for the E2 account                                                                                                               | databricksmaster@westmonroe.com     |
| databricksAccountPassword | Password used to sign up for the E2 account (mark as sensitive)                                                                                        | password123                         |

### Run Terraform

### Turn off environment and reach out to Databricks representative

### Use migration tool

### Turn on environment
