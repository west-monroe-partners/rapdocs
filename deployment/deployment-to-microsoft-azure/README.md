---
description: How to deploy Intellio DataOps (RAP) in a Microsoft Azure infrastructure
---

# !! Deployment to Microsoft Azure

Listed here are the steps required for Intellio DataOps \(RAP\) to be deployed in the Microsoft Azure infrastructure. Please



TODO - what does client need to set up beforehand, what permissions needed for user performing deployment, what to run

!! Currently working off of this [document](https://westmonroepartners1.sharepoint.com/:w:/r/sites/Technology/_layouts/15/Doc.aspx?sourcedoc=%7B9F621784-AD4C-4B1F-A1A9-909FF39BF74B%7D&file=Azure%20deployment.docx&action=default&mobileredirect=true)

## Pre-Deployment

### SSL Certificat

A valid SSL certificate that the client organization controls to perform secure connect and termination for RAP websites. Select from the following:

* Use an existing certificate and define a subdomain allocated to RAP.
* Purchase a new SSL certificate for a new domain or subdomain.
  * An Azure partner is Digicert.com
  * Deployment requires either a wildcard certificate or two single domain certificates per environment.
  * After purchase is complete, verify ownership of the domain to receive the certificate. **This is a requirement for deployment.**

### **Create a Docker Hub Account**

It is recommended this is not tied to any individual employee.

### Create an Auth0 Account

Create if one does not already exist with the following guidance:

* We recommend this account is not tied to an employee 
* Auth0 tier should be “Developer Pro” with external users, 100 external active users, and 1,000 Machine to Machine tokens
* Create an account for the RAP deployment team 

### Create Azure Instance

Again, recommend this account not be tied to any one person, and create an account for the RAP development team. The account should be able to create Active Directory resources.

### Choose VPN

Ensure the VPN can be deployed into a VNET Azure, or utilize Open VPN to be deployed into the RAP environment.

### Set Parameters 

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

