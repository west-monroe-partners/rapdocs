---
description: Requirements to set up RAP in a Microsoft Azure environment.
---

# Pre Deployment Requirements \(Microsoft Azure\)

This section is the list of requirements needed before RAP can be deployed in the Microsoft Azure environment. Make sure the information listed in each section is appropriately actioned upon whether via credentials, accounts or permissions.

## SSL Certificat

A valid SSL certificate that the client organization controls to perform secure connect and termination for RAP websites. Select from the following:

* Use an existing certificate and define a subdomain allocated to RAP.
* Purchase a new SSL certificate for a new domain or subdomain.
  * An Azure partner is Digicert.com
  * Deployment requires either a wildcard certificate or two single domain certificates per environment.
  * After purchase is complete, verify ownership of the domain to receive the certificate. **This is a requirement for deployment.**

## **Create a Docker Hub Account**

It is recommended this is not tied to any individual employee.

## Create an Auth0 Account

Create if one does not already exist with the following guidance:

* We recommend this account is not tied to an employee 
* Auth0 tier should be “Developer Pro” with external users, 100 external active users, and 1,000 Machine to Machine tokens
* Create an account for the RAP deployment team 

## Create Azure Environment

Again, recommend this account not be tied to any one person, and create an account for the RAP development team. The account should be able to create Active Directory resources.

## Choose VPN

Ensure the VPN can be deployed into a VNET Azure, or utilize Open VPN to be deployed into the RAP environment.

## Set Parameters 

Listed below are the parameters required to be set in the Microsoft Azure deployment.

| Variable | Description | Example |
| :--- | :--- | :--- |
| azureAccessKey | access key for WMP Azure account |  |
| azureSecretKey | secret key for WMP Azure account |  |
| region | Azure region to deploy RAP to |  |
| environment | Environment to be deployed | Prod |
| client | Client name | WMP |
| vpcCidrBlock | First two numbers of the VPC CIDR block | 10.0 |
| dockerUsername | Docker account authenticated with RAP private container registry username |  |
| dockerPassword | Docker account authenticated with RAP private container registry password |  |
| RDSmasterpassword | Database password |  |
| databricksToken | Databricks token |  |
| dnsZone | Base name of the website to deploy RAP to | Wmprap.com |

## Verify the deployment

Once RAP is up and running, the [Data Integration Example](../../getting-started-guide/data-integration-example/) in the Getting Started Guide can be followed to verify that the full RAP stack is working correctly.

