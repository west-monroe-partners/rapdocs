# Performing the Deployment

## Creating Deployment Principal for Terraform

Navigate to Azure Active Directory and select "App registrations" on the left menu blade. Create a new registration. Make the name something that is appropriate for the master deployment registration - ex: Master-Terraform.

Choose "Accounts in this organizational directory only" for supported account types.

After creating the App Registration, navigate to the API permissions section of the App registration. Click "Add a permission" and navigate to "Supported legacy APIs". Then, click "Application permissions" and then choose these two permissions:

* Application.ReadWrite.All
* Directory.ReadWrite.All

Add the permissions and grant them consent if needed. 

Navigate to the Azure Subscription and add this newly created app registration as an "Owner" on the subscription.

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

## Post Terraform Steps

After the terraform is complete, there will be a resource group now created in Azure Portal. These are the main resources created by Terraform, but there are still configuration changes that need to be done before any data can be brought into the platform. 

## Configuring Databricks

In the Azure Portal Resource Group that's been created, navigate to the Databricks resource. Click "Launch Workspace" on the overview page.

In Databricks, navigate to the "Clusters" tab. Create a cluster named "rap-mini-sparky". Configure the cluster with the following configurations

![](../../.gitbook/assets/image%20%28276%29.png)

Click "Create Cluster".

Naviage back to to "Clusters" and click "Pools" and then "Create Pool". Create a pool called "sparky-pool" with the following configurations

![](../../.gitbook/assets/image%20%28275%29.png)

Click "Create".

After the pool is created, go back to the pool configuration and save the value called "DatabricksInstancePoolId" in the Tags section of the configuration. This value will be used later when updating the Key Vault.

Navigate to the Databricks home screen and create a new notebook. On a command box, add this code snippet:

```text
val databricksPrincipalId = ""
val databricksPrincipalSecret = ""
val databricksPrincipalEndpoint = ""
val environment = ""
val client = ""

spark.sql("CREATE DATABASE " + environment.toLowerCase)

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
spark.sql("INSERT INTO datatypes SELECT decimal + 1, bigint + 1, string || '2',int +1, float +1, double +1, date + INTERVAL 1 DAY, timestamp + INTERVAL 1 HOUR,false, long + 1 FROM datatypes")
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

We will need to make an API request to the Databricks API to create a secret scope for our Databricks secret. Recommended tool to do this is [Postman](https://www.postman.com/downloads/) - but any method to make a POST API request can be used. Make a POST API request to [https://xxxxxxxxxxx.azuredatabricks.net/api/2.0/secrets/scopes/create](https://adb-6797256387059301.1.azuredatabricks.net/api/2.0/secrets/scopes/create) \(Replace the x's with the value in the Databricks URL for the environment\) The body of the request will be -

```text
{
  "scope": "auth0token",
  "initial_manage_principal": "users"
}
```

The authorization is a "Bearer Token" and the token is the Databricks Access token we generated above. After making the request, the reponse should be a 200 OK with no body.

Capture a value out of the Databricks URL before moving on to the key vault step. If the URL is [https://xxxxxxxxxxxx.azuredatabricks.net/?o=4433553810974403\#/setting/clusters/1102-212023-gauge891/configuration](https://adb-4433553810974403.3.azuredatabricks.net/?o=4433553810974403#/setting/clusters/1102-212023-gauge891/configuration) then you would want to capture the "4433553810974403" portion after the /?o= for use when updating the Key Vault.

## Updating Key Vaults

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



