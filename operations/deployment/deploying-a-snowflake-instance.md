# Deploying a Snowflake Instance

## Setting up an account

The first step to deploying Snowflake is to create a Snowflake account. We recommend have an individual within the client's organization handle this so that the account is not tied to WMP. Sign up can be done [here](https://signup.snowflake.com/). No credit card will be needed for the initial Snowflake signup.

On the initial page, contact info must be filled out for the account. 

![](../../.gitbook/assets/image%20%28120%29.png)

The next page will be used for basic account configuration. The Snowflake edition must be selected on this page. For deployments without public endpoints, Business Critical Edition must be used for security integration using PrivateLink. For all other types of deployments, we recommend Enterprise edition. 

After selecting the Snowflake edition. Select the cloud provider that matches your DataOps deployment, then select the region that is same as/closest to the region in which your DataOps instance is deployed.

![](../../.gitbook/assets/image%20%28206%29.png)

That's it for this page. Next we will need to activate the account via email.

## Activating the account

The email that signed up for the Snowflake account will receive an email with subject line **Activate Your Snowflake Account**. Click the **Click to Activate** link to continue.

![](../../.gitbook/assets/image%20%28187%29.png)

This will navigate to the Snowflake activation page. Create an admin user name and password. Be sure to record this username and password. Click **Get Started.** Notice the URL of the page you are navigated to after clicking **Get Started**. This contains the account URL that will be needed in the future. Record it.

## Setting up networking

If your DataOps deployment has public endpoints, no additional steps are necessary. For fully private deployments, follow the steps detailed [here](https://docs.snowflake.com/en/user-guide/privatelink-azure.html#configuring-access-to-snowflake-with-azure-private-link) for Azure and [here](https://docs.snowflake.com/en/user-guide/admin-security-privatelink.html#what-is-aws-privatelink) for AWS to set up a PrivateLink connection between the internal and snowflake networks.

## Setting up IDO user accounts and databases

Log into the Snowflake portal using the account URL recorded earlier and the admin credentials. A worksheet should be opened by default. In the top right corner of the worksheet, Change the Role to ACCOUNTADMIN. Then run each of the following commands in the worksheet. 

```text
--Create Database
CREATE DATABASE SNOWFLAKE_EXAMPLE;

-- read only user
CREATE role READ_ROLE;
CREATE USER read_user password = 'password', LOGIN_NAME = read_user, DEFAULT_ROLE = READ_ROLE, DEFAULT_WAREHOUSE = COMPUTE_WH;
GRANT ROLE READ_ROLE to USER read_user;
GRANT USAGE, OPERATE on warehouse COMPUTE_WH to role READ_ROLE;
GRANT USAGE ON DATABASE SNOWFLAKE_EXAMPLE TO role READ_ROLE;
GRANT USAGE ON SCHEMA SNOWFLAKE_EXAMPLE.PUBLIC TO role READ_ROLE;
GRANT select on all tables in schema SNOWFLAKE_EXAMPLE.PUBLIC to role READ_ROLE;
GRANT select on all views in schema SNOWFLAKE_EXAMPLE.PUBLIC to role READ_ROLE;
GRANT select on future tables in schema SNOWFLAKE_EXAMPLE.PUBLIC to role READ_ROLE;
GRANT select on future views in schema SNOWFLAKE_EXAMPLE.PUBLIC to role READ_ROLE;

--Read/Write User
CREATE role RW_ROLE;
CREATE USER rw_user password = 'password', LOGIN_NAME = rw_user, DEFAULT_ROLE = RW_ROLE, DEFAULT_WAREHOUSE = COMPUTE_WH;
GRANT ROLE RW_ROLE to USER rw_user;
GRANT USAGE, OPERATE on warehouse COMPUTE_WH to role RW_ROLE;
GRANT USAGE ON DATABASE SNOWFLAKE_EXAMPLE to role RW_ROLE;
GRANT ALL, usage ON schema SNOWFLAKE_EXAMPLE.PUBLIC to ROLE RW_ROLE;
GRANT ALL on ALL TABLES IN SCHEMA SNOWFLAKE_EXAMPLE.PUBLIC to role RW_ROLE;
GRANT ALL on all views in schema SNOWFLAKE_EXAMPLE.PUBLIC to role RW_ROLE;
GRANT ALL ON FUTURE TABLES IN SCHEMA SNOWFLAKE_EXAMPLE.PUBLIC TO ROLE RW_ROLE;
GRANT ALL on future views in schema SNOWFLAKE_EXAMPLE.PUBLIC to role RW_ROLE;

```

## Setting up a Snowflake Integration

{% hint style="warning" %}
As of IDO version 2.4.0, the Snowflake Integration is no longer needed. We now use the Spark integration with Snowflake to write data to Snowflake during the Output process. 
{% endhint %}

An integration between Snowflake and the cloud account must be created in order to DataOps to write data out to Snowflake. 

Instructions for setting up an AWS storage integration can be found [here](https://docs.snowflake.com/en/user-guide/data-load-s3-config-storage-integration.html).

Instructions for setting up and Azure storage integration can be found [here](https://docs.snowflake.com/en/user-guide/data-load-azure-config.html#option-1-configuring-a-snowflake-storage-integration).

Snowflake integrations should be named _DATAOPS\_OUTPUT\_&lt;&gt;_ replacing the &lt;&gt; with the name of the DataOps environment. Notice that the name is in ALL CAPS. 

Snowflake integrations should have allowed storage locations of "s3://&lt;datalakeBucket&gt;" for AWS environments and "azure://&lt;storageAccount&gt;.blob.core.windows.net/&lt;datalakeContainer" for Azure environments.

The best way to find the value of the environment name is to run the below query against the DataOps Postgres instance. 

```text
SELECT value FROM meta.system_configuration WHERE name = 'environment';
```

After creating the integration, be sure to grant usage of that integration to the Snowflake user/role that will be used for the DataOps output connection. Do so by running a statment similar to the one below.

```text
GRANT USAGE ON INTEGRATION DATAOPS_OUTPUT_SNOWFLAKEEXAMPLE TO ROLE RW_ROLE;
```

## Setting up a Snowflake IDO connection

The Snowflake instance is now ready for use with DataOps. Create a new connection and fill it out with the information relevant to your snowflake setup. We recommend leaving Connection String and Table Schema blank unless specifically needed.

![](../../.gitbook/assets/image%20%28209%29.png)

After creating the connection. It will be available for use in Snowflake outputs. You are now ready to output to Snowflake via DataOps.

