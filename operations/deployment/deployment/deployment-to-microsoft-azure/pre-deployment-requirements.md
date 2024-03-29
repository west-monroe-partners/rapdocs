---
description: Requirements to set up DataOps in a Microsoft Azure environment.
---

# Pre Deployment Requirements

This section is the list of requirements needed before DataOps can be deployed in the Microsoft Azure environment. Make sure the information listed in each section is appropriately actioned upon whether via credentials, accounts or permissions.

## Create a Service Account/Distribution list

Create a Distribution Group or Microsoft 365 group for all 3rd party account sign ups&#x20;

* DataOps uses as many native Azure services as possible, but some 3rd party vendors are used to allow for easier customization per client, and simplified operations for version upgrades/rollback

## Create Azure Environment

Recommend this account not be tied to any one person, and create an account for the DataOps development team. The account should be able to create Active Directory resources.

The Azure account should be upgraded to a Pay-As-You-Go plan at minimum, and a subscription should be created for the IDO deployment, unless there is an existing Subscription to use. The $29/mo Azure support plan is highly recommended at minimum, as there may be need to raise quotas in the Subscription if this is a new account.&#x20;

There is also a chance that the Subscription will be locked for creating new VM's and this will be a blocker for deployment. This is usually seen in brand new Azure accounts. Once the Azure account is created and a Subscription is created, try going to the Subscription and creating a VM. If you can't create any VM's, the following process will need to be followed before you can deploy IDO. [https://docs.microsoft.com/en-us/troubleshoot/azure/general/region-access-request-process](https://docs.microsoft.com/en-us/troubleshoot/azure/general/region-access-request-process)

{% hint style="danger" %}
If creating a brand new Azure account is necessary, we strongly recommend doing these  steps 1-2 weeks in advance of the deployment
{% endhint %}

## Decide on Public or Private Endpoint Architecture

Public Endpoints

* UI/API will be accessible on public internet, secured with Auth0 for authentication and SSL certificate for HTTPS
* On-prem source systems can use Agent to bypass firewall and VPN tunneling to stream data into platform

Private Endpoints

* UI will be accessed through private VM that is deployed in the IDO VNet, connections to the VM will be made using Azure Bastion
* API is not publicly exposed
* Agent can only access networks that can be VNet Peered to IDO VNet

Please reach out to IDO team for diagrams of both architectures

## Define DNS Names and Process for Managing Records

* One name for UI and one for API
  * EX: prod.dataops.com and api.prod.dataops.com
* Delegate subdomain or create DNS records in DNS provider
* GoDaddy recommended if there is no DNS provider currently being used

## SSL Certificate

A valid SSL certificate that the client organization controls to perform secure connect and termination for DataOps websites. Select from the following:

* Use an existing certificate and define a subdomain allocated to DataOps.
* Purchase a new SSL certificate for a new domain or subdomain.
  * An Azure partner is Digicert.com, GoDaddy is recommended if DNS is set up using GoDaddy
  * Deployment requires either a wildcard certificate or two single domain certificates per environment.
  * Certificate must cover the DNS names defined in the previous step!
  * After purchase is complete, verify ownership of the domain to receive the certificate. **This is a requirement for deployment.**

## **Create a Docker Hub Account**

Create a [Docker Hub](https://hub.docker.com/signup) account, and it is recommended this is not tied to any individual employee.

## Create an Auth0 Account

Create if one does not already exist with the following guidance:

* We recommend this account is not tied to an employee&#x20;
* [https://auth0.com/](https://auth0.com)
* Auth0 tier should be a minimum of free tier, but “Developer” ($23/month) with external users, 100 external active users, and 1,000 Machine to Machine tokens is recommended if deploying more than one environment
* Create an account for the DataOps deployment team

## Create a Terraform Cloud Account

* We recommend this account is not tied to an employee&#x20;
* [https://www.terraform.io/](https://www.terraform.io)&#x20;
* This will be used to manage the terraform deployment in the cloud&#x20;

## Create a GitHub Account

Create a [GitHub](https://github.com) account. This will allow for access to the Intellio DataOps source code.

## Choose VPN (Optional, can be done later)

Ensure the VPN can be deployed into a VNET Azure, or utilize Open VPN to be deployed into the DataOps environment.

## Set Terraform Variable Parameters

| variable           | example                                  | description                                                                                                              |
| ------------------ | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| vpcCidrBlock       | 10.0                                     | Enter the first two digits for the VPC’s /16 CDIR block. Example: \`10.1\`                                               |
| dockerUsername     | wmp                                      | Docker username for account that will have access to WMPDockerhub                                                        |
| dockerPassword     | \<password>                              | Password for above account                                                                                               |
| RDSmasterpassword  | \<password>                              | Administrative Password for the RDS Postgres Database. Use any printable ASCII character except /, double quotes, or @.  |
| auth0ClientId      | 384u3kddxj112j3                          | Client Id of Auth0 account’s Management API application                                                                  |
| environment        | AzureDev                                 | The environment to be deployed. This is prepended to all resource names Ex: Dev                                          |
| databricksToken    |                                          | Populate this and reapply once the first deploy finishes and Databricks is configured.                                   |
| auth0ClientSecret  | s09df098ds0f8s0d8f0sd98f0s               | Client secret of Auth0 account’s Management API application                                                              |
| auth0Domain        | wmpdemo.auth0.com                        | Domain of the Auth0 account                                                                                              |
| client             | orgname                                  | Client name. This is postpended to all resource names. Ex: WMP                                                           |
| clientSecret       | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure client secret from user/app authenticating deploy                                                                  |
| clientId           | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure client ID from user/app authenticating deploy                                                                      |
| dnsZone            | azure.wmpdemo.com                        | Base URL for the wildcard cert                                                                                           |
| region             | East US                                  | Azure region to deploy the environment to                                                                                |
| tenantId           | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure tenant ID                                                                                                          |
| subscriptionId     | c6fxxxxxbf1-axxa-43d1-axx8-c50669xxxxef  | Azure Subscription ID                                                                                                    |
| cert               |                                          | Contents of the SSL certificate - see instructions below                                                                 |
| imageVersion       | 2.0.6                                    | Deployment version for the platform                                                                                      |
| publicFacing       | yes                                      | Is the infrastructure private or public facing?                                                                          |

## SSL Certificate Contents

The "cert" variable will need the SSL certificate contents in Base 64 encoding, so it can be saved as a text variable. To do this, you will need to download the certificate in .pfx format, with no password protection. This can easily be done if the certificate is saved in Azure Key Vault certificate manager. Once the certificate is downloaded, run these two commands in Windows Powershell (change the values in the first line to point to the pfx file on your local system):

$fileContentBytes = get-content 'C:\\\<path-to-pfx>\\\<file>.pfx' -Encoding Byte

\[System.Convert]::ToBase64String($fileContentBytes) | Out-File 'pfx-encoded-bytes.txt'

Then, open pfx-encoded-bytes.txt and save the contents of the file into the "cert" variable in Terraform.

## Next Steps

Once all of the prerequisites are complete, and the variables have been figured out, navigate to the [Performing the Deployment](https://app.gitbook.com/@wmp-rap/s/rap/\~/drafts/-MVMMZtmzextcDim-8qp/v/master/deployment/deployment-to-microsoft-azure/performing-the-deployment) guide to begin deploying IDO resources.

## Verify the deployment

Once DataOps is up and running, the [Data Integration Example](../../../../getting-started/data-integration-example/) in the Getting Started Guide can be followed to verify that the full DataOps stack is working correctly.
