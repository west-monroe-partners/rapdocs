# Performing the Deployment

## Creating Deployment AWS User for Terraform

Create an Master Deployment IAM user that has Console Admin access - this will be the user that runs the Terraform scripts. Use the Access Key and Secret Key from the user in the Terraform Cloud variables.

## Fork Infrastructure Repository in GitHub

Work with DataOps team to get a service GitHub account added - this will be used to fork the main Infrastructure repository into the service account.

Follow this guide to help setup the connections: [https://www.terraform.io/docs/cloud/vcs/github.html](https://www.terraform.io/docs/cloud/vcs/github.html)

{% hint style="info" %}
When the connection is made between Terraform Cloud and the forked repo, make sure that a request is sent from the forked repo to the main DataOps repo to allow the connection.
{% endhint %}

## Setting up Terraform Cloud Workspace

Create a new workspace in Terraform Cloud. Choose "Version control workflow". Configure the VCS connection to the forked repository in GitHub. Follow the Terraform Cloud steps when configuring a new VCS connection.

When the VCS connection is created, set the working directory to "terraform/aws/main-deployment". The VCS branch can be the default branch, as it generally defaults to master.

{% hint style="info" %}
Make sure that the Terraform version in the workspace is set to "0.14.11"
{% endhint %}

## Populating Variables in Terraform Cloud

Manually enter the following variable names and set values accordingly. If the pre-deployment steps were followed, these values should be mostly known. 

{% hint style="danger" %}
The variable names are case sensitive - please enter them as they appear in this list
{% endhint %}

| Variable | Example | Description |
| :--- | :--- | :--- |
| awsRegion | us-west-2 | AWS Region for deployment - As of 11/9/2020 us-west-1 is not supported |
| awsAccessKey | AKIAXXXXXXXX | Access Key for Master Deployment IAM user  - mark as sensitive |
| awsSecretKey | fdldsjfs8f34dfsdf344334\*\* | Secret Key for Master Deployment IAM user - mark as sensitive |
| publicFacing | yes | yes/no - controls public vs private facing architecture |
| environment | dev | This will be prepended to resources in the environment. E.g. Dev. Prod. etc.  |
| client | intellio | This will be postpended to resources in the environment - use company or organization name |
| vpcCidrBlock | 10.1 | Only the first two digits here, not the full CIDR block |
| availabilityZoneA | us-west-2a | Not all regions have availability zones |
| availabilityZoneB | us-west-2b | Not all regions have availability zones |
| RDSmasterusername | admin | Database master username |
| RDSmasterpassword | password123 | Database master password - mark sensitive |
| stageUsername | stageuser | Database stage username for metastore access |
| stagePassword | password123 | Database stage password for metastore access - mark sensitive |
| imageVersion | 2.3.2 | Deployment version for the platform |
| dockerUsername | wmpintellio | DockerHub service account username |
| dockerPassword | xxxxx | DockerHub service account password |
| urlEnvPrefix | dev | Prefix for environment site url |
| baseUrl | intellioplatform | the base URL of the certificate - example [https://\(urlEnvPrefix\)\(baseUrl\).com](https://%28urlEnvPrefix%29%28baseUrl%29.com) This should not include www. .com or https://. e.g. "wmp" |
| usEast1CertURL | \*.intellioplatform.com | Full certificate name \(with wildcards\) used for SSL |
| auth0Domain | intellioplatform.auth0.com | Domain of Auth0 account |
| auth0ClientId | jdflsdfsdf | Client ID of API Explorer Application in Auth0 \(needs to be generated when account is created\) |
| auth0ClientSecret | faddfjXXXSssddff | Client Secret of API Explorer Application in Auth0 \(needs to be generated when account is created\) |
| databricksE2Enabled | yes | Is Databricks E2 architecture being used in this environment? |
| databricksAccountId | 638396f1-xxxx-xxxx-xxxx-ddf61adc4b06 | Account ID for Databricks E2 |
| databricksAccountUser | user@wmp.com | Username for main E2 account user |
| databricksAccountPassword | xxxxxxxxx | Password for main E2 account user |

There are some advanced variables that can be added as well - please refer to the variables file in the infrastructure repository to see a list of all variables.

## Running Terraform Cloud

When the variables are configured, Terraform is ready to generate a plan. Click the "Queue plan" button and let Terraform do it's magic. If all the variables are correct, then the plan should have about 134 resources to add. If the plan finishes successfully, then click apply and let the deployment begin!

## Post Terraform Steps

After the terraform is complete, there will be various resources created in the AWS account. These are the main resources created by Terraform, but there are still configuration changes that need to be done before any data can be brought into the platform. 

## Configuring Databricks

Log into the Databricks account that was created during the Terraform deploy. The account URL is an output variable from the Terraform apply.

Navigate to the S3 bucket named "&lt;environment&gt;-datalake-&lt;client&gt;" Ex: dev-datalake-intellio

Place the following file in the bucket root-

{% embed url="https://s3.us-east-2.amazonaws.com/wmp.rap/datatypes.avro" %}

Once this file is uploaded, connect the workbook called "databricks-init" and run the workbook. Attach to the "dataops-init-cluster"

If the workbook runs successfully, move on to the next step! 

## Running Deployment Container

Navigate to the Container instance named &lt;environment&gt;-Deployment-&lt;client&gt;. Ex: Dev-Deployment-Intellio. Click "Containers" on the left blade menu and then click "Logs". Check to see if the container has ran successfully. There should be a final message in the logs that says

```text
 INFO  Azure.AzureDeployActor - Deployment complete
```

If this message exists - the first time deployment is good to go.

If this message does not exist - try running the container again \(click stop and start on the container\) and troubleshoot from there. 

## Configuring Postgres System Configuration Table

Use the "database-connection" value from the Private secret in Key Vault to connect to the RDS in the RDS service within AWS.

Run the following queries in the database named &lt;environment&gt;. Update the region parameter with the AWS region that DataOps is deployed in.

{% hint style="info" %}
Replace the "DEV" values with the name of your environment. Make sure that the databricks-db-name is all lowercase.
{% endhint %}

```text

update meta.system_configuration set value = 'DEV' where name = 'environment';
update meta.system_configuration set value = 'dev' where name = 'databricks-db-name';
update meta.system_configuration set value = 'Databricks' where name = 'spark-provider';
update meta.system_configuration set value = 'AWS' where name = 'cloud';
```

## Restart Everything!

At this point, all the post Terraform configuration should be good to go. There are three container instances that should be restarted now - Core, Agent, and Api. Navigate to each of the containers, click stop on them, and then click start once they're fully stopped. We recommend starting them in the following order -

1. Api
2. Core
3. Agent

Check the container logs to ensure the containers have started and are running with no errors. Once all three containers are running, it's time to go on the site!

## Auth0 Rule Updates

In the Auth0 Dashboard there is a section on the left hand menu called "Rules". Edit the "Email domain whitelist" rule to add domains that should be able to sign up to the Intellio Frontend. By default, the rule is generated with only the WMP emails.

![](../../../.gitbook/assets/image%20%28277%29%20%281%29.png)



