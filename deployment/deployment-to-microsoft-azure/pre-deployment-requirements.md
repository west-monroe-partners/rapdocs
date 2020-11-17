---
description: Requirements to set up RAP in a Microsoft Azure environment.
---

# Pre Deployment Requirements \(Microsoft Azure\)

This section is the list of requirements needed before RAP can be deployed in the Microsoft Azure environment. Make sure the information listed in each section is appropriately actioned upon whether via credentials, accounts or permissions.

## Create a Service Account/Distribution list

Create a Distribution Group or Microsoft 365 group for all 3rd party account sign ups 

* DataOps uses as many native Azure services as possible, but some 3rd party vendors are used to allow for easier customization per client, and simplified operations for version upgrades/rollback

## SSL Certificate

A valid SSL certificate that the client organization controls to perform secure connect and termination for RAP websites. Select from the following:

* Use an existing certificate and define a subdomain allocated to RAP.
* Purchase a new SSL certificate for a new domain or subdomain.
  * An Azure partner is Digicert.com
  * Deployment requires either a wildcard certificate or two single domain certificates per environment.
  * After purchase is complete, verify ownership of the domain to receive the certificate. **This is a requirement for deployment.**

## **Create a Docker Hub Account**

Create a [Docker Hub](https://hub.docker.com/signup) account, and it is recommended this is not tied to any individual employee.

## Create an Auth0 Account

Create if one does not already exist with the following guidance:

* We recommend this account is not tied to an employee 
* Auth0 tier should be “Developer Pro” with external users, 100 external active users, and 1,000 Machine to Machine tokens
* Create an account for the RAP deployment team 

## Create Azure Environment

Again, recommend this account not be tied to any one person, and create an account for the RAP development team. The account should be able to create Active Directory resources.

## Create a Terraform Cloud Account

* We recommend this account is not tied to an employee 
* [https://www.terraform.io/](https://www.terraform.io/) 
* This will be used to manage the terraform deployment in the cloud 

## Create a GitHub Account

Create a [GitHub](https://github.com/) account. This will allow for access to the Intellio DataOps \(RAP\) source code.

## Choose VPN

Ensure the VPN can be deployed into a VNET Azure, or utilize Open VPN to be deployed into the RAP environment.

## Set Terraform Variable Parameters

| variable  | example  | description  |
| :--- | :--- | :--- |
| vpcCidrBlock  | 10.0  | Enter the first two digits for the VPC’s /16 CDIR block. Example: \`10.1\`  |
| dockerUsername  | wmprap  | Docker username for account that will have access to WMPDockerhub  |
| dockerPassword  | &lt;password&gt;  | Password for above account  |
| RDSmasterpassword  | &lt;password&gt;  | Administrative Password for the RDS Postgres Database. Use any printable ASCII character except /, double quotes, or @.  |
| auth0ClientId  | 384u3kddxj112j3  | Client Id of Auth0 account’s Management API application  |
| environment  | AzureDev  | The environment to be deployed. This is prepended to all resource names Ex: Dev  |
| databricksToken  |  | Populate this and reapply once the first deploy finishes and Databricks is configured.  |
| auth0ClientSecret  | s09df098ds0f8s0d8f0sd98f0s  | Client secret of Auth0 account’s Management API application  |
| auth0Domain  | wmprapdemo.auth0.com  | Domain of the Auth0 account  |
| client  | RAP  | Client name. This is postpended to all resource names. Ex: WMP  |
| clientSecret  | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure client secret from user/app authenticating deploy  |
| clientId  | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure client ID from user/app authenticating deploy  |
| dnsZone  | azure.wmprapdemo.com  | Base URL for the wildcard cert  |
| region  | East US  | Azure region to deploy the environment to  |
| tenantId  | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure tenant ID  |
| subscriptionId  | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure Subscription ID  |
| cert  |  | Contents of the SSL certificate  |
| imageVersion  | 2.0.6  | Deployment version for the platform  |

## Terraform README

\(Skip if using Terraform cloud\) Use this [link](https://www.terraform.io/docs/providers/azurerm/guides/azure_cli.html) to set up terraform on your cli and for all other questions regarding setting up a connection with azure look at [this](https://www.terraform.io/docs/providers/azurerm/index.html) link. Make sure the Azure cli works by running az resource list. Also make sure to set your default subscription with az account set --subscription=""  

In order to provision front door correctly right now you need to only assign 1 frontend URL, then uncomment the second URL and add it back into the script 

There is also a limitation with azure CDN and custom domain names. They can not be managed through terraform at this time. They need to be added manually and have their https turned on for Content Delivery Network. [link](https://github.com/terraform-providers/terraform-provider-azurerm/issues/398) to Github post about it.\ 

pkcs12 -export -in star.azure.wmprapdemo.com.crt -inkey STAR\_azure\_wmprapdemo\_com\_key.txt -out azure.wmprapdemo.pfx 

Need to make sure that I have access to create azure active directory tenants need to be able to create enterprise applications with this provision service principles 

You need to setup the right permissions for CDN to access your Key vault: 1\) Register Azure CDN as an app in your Azure Active Directory \(AAD\) via PowerShell using this command: New-AzureRmADServicePrincipal -ApplicationId "[205478c0](https://bitbucket.org/wmp-rap/infrastructure/commits/205478c0)-bd83-4e1b-a9d6-db63a3e1e1c8". 2\) Grant Azure CDN service the permission to access the secrets in your Key vault. Go to “Access policies” from your Key vault to add a new policy, then grant “Microsoft.Azure.Cdn” service principal a “get-secret” permission. 

## Verify the deployment

Once RAP is up and running, the [Data Integration Example](../../getting-started-guide/data-integration-example/) in the Getting Started Guide can be followed to verify that the full RAP stack is working correctly.

