---
description: '******THIS IS NOT COMPLETE OR READY FOR CLIENT USE*****************'
---

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

When the VCS connection is created, set the working directory to "terraform/aws". The VCS branch can be the default branch, as it generally defaults to master.

{% hint style="info" %}
Make sure that the Terraform version in the workspace is set to "0.12.29"
{% endhint %}

## Populating Variables in Terraform Cloud

Manually enter the following variable names and set values accordingly. If predeployment steps were followed, these values should be mostly known. 

In Databricks, click user dropdown in the top right of the Databricks portal. Click "User Settings" in the dropdown. On the Access Tokens tab, click "Generate New Token". Set the token lifetime to blank, so the token will not expire. Use the token in the "databricksToken" variable here.

{% hint style="danger" %}
The variable names are case sensitive - please enter them as they appear in this list
{% endhint %}

| Variable | Example | Description |
| :--- | :--- | :--- |
| awsRegion | us-west-2 | AWS Region for deployment - As of 11/9/2020 us-west-1 is not supported |
| awsAccessKey | AKIAXXXXXXXX | Access Key for Master Deployment IAM user  - mark as sensitive |
| awsSecretKey | fdldsjfs8f34dfsdf344334\*\* | Secret Key for Master Deployment IAM user - mark as sensitive |
| environment | dev | This will be prepended to resources in the environment. E.g. Dev. Prod. etc.  |
| client | intellio | This will be postpended to resources in the environment - use company or organization name |
| vpcCidrBlock | 10.1 | Only the first two digits here, not the full CIDR block |
| avalibilityZoneA | us-west-2a | Not all regions have availability zones |
| avalibilityZoneB | us-west-2b | Not all regions have availability zones |
| RDSretentionperiod | 7 | Database backup retention period \(in days\) |
| RDSmasterusername | rap\_admin | Database master username |
| RDSmasterpassword | password123 | Database master password - mark sensitive |
| RDSport | 5432 | RDS port |
| TransitiontoAA | 60 | Transition to Standard-Infrequent Access |
| TransitiontoGLACIER | 360 | Transition to Amazon Glacier |
| stageUsername | stageuser | Database stage username for metastore access |
| stagePassword | password123 | Database stage password for metastore access - mark sensitive |
| coreImageName | 2.0.11 | Core application Docker image tag |
| agentImageName | 2.0.11 | Agent application Docker image tag |
| apiImageName | 2.0.11 | API application Docker image tag |
| deploymentImageName | 2.0.11 | Deployment application Docker image tag |
| dockerUsername | wmpintellio | DockerHub service account username |
| dockerPassword | xxxxx | DockerHub service account password |
| urlEnvPrefix | dev | Prefix for environment site url |
| baseUrl | intellioplatform | the base URL of the certificate - example [https://\(urlEnvPrefix\)\(baseUrl\).com](https://%28urlEnvPrefix%29%28baseUrl%29.com) This should not include www. .com or https://. e.g. "wmp-rap" |
| databricksToken | dapi10323SSXXXXXXX | Token from the Databricks environment - generate access token in Databricks and place here |
| usEast1CertURL | \*.intellioplatform.com | Full certificate name \(with wildcards\) used for SSL |
| auth0Domain | intellioplatform.auth0.com | Domain of Auth0 account |
| auth0ClientId | jdflsdfsdf | Client ID of API Explorer Application in Auth0 \(needs to be generated when account is created\) |
| auth0ClientSecret | faddfjXXXSssddff | Client Secret of API Explorer Application in Auth0 \(needs to be generated when account is created\) |

## Running Terraform Cloud

When the variables are configured, Terraform is ready to generate a plan. Click the "Queue plan" button and let Terraform do it's magic. If all the variables are correct, then the plan should have about 134 resources to add. If the plan finishes successfully, then click apply and let the deployment begin!

\*Add common terraform run hiccups here\*

## Post Terraform Steps

After the terraform is complete, there will be various resources created in the AWS account. These are the main resources created by Terraform, but there are still configuration changes that need to be done before any data can be brought into the platform. 

## Configuring Databricks

Log into the Databricks account that was created during Pre-Deployment steps.

Configure instance profile - [Official Databricks Documentation](https://docs.databricks.com/administration-guide/cloud-configurations/aws/instance-profiles.html). When configuring the instance profile, make sure that the authorized S3 bucket is "&lt;environment&gt;-datalake-&lt;client&gt;" ex: dev-datalake-intellio.

Click "Pools" and then "Create Pool". Create a pool called "sparky-pool" with the following configurations

![](../../.gitbook/assets/image%20%28287%29.png)

After the pool is created, save the value called "DatabricksInstancePoolId" in the Tags section of the configuration. This value will be used later when updating Secrets Manager.

In Databricks, navigate to the "Clusters" tab. Create a cluster named "rap-mini-sparky". Configure the cluster with the following configurations. Make sure the previously created instance profile is used when configuring.

![](../../.gitbook/assets/image%20%28288%29.png)

Navigate to the Databricks home screen and create a new notebook. On a command box, add this code snippet:

```text
val environment = ""
val client = ""

spark.sql("CREATE DATABASE " + environment.toLowerCase)
val df = spark.read.format("avro").load("s3a://"+environment.toLowerCase+"-datalake-"+client.toLowerCase+"/datatypes.avro")
df.write.saveAsTable("datatypes")
spark.sql("INSERT INTO datatypes SELECT decimal + 1, bigint + 1, string || '2', int +1, float +1, double +1, date + INTERVAL 1 DAY, timestamp + INTERVAL 1 HOUR, false, long + 1 FROM datatypes")
```

There are 2 variables at the top that will need to be updated. Enter the environment and client values that we used in the Terraform variable step.

Navigate to the S3 bucket named "&lt;environment&gt;-datalake-&lt;client&gt;" Ex: dev-datalake-intellio

Place the following file in the bucket root-

{% embed url="https://s3.us-east-2.amazonaws.com/wmp.rap/datatypes.avro" %}

Once this file is uploaded, connect the workbook to the cluster that was created earlier, and run this snippet.

Test that the table is created by running

```text
spark.sql("select * from datatypes").show
```

This should not error out and should display the table data.

Capture a value out of the Databricks URL before moving on to the Secret Manager step. If the URL is [https://xxxxxxxxxxxx.cloud.databricks.com/?o=4433553810974403\#/setting/clusters/1102-212023-gauge891/configuration  ](https://xxxxxxxxxxxx.cloud.databricks.com/?o=4433553810974403#/setting/clusters/1102-212023-gauge891/configuration%20)then you would want to capture the "4433553810974403" portion after the /?o= for use when updating the Secret Manager secrets.

## Updating Secrets Manager

\*\*\*\*\*\*\*next to update

There are two Key Vaults that will need updated before the containers can run properly. The keys in the JSON will all exist, but some of the values will need to be updated/replaced.

The first secret that will need updating is the "&lt;environment&gt;-public-system-configuration" secret. It is recommended to copy the JSON value of the secret into a text editor such as [Notepad++](https://notepad-plus-plus.org/downloads/) for easy updating.

Public system configuration values that need updates:

* databricks-jdbc-uri
  * replace the number after/protocolv1/o/ with the number we saved from the Databricks URL in the last step of the Databricks post deployment instructions
* databricks-instance-pool-id
  * replace with value of sparky-pool ID that we saved in Databricks config step

The second secret that will need updating is the "&lt;environment&gt;-private-secret-configuration" secret.

Private system configuration values that need updates:

* databricks-token
  * replace with value saved from generating the access token in Databricks config step
* databricks-spark-conf
  * replace with account key from storage account &lt;environment&gt;storage&lt;client&gt;
  * Example value: {"fs.azure.account.key.devstorageintellio.blob.core.windows.net": "qTNCgxxxxxxxxxxxxxxxxxxxxxxxxxxxUuSk8x7LwAWXbKFiA2xxxxxxxxxxxxz0H5t0uZxxxxxiL5cEy8kag=="}

When the JSONs are updated and ready to go back into Key Vault, just create a new version of the secret with the updated JSON values. 

## Running Deployment Container

Navigate to the Container instance named &lt;environment&gt;-Deployment-&lt;client&gt;. Ex: Dev-Deployment-Intellio. Click "Containers" on the left blade menu and then click "Logs". Check to see if the container has ran successfully. There should be a final message in the logs that says

```text
 INFO  Azure.AzureDeployActor - Deployment complete
```

If this message exists - the first time deployment is good to go.

If this message does not exist - try running the container again \(click stop and start on the container\) and troubleshoot from there. 

\*Add common deployment container errors here\*

## Configuring Postgres System Configuration Table

Use the "database-connection" value from the Private secret in Key Vault to connect to the Azure Database for PostgreSQL server that is installed in the Resource Group. It will be named &lt;environment&gt;-db-&lt;client&gt;, Ex: dev-db-intellio. You may need to add the IP that you're connecting from to the Connection Security on the database configuration window. We recommend using a tool like PgAdmin or DataGrip to connect to the server.

Run the following queries in the database named &lt;environment&gt;. 

{% hint style="info" %}
Replace the "DEV" values with the name of your environment. Make sure that the databricks-db-name is all lowercase.
{% endhint %}

```text

update meta.system_configuration set value = 'DEV' where name = 'environment';
update meta.system_configuration set value = 'dev' where name = 'databricks-db-name';
update meta.system_configuration set value = 'Databricks' where name = 'spark-provider';
update meta.system_configuration set value = 'Azure' where name = 'cloud';
insert into meta.agent values ('local','local',null,null,
                               '{"default": true, "autoUpdate": false,
                                "maxResources": 4, "akkaStreamTimeout": 300,
                                 "checkDeltaInterval": 30,
                                  "checkPushFilesInterval": 10}','startxx',false);
```



## Configuring Custom Endpoint

Navigate to the Frontend Endpoint resource called &lt;environment&gt;-FrontendEndpoint-&lt;client&gt;, Ex: Dev-FrontendEndpoint-Intellio. Click "Custom domain" in the overview screen. In the "Custom hostname" box, enter the DNS name of the Intellio site that is being deployed. This will generally be: &lt;environment&gt;-&lt;dnsZone&gt;. dnsZone was a variable that was set when the Terraform variables were populated. The custom hostname will need to be DNS resolvable before it can be added.

After adding the custom hostname, click on the custom hostname to configure the domain further. The configuration should then look similar to the following image, with the deployment specific values replaced.

![](../../.gitbook/assets/image%20%28278%29.png)

{% hint style="warning" %}
Make sure the Azure CDN step is followed so that CDN can access the Key Vault where the secret lives
{% endhint %}

Save the configuration and this step will be complete.

## Restart Everything!

At this point, all the post Terraform configuration should be good to go. There are three container instances that should be restarted now - Core, Agent, and Api. Navigate to each of the containers, click stop on them, and then click start once they're fully stopped. We recommend starting them in the following order -

1. Api
2. Core
3. Agent

Check the container logs to ensure the containers have started and are running with no errors. Once all three containers are running, it's time to go on the site!

## Check Application Gateway Health probe

Navigate to the Application Gateway and click "Health probes" in the left menu blade. There should be one record with the name &lt;environment&gt;-HealthProbe-&lt;client&gt;, EX: Dev-HealthProbe-Intellio. Make sure that the "Host" value is set to the IP of the currently running Api Container instance. If the values are different, change the Host value on the Health Probe to equal the IP address of the currently running Api container instance. The IP Address is in the overview of the container instance like so:

![](../../.gitbook/assets/image%20%28277%29.png)

Test the backend health before adding the health probe, then Save the update to the health probe.

## Auth0 Rule Updates

In the Auth0 Dashboard there is a section on the left hand menu called "Rules". Edit the "Email domain whitelist" rule to add domains that should be able to sign up to the Intellio Frontend. By default, the rule is generated with only the WMP emails.

![](../../.gitbook/assets/image%20%28277%29%20%281%29.png)

## Smoke Testing

Time to drop a file in!

