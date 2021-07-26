# Pre Deployment Requirements

Outlined here are the requirements necessary to deploy Intellio DataOps onto the AWS platform.

{% hint style="info" %}
For each of the following accounts we recommend a service account be created \(e.g. ido@wmp.com\) so as to not tie infrastructure to a specific employee.
{% endhint %}

## Decide on Public or Private Endpoint Architecture

Public Endpoints

* UI/API will be accessible on public internet, secured with Auth0 for authentication and SSL certificate for HTTPS
* On-premise source systems can use Agent to bypass firewall and VPN tunneling to stream data into platform

Private Endpoints

* UI will be accessed through private VM that is deployed in the IDO VNet, connections to the VM will be made using VPN or Amazon Appstream
* API is not publicly exposed
* Agent can only access networks that can be VPC Peered to IDO VNet

Please reach out to IDO team for diagrams of both architectures

## URL Management

Two options exist for URL management:

1. Create a sub-domain in an existing organization domain \(e.g. dataops.wmp.com\) and delegate control to Route 53 DNS.
2. Pick an available domain and purchase in AWS.

Regardless of the above method, **the domain needs to be purchased and validated before a DataOps deployment.**

## SSL Certificate

A valid SSL certificate that the client organization controls to perform secure connect and termination for DataOps websites. Select from the following:

* Use an existing certificate and define a subdomain allocated to DataOps.
* Purchase a new SSL certificate for a new domain or subdomain.
  * An Azure partner is Digicert.com
  * Deployment requires either a wildcard certificate or two single domain certificates per environment.
  * After purchase is complete, verify ownership of the domain to receive the certificate. **This is a requirement for deployment.**

## **Create a Docker Hub Account**

Create a [Docker Hub](https://hub.docker.com/signup) account, and it is recommended this is not tied to any individual employee. Send the Docker username to the West Monroe team so that they can provide access to the Intellio Docker repository.

## Sign up for a Databricks E2 Account

{% hint style="danger" %}
The Databricks Account will require a credit card to be added during sign up - Please have a corporate card or billing account ready to go
{% endhint %}

Reach out to WMP team or Databricks representative to get Databricks E2 Account provisioned

## Create a GitHub Account

Create a [GitHub](https://github.com/) account. This will allow for access to the Intellio DataOps source code.

## Create a Terraform Cloud Account

Create a [Terraform Cloud](https://www.terraform.io/) account. This is for infrastructure deployment.

## Create an Auth0 Account

[Auth0](https://auth0.com/) Developer tier is required. Again create a specific account for the Intellio DataOps deployment team.

## Create an AWS Environment

If an AWS environment does not already exist, it is required to deploy onto AWS. [Create an account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/) specific for the Intellio DataOps team, and the account should be able to create Active Directory resources.

## Decide on a VPN

If a VPN vendor is not already chosen, recommend to utilize Open VPN which can be deployed into the Intellio DataOps environment.

## AWS Deployment Parameters

What follows is a list of parameters that tailor the standard AWS deployment environment.

| Variable | Example | Description |
| :--- | :--- | :--- |
| awsRegion | us-west-2 | AWS Region for deployment - As of 11/9/2020 us-west-1 is not supported |
| awsAccessKey | xxx | Access Key for Master Deployment IAM user  - mark as sensitive |
| awsSecretKey | xxx | ecret Key for Master Deployment IAM user - mark as sensitive |
| environment | dev | This will be prepended to resources in the environment. E.g. Dev. Prod. etc.  |
| client | intellio | This will be postpended to resources in the environment - use company or organization name |
| vpcCidrBlock | 10.1 | Only the first two digits here, not the full CIDR block |
| avalibilityZoneA | us-west-2a | Not all regions have availability zones |
| avalibilityZoneB | us-west-2b | Not all regions have availability zones |
| RDSretentionperiod | 7 | Database backup retention period \(in days\) |
| RDSmasterusername | admin | Database master username |
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
| dockerPassword | xxx | DockerHub service account password |
| urlEnvPrefix | dev | Prefix for environment site url |
| baseUrl | intellioplatform | the base URL of the certificate - example [https://\(urlEnvPrefix\)\(baseUrl\).com](https://%28urlEnvPrefix%29%28baseUrl%29.com) This should not include www. .com or https://. e.g. "wmpdataops" |
| databricksToken | xxx | Token from the Databricks environment - generate access token in Databricks and place here |
| usEast1CertURL | \*.intellioplatform.com | Full certificate name \(with wildcards\) used for SSL |
| auth0Domain | intellioplatform.auth0.com | Domain of Auth0 account |
| auth0ClientId | xxx | Client ID of API Explorer Application in Auth0 \(needs to be generated when account is created\) |
| auth0ClientSecret | xxx | Client Secret of API Explorer Application in Auth0 \(needs to be generated when account is created\) |

If running a non-public facing deployment - these variables will need to be added:

| Variable | Example | Description |
| :--- | :--- | :--- |
| publicFacing | no | Triggers the infrastructure to deploy non-public facing resources |
| privateApiName | api.intellio.test | API url |
| privateDomainName | intellio.test | Base url for the environment |
| privateUIName | dev.intellio.test | UI url |

If running a non-public facing deployment - these variables are optional:

| Variable | Example | Description |
| :--- | :--- | :--- |
| privateCertArn | arn:aws:acm:us-east-2:678910112:certificate/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | ARN to an imported SSL certificate that will be attached to the HTTPS listener on the internal load balancer. If this variable is not added, a new certificate will be requested by the Terraform script. |
| privateRoute53ZoneId | Z04XXXXXXXX | Id for private hosted zone to add route 53 records to. If this variable is not added, a new private hosted zone will be created by the Terraform script. |

## Verify the deployment

Once DataOps is up and running, the [Data Integration Example](../../../getting-started/data-integration-example/) in the Getting Started Guide can be followed to verify that the full DataOps stack is working correctly. 

