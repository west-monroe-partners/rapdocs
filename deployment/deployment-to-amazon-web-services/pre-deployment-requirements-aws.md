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

What follows is a list of parameters that tailor the AWS deployment environment.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Variable</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Additional Info</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">awsRegion</td>
      <td style="text-align:left">AWS Region for deployment</td>
      <td style="text-align:left">As of 6/24/2020 us-west-1 is not supported</td>
    </tr>
    <tr>
      <td style="text-align:left">awsAccessKey</td>
      <td style="text-align:left">Access key for AWS account running the terraform script</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">awsSecretKey</td>
      <td style="text-align:left">Secret Key for AWS account running the terraform script</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">environment</td>
      <td style="text-align:left">Environment being deployed</td>
      <td style="text-align:left">This will be prepended to resources in the environment. E.g. Dev. Prod.
        etc.</td>
    </tr>
    <tr>
      <td style="text-align:left">Client Name</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">vpcCidrBlock</td>
      <td style="text-align:left">CIDR block (first two digits)</td>
      <td style="text-align:left">Only the first two digits here, not the full CIDR block. e.g. 10.1</td>
    </tr>
    <tr>
      <td style="text-align:left">avalibilityZoneA</td>
      <td style="text-align:left">Primary availability zone</td>
      <td style="text-align:left">Not all regions have availability zones. e.g. us-west-2-a</td>
    </tr>
    <tr>
      <td style="text-align:left">avalibilityZoneB</td>
      <td style="text-align:left">Secondary availability zone</td>
      <td style="text-align:left">
        <p>Not all regions have availability zones</p>
        <p>e.g. us-west-2-a</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">RDSretentionperiod</td>
      <td style="text-align:left">Database backup retention period (in days)</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">RDSmasterusername</td>
      <td style="text-align:left">Database master username</td>
      <td style="text-align:left">e.g. admin</td>
    </tr>
    <tr>
      <td style="text-align:left">RDSmasterpassword</td>
      <td style="text-align:left">Database master password</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">RDSpostgresengineversion</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">RDSport</td>
      <td style="text-align:left">Database port</td>
      <td style="text-align:left">e.g. 5432</td>
    </tr>
    <tr>
      <td style="text-align:left">TransitiontoAA</td>
      <td style="text-align:left">Transition to Standard-Infrequent Access</td>
      <td style="text-align:left">Default is 60 days</td>
    </tr>
    <tr>
      <td style="text-align:left">TransitiontoGLACIER</td>
      <td style="text-align:left">Transition to Amazon Glacier</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">stageUsername</td>
      <td style="text-align:left">Database Stage username</td>
      <td style="text-align:left">e.g. stage_user</td>
    </tr>
    <tr>
      <td style="text-align:left">stagePassword</td>
      <td style="text-align:left">Database Stage password</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">coreImageName</td>
      <td style="text-align:left">Core application Docker image tag</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">agentImageName</td>
      <td style="text-align:left">Agent application Docker image tag</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">apiImageName</td>
      <td style="text-align:left">API application Docker image tag</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">deploymentImageName</td>
      <td style="text-align:left">Deployment application Docker image tag</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">dockerUsername</td>
      <td style="text-align:left">Client docker service account username</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">dockerPassword</td>
      <td style="text-align:left">Client docker service account password</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">urlEnvPrefix</td>
      <td style="text-align:left">prefix for environment URLs</td>
      <td style="text-align:left">e.g. dev or prod</td>
    </tr>
    <tr>
      <td style="text-align:left">baseUrl</td>
      <td style="text-align:left">the base URL of the certificate</td>
      <td style="text-align:left">example <a href="https://(urlEnvPrefix)(baseUrl).com">https://(urlEnvPrefix)(baseUrl).com</a> This
        should not include www. .com or https://. e.g. &quot;wmp-rap&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left">databricksToken</td>
      <td style="text-align:left">Token from the Databricks environment</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">usEast1CertURL</td>
      <td style="text-align:left">full certificate used for SSL</td>
      <td style="text-align:left">e.g. *.wmrapdemo.com</td>
    </tr>
    <tr>
      <td style="text-align:left">auth0Domain</td>
      <td style="text-align:left">Auth0 domain</td>
      <td style="text-align:left">See Auth0 Deployment documentation.</td>
    </tr>
    <tr>
      <td style="text-align:left">auth0ClientId</td>
      <td style="text-align:left">Auth0 client id</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">auth0ClientSecret</td>
      <td style="text-align:left">Auth0 client secret</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

## Verify the deployment

Once RAP is up and running, the [Data Integration Example](../../getting-started-guide/data-integration-example/) in the Getting Started Guide can be followed to verify that the full RAP stack is working correctly. 

