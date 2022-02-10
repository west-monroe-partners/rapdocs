# Performing the Deployment

## Creating Deployment Principal for Terraform

Navigate to Azure Active Directory and select "App registrations" on the left menu blade. Create a new registration. Make the name something that is appropriate for the master deployment registration - ex: Master-Terraform.

Choose "Accounts in this organizational directory only" for supported account types.

After creating the App Registration, navigate to the API permissions section of the App registration. Click "Add a permission" and navigate to "Supported legacy APIs". Then, click "Application permissions" and then choose these two permissions:

* Application.ReadWrite.All
* Directory.ReadWrite.All

Add the permissions and grant them consent if needed.&#x20;

Save the Application (client) ID and generate a secret for the app registration. These will be used when populating variables in Terraform Cloud in the next step.

## Fork Infrastrucure Repository in GitHub

Work with DataOps team to get a service GitHub account added - this will be used to fork the main Infrastructure repository into the service account.

## Setting up Terraform Cloud Workspace

Create a new workspace in Terraform Cloud. Choose "Version control workflow". Configure the VCS connection to the forked repository in GitHub. Follow the Terraform Cloud steps when configuring a new VCS connection.

When the VCS connection is created, set the working directory to "terraform/azure". The VCS branch can be the default branch, as it generally defaults to master.

{% hint style="info" %}
Make sure that the Terraform version in the workspace is set to "1.1.5"
{% endhint %}

## Populating Variables in Terraform Cloud

Manually enter the following variable names and set values accordingly. If predeployment steps were followed, these values should be mostly known.&#x20;

{% hint style="danger" %}
The variable names are case sensitive - please enter them as they appear in this list
{% endhint %}

| variable           | example                                  | description                                                                                                              |
| ------------------ | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| vpcCidrBlock       | 10.0                                     | Enter the first two digits for the VPC’s /16 CDIR block. Example: \`10.1\`                                               |
| dockerUsername     | intellio                                 | Docker username for account that will have access to WMPDockerhub                                                        |
| dockerPassword     | \<password>                              | Password for above account                                                                                               |
| RDSmasterpassword  | \<password>                              | Administrative Password for the RDS Postgres Database. Use any printable ASCII character except /, double quotes, or @.  |
| auth0ClientId      | 384u3kddxj112j3                          | Client Id of Auth0 account’s Management API application                                                                  |
| environment        | Dev                                      | The environment to be deployed. This is prepended to all resource names Ex: Dev                                          |
| databricksToken    |                                          | Populate this and reapply once the first deploy finishes and Databricks is configured.                                   |
| auth0ClientSecret  | s09df098ds0f8s0d8f0sd98f0s               | Client secret of Auth0 account’s Management API application                                                              |
| auth0Domain        | intellio.auth0.com                       | Domain of the Auth0 account                                                                                              |
| client             | Intellio                                 | Client name. This is postpended to all resource names. Ex: WMP                                                           |
| clientSecret       | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure client secret from user/app authenticating deploy - This comes from step one, the deployment principal             |
| clientId           | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure client ID from user/app authenticating deploy - This comes from step one, the deployment principal                 |
| dnsZone            | dev.intellio.com                         | Base URL for the wildcard cert                                                                                           |
| region             | East US                                  | Azure region to deploy the environment to                                                                                |
| tenantId           | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure tenant ID                                                                                                          |
| subscriptionId     | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure Subscription ID                                                                                                    |
| cert               |                                          | Contents of the SSL certificate                                                                                          |
| imageVersion       | 2.x.x                                    | Deployment version for the platform                                                                                      |
| publicFacing       | yes                                      | Is the environment public or private facing?                                                                             |

## SSL Certificate Contents

The "cert" variable will need the SSL certificate contents in Base 64 encoding, so it can be saved as a text variable. To do this, you will need to download the certificate in .pfx format, with no password protection. This can easily be done if the certificate is saved in Azure Key Vault certificate manager. Once the certificate is downloaded, run these two commands in Windows Powershell (change the values in the first line to point to the pfx file on your local system):

$fileContentBytes = get-content 'C:\\\<path-to-pfx>\\\<file>.pfx' -Encoding Byte

\[System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-encoded-bytes.txt'

Then, open pfx-encoded-bytes.txt and save the contents of the file into the "cert" variable in Terraform.

## Running Terraform Cloud

When the variables are configured, Terraform is ready to generate a plan. Click the "Queue plan" button and let Terraform do it's magic. If all the variables are correct, then the plan should have about 74 resources to add. If the plan finishes successfully, then click apply and let the deployment begin!

## Post Terraform Steps

After the terraform is complete, there will be a resource group now created in Azure Portal. These are the main resources created by Terraform, but there are still configuration changes that need to be done before any data can be brought into the platform.&#x20;

## Configuring Databricks

In the Azure Portal Resource Group that's been created, navigate to the Databricks resource. Click "Launch Workspace" on the overview page.

Navigate to the storage container in the resource group called "\<environment>storage\<client>" Ex: devstorageintellio

Place the following file in the container called "\<environment>-jars-\<client>" Ex: dev-jars-intellio

{% embed url="https://s3.us-east-2.amazonaws.com/wmp.rap/datatypes.avro" %}

Once this file is uploaded, connect the workbook called "databricks-init" and run the workbook. Attach to the "dataops-init-cluster"

If the workbook runs successfully, move on to the next step!&#x20;

## Running Deployment Container

Navigate to the Container instance named \<environment>-Deployment-\<client>. Ex: Dev-Deployment-Intellio. Click "Containers" on the left blade menu and then click "Logs". Check to see if the container has ran successfully. There should be a final message in the logs that says

```
 INFO  Azure.AzureDeployActor - Deployment complete
```

If this message exists - the first time deployment is good to go.

If this message does not exist - try running the container again (click stop and start on the container) and troubleshoot from there.&#x20;

## Configuring Custom Endpoint

Navigate to the Frontend Endpoint resource called \<environment>-FrontendEndpoint-\<client>, Ex: Dev-FrontendEndpoint-Intellio. Click "Custom domain" in the overview screen. In the "Custom hostname" box, enter the DNS name of the Intellio site that is being deployed. This will generally be: \<environment>-\<dnsZone>. dnsZone was a variable that was set when the Terraform variables were populated. The custom hostname will need to be DNS resolvable before it can be added.

After adding the custom hostname, click on the custom hostname to configure the domain further. The configuration should then look similar to the following image, with the deployment specific values replaced.

![](<../../../../.gitbook/assets/image (278).png>)

{% hint style="warning" %}
Make sure the Azure CDN step is followed so that CDN can access the Key Vault where the secret lives
{% endhint %}

Save the configuration and this step will be complete.

## Auth0 Rule Updates

In the Auth0 Dashboard there is a section on the left hand menu called "Rules". Edit the "Email domain whitelist" rule to add domains that should be able to sign up to the Intellio Frontend. By default, the rule is generated with only the WMP emails.

![](<../../../../.gitbook/assets/image (277) (1).png>)

