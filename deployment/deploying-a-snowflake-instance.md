# Deploying a Snowflake Instance

## Setting up an account

The first step to deploying Snowflake is to create a Snowflake account. We recommend have an individual within the client's organization handle this so that the account is not tied to WMP. Sign up can be done [here](https://signup.snowflake.com/). No credit card will be needed for the initial Snowflake signup.

On the initial page, contact info must be filled out for the account. 

![](../.gitbook/assets/image%20%28120%29.png)

The next page will be used for basic account configuration. The Snowflake edition must be selected on this page. For private Azure deployments, Business Critical Edition must be used for security integration. For all other types of deployments, we recommend Enterprise edition. 

After selecting the Snowflake edition. Select the cloud provider that matches your DataOps deployment, then select the region that is same as/closest to the region in which your DataOps instance is deployed.

![](../.gitbook/assets/image%20%28206%29.png)

That's it for this page. Next we will need to activate the account via email.

## Activating the account

The email that signed up for the Snowflake account will receive an email with subject line **Activate Your Snowflake Account**. Click the **Click to Activate** link to continue.

![](../.gitbook/assets/image%20%28187%29.png)

This will navigate to the Snowflake activation page. Create an admin user name and password. Be sure to record this username and password. Click **Get Started.** Notice the URL of the page you are navigated to after clicking **Get Started**. This contains the account URL that will be needed in the future. Record it.

## Setting up networking

If using any type of DataOps deployment besides a private Azure deployment, no additional steps are necessary. For private Azure deployments, follow the steps detailed [here](https://docs.snowflake.com/en/user-guide/privatelink-azure.html#configuring-access-to-snowflake-with-azure-private-link) to connect Snowflake into your private Azure VNet.

## Setting up IDO user accounts and databases

SCRIPT COMING SOON

## Setting up a Snowflake Integration

An integration between Snowflake and the cloud account must be created in order to DataOps to write data out to Snowflake. 

Instructions for setting up an AWS storage integration can be found [here](https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html).

Instructions for setting up and Azure storage integration can be found [here](https://docs.snowflake.com/en/user-guide/data-load-azure-config.html#option-1-configuring-a-snowflake-storage-integration).

Snowflake integrations should be named _DATAOPS\_OUTPUT\_&lt;&gt;_ replacing the &lt;&gt; with the name of the DataOps environment. Notice that the name is in ALL CAPS. 

Snowflake integrations should have allowed storage locations of "s3://&lt;datalakeBucket&gt;" for AWS environments and "azure://&lt;storageAccount&gt;.blob.core.windows.net/&lt;datalakeContainer" for Azure environments.

The best way to find the value of the environment name is to run the below query against the DataOps postgres instance. 

```text
SELECT value FROM meta.system_configuration WHERE name = 'environment';

```

## Setting up a Snowflake IDO connection

The Snowflake instance is now ready for use with DataOps. Create a new connection and fill it out with the information relevant to your snowflake setup. We recommend leaving Connection String and Table Schema blank unless specifically needed.

![](../.gitbook/assets/image%20%28209%29.png)



