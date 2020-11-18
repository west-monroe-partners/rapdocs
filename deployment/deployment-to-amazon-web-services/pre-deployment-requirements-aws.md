# Pre Deployment Requirements \(AWS\)

Outlined here are the requirements necessary to deploy Intellio DataOps onto the AWS platform.

{% hint style="info" %}
For each of the following accounts re recommend a service account be created \(e.g. rap@westmonroepartners.com\) so as to not tie infrastructure to a specific employee.
{% endhint %}

## URL Management

Two options exist for URL management:

1. Create a sub-domain in an existing organization domain \(e.g. rap.wmp.com\) and delegate control to Route 53 DNS.
2. Pick an available domain and purchase in AWS.

Regardless of the above method, **the domain needs to be purchased and validated before a RAP deployment.**

## SSL Certificate

A valid SSL certificate that the client organization controls to perform secure connect and termination for RAP websites. Select from the following:

* Use an existing certificate and define a subdomain allocated to RAP.
* Purchase a new SSL certificate for a new domain or subdomain.
  * An Azure partner is Digicert.com
  * Deployment requires either a wildcard certificate or two single domain certificates per environment.
  * After purchase is complete, verify ownership of the domain to receive the certificate. **This is a requirement for deployment.**

## **Create a Docker Hub Account**

Create a [Docker Hub](https://hub.docker.com/signup) account, and it is recommended this is not tied to any individual employee.

## Create a Databricks Account

Recommended to create a premium account on [Databricks](https://databricks.com/try-databricks). The standard account is acceptable for the development environment, but a premium account will be needed before production release.

Create a specific account for the Intellio DataOps \(RAP\) deployment team.

## Create a GitHub Account

Create a [GitHub](https://github.com/) account. This will allow for access to the Intellio DataOps \(RAP\) source code.

## Create a Terraform Cloud Account

Create a [Terraform Cloud](https://www.terraform.io/) account. This is for infrastructure deployment.

## Create an Auth0 Account

[Auth0](https://auth0.com/) Developer tier is required. Again create a specific account for the Intellio DataOps \(RAP\) deployment team.

## Create an AWS Environment

If an AWS environment does not already exist, it is required to deploy onto AWS. [Create an account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/) specific for the Intellio DataOps \(RAP\) team, and the account should be able to create Active Directory resources.

## Decide on a VPN

If a VPN vendor is not already chosen, recommend to utilize Open VPN which can be deployed into the Intellio DataOps \(RAP\) environment.

## AWS Deployment Parameters

What follows is a list of parameters that tailor the standard AWS deployment environment.

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

If running a non-public facing deployment - these variables will need to be added:

| Variable | Example | Description |
| :--- | :--- | :--- |
| publicFacing | no | Triggers the infrastructure to deploy non-public facing resources |
| privateCertArn | arn:aws:acm:us-east-2:678910112:certificate/xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx | ARN to an imported SSL certificate that will be attached to the HTTPS listener on the internal load balancer |
| privateApiName | api.intellio.test | API url |
| privateDomainName | intellio.test | Base url for the environment |
| privateUIName | dev.intellio.test | UI url |
| privateRoute53ZoneId | Z04XXXXXXXX | Id for private hosted zone to add route 53 records to |

## Verify the deployment

Once RAP is up and running, the [Data Integration Example](../../getting-started-guide/data-integration-example/) in the Getting Started Guide can be followed to verify that the full RAP stack is working correctly. 

