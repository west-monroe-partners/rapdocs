# !! Deployment to a New Environment

TODO - write an intro - RAP can be deployed on AWS or Azure, process should be similar but different

RAP can be deployed on Amazon Web Services or on Microsoft Azure. The process to deploy on either is similar, but there are key differences. Outlined in this section is how to deploy RAP.

{% hint style="info" %}
!! Key differences between AWS and Azure deployment.
{% endhint %}

## Prerequisites

Before RAP can be deployed, the following requirements need to be met:

* DNS name chosen for the target environment
* Auth0 account
* AWS or Azure account
* Administrator access to both AWS and Auth0 for the person who will be deploying the new RAP environment

## !! Deploying to Amazon Web Services \(AWS\)

TODO - what does client need to set up beforehand, what permissions needed for user performing deployment, what to run

!! AWS Deployment Steps [link to doc](https://westmonroepartners1.sharepoint.com/:w:/r/sites/Technology/_layouts/15/Doc.aspx?sourcedoc=%7B418824AA-8266-4D46-A551-78EF8371A901%7D&file=AWS%20pre%20deployment.docx&action=default&mobileredirect=true)



## !! Deploying to Microsoft Azure

TODO - what does client need to set up beforehand, what permissions needed for user performing deployment, what to run

!! Currently working off of this [document](https://westmonroepartners1.sharepoint.com/:w:/r/sites/Technology/_layouts/15/Doc.aspx?sourcedoc=%7B9F621784-AD4C-4B1F-A1A9-909FF39BF74B%7D&file=Azure%20deployment.docx&action=default&mobileredirect=true)

### Pre-Deployment

#### SSL Certificat

A valid SSL certificate that the client organization controls to perform secure connect and termination for RAP websites. Select from the following:

* Use an existing certificate and define a subdomain allocated to RAP.
* Purchase a new SSL certificate for a new domain or subdomain.
  * An Azure partner is Digicert.com
  * Deployment requires either a wildcard certificate or two single domain certificates per environment.
  * After purchase is complete, verify ownership of the domain to receive the certificate. **This is a requirement for deployment.**

#### **Create a Docker Hub Account**

It is recommended this is not tied to any individual employee.

#### Create an Auth0 Account

Create if one does not already exist with the following guidance:

* We recommend this account is not tied to an employee 
* Auth0 tier should be “Developer Pro” with external users, 100 external active users, and 1,000 Machine to Machine tokens
* Create an account for the RAP deployment team 

#### Create Azure

Again, recommend this account not be tied to any one person, and create an account for the RAP development team. The account should be able to create Active Directory resources.

#### Choose VPN

Ensure the VPN can be deployed into a VNET Azure, or utilize Open VPN to be deployed into the RAP environment.

#### Set Parameters 

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

### !! Teraforming

!! Split between Azure, AWS \(different providers, credentials\). Where to find the code.

!! split out Azure, AWS.



## !! Verifying the deployment

Once RAP is up and running, the [Data Integration Example](../getting-started-guide/data-integration-example/) in the Getting Started Guide can be followed to verify that the full RAP stack is working correctly.

