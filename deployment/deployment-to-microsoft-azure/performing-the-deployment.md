# Performing the Deployment

## Creating Deployment Principal for Terraform

Navigate to Azure Active Directory and select "App registrations" on the left menu blade. Create a new registration. Make the name something that is appropriate for the master deployment registration - ex: Master-Terraform.

Choose "Accounts in this organizational directory only" for supported account types.

After creating the App Registration, navigate to the API permissions section of the App registration. Click "Add a permission" and navigate to "Supported legacy APIs". Then, click "Application permissions" and then choose these two permissions:

* Application.ReadWrite.All
* Directory.ReadWrite.All

Add the permissions and grant them consent if needed. 

Save the Application \(client\) ID and generate a secret for the app registration. These will be used when populating variables in Terraform Cloud in the next step.

## Fork Infrastrucure Repository in GitHub

Work with DataOps team to get a service GitHub account added - this will be used to fork the main Infrastructure repository into the service account.

## Setting up Terraform Cloud Workspace

Create a new workspace in Terraform Cloud. Choose "Version control workflow". Configure the VCS connection to the forked repository in GitHub. Follow the Terraform Cloud steps when configuring a new VCS connection.

When the VCS connection is created, set the working directory to "terraform/azure". The VCS branch can be the default branch, as it generally defaults to master.

{% hint style="info" %}
Make sure that the Terraform version in the workspace is set to "0.12.29"
{% endhint %}

## Populating Variables in Terraform Cloud

Manually enter the following variable names and set values accordingly. If predeployment steps were followed, these values should be mostly known. 

{% hint style="danger" %}
The variable names are case sensitive - please enter them as they appear in this list
{% endhint %}

| variable  | example  | description  |
| :--- | :--- | :--- |
| vpcCidrBlock  | 10.0  | Enter the first two digits for the VPC’s /16 CDIR block. Example: \`10.1\`  |
| dockerUsername  | intellio  | Docker username for account that will have access to WMPDockerhub  |
| dockerPassword  | &lt;password&gt;  | Password for above account  |
| RDSmasterpassword  | &lt;password&gt;  | Administrative Password for the RDS Postgres Database. Use any printable ASCII character except /, double quotes, or @.  |
| auth0ClientId  | 384u3kddxj112j3  | Client Id of Auth0 account’s Management API application  |
| environment  | Dev | The environment to be deployed. This is prepended to all resource names Ex: Dev  |
| databricksToken  |  | Populate this and reapply once the first deploy finishes and Databricks is configured.  |
| auth0ClientSecret  | s09df098ds0f8s0d8f0sd98f0s  | Client secret of Auth0 account’s Management API application  |
| auth0Domain  | intellio.auth0.com  | Domain of the Auth0 account  |
| client  | Intellio  | Client name. This is postpended to all resource names. Ex: WMP  |
| clientSecret  | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure client secret from user/app authenticating deploy - This comes from step one, the deployment principal |
| clientId  | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure client ID from user/app authenticating deploy - This comes from step one, the deployment principal |
| dnsZone  | dev.intellio.com  | Base URL for the wildcard cert  |
| region  | East US  | Azure region to deploy the environment to  |
| tenantId  | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure tenant ID  |
| subscriptionId  | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure Subscription ID  |
| cert  |  | Contents of the SSL certificate  |
| imageVersion  | 2.x.x | Deployment version for the platform  |

## Running Terraform Cloud

When the variables are configured, Terraform is ready to generate a plan. Click the "Queue plan" button and let Terraform do it's magic. If all the variables are correct, then the plan should have about 74 resources to add. If the plan finishes successfully, then click apply and let the deployment begin!

\*Add common terraform run hiccups here\*

## Post Terraform Steps

After the terraform is complete, there will be a resource group now created in Azure Portal. These are the main resources created by Terraform, but there are still configuration changes that need to be done before any data can be brought into the platform. 

## Configuring Databricks

In the Azure Portal Resource Group that's been created, navigate to the Databricks resource. Click "Launch Workspace" on the overview page.

In Databricks, navigate to the "Clusters" tab. Create a cluster named "rap-mini-sparky". Configure the cluster with the following configurations

![](../../.gitbook/assets/image%20%28276%29.png)

Click "Pools" and then "Create Pool". Create a pool called "sparky-pool" with the following configurations

![](../../.gitbook/assets/image%20%28275%29.png)

After the pool is created, save the value called "DatabricksInstancePoolId" in the Tags section of the configuration. This value will be used later when updating the Key Vault.

Navigate to the Databricks home screen and create a new notebook. On a command box, add this code snippet:

```text
val databricksPrincipalId = ""
val databricksPrincipalSecret = ""
val databricksPrincipalEndpoint = ""
val environment = ""
val client = ""

spark.sql("CREATE DATABASE azuretest")

val configs = Map(
  "fs.azure.account.auth.type" -> "OAuth",
  "fs.azure.account.oauth.provider.type" -> "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
  "fs.azure.account.oauth2.client.id" -> databricksPrincipalId,
  "fs.azure.account.oauth2.client.secret" -> databricksPrincipalSecret,
  "fs.azure.account.oauth2.client.endpoint" -> databricksPrincipalEndpoint)

dbutils.fs.mount(
  source = "abfss://"+environment+"-jars-"+client+"@"+environment+"storage"+client+".dfs.core.windows.net/",
  mountPoint = "/mnt/jars",
  extraConfigs = configs)
  
val df = spark.read.format("avro").load("dbfs:/mnt/jars/datatypes.avro")
df.write.saveAsTable("datatypes")
spark.sql("INSERT INTO datatypes SELECT decimal + 1, bigint + 1, string || '2',
 int +1, float +1, double +1, date + INTERVAL 1 DAY, timestamp + INTERVAL 1 HOUR,
  false, long + 1 FROM datatypes")
```

There are 5 variables at the top that will need to be updated. Navigate to Active Directory and then App Registrations. Search for the application named &lt;Environment&gt;-Databricks. Ex: Dev-Databricks. Click on this application. Use the Application \(client\) ID on the App Registration overview as the databricksPrincipalId value. Create a new client secret in the App Registration and save that value in the databricksPrincipalSecret value. Head back to the overview of this App Registration and click "Endpoints". Copy the value in "OAuth 2.0 token endpoint \(v1\)" and save that value in the databricksPrincipalEndpoint variable. Finally, enter the environment and client values that we used in the Terraform variable step.

Navigate to the storage container in the resource group called "&lt;environment&gt;storage&lt;client&gt;" Ex: devstorageintellio

Place the following file in the container called "&lt;environment&gt;-jars-&lt;client&gt;" Ex: dev-jars-intellio

{% embed url="https://s3.us-east-2.amazonaws.com/wmp.rap/datatypes.avro" %}

Once this file is uploaded, connect the workbook to the cluster that was created earlier, and run this snippet.

Test that the jars path is mounted by running 

```text
dbutils.fs.ls("mnt/jars")
```

This should not error out and should list the datatypes.avro file.

Next, click on the user dropdown in the top right of the Databricks portal. Click "User Settings" in the dropdown. On the Access Tokens tab, click "Generate New Token". Set the token lifetime to blank, so the token will not expire. Copy the token for use later on \(It will be used in the "Updating Key Vaults" section of this guide.

## Updating Key Vaults

## Configuring Custom Endpoint

## Configuring Postgres System Configuration Table

## Auth0 Rule Updates

