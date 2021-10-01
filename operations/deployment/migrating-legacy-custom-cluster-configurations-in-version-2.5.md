# Migrating legacy custom cluster configurations in version 2.5

All source using custom cluster configurations will need to to be manually migrated after 2.5 release deployment. Migration steps:

1. Open source setttings, scroll down to Parameters-&gt;Performance & Cost-&gt;Custom Cluster Params and save the json value
2.  Using saved json config and databricks' [documentation for custom job & cluster configuration](https://docs.databricks.com/dev-tools/api/latest/jobs.html) as a reference, create new custom cluster configuration in UI by going to top level menu and clicking Cluster Configurations:

![](../../.gitbook/assets/image%20%28375%29.png)

3. After validating and saving cluster configuration, create Process Configuration with the new Cluster Configuration as default cluster

4. Go back to the original source setting screen and update process configuration with the one you  created in step 3:

![](../../.gitbook/assets/image%20%28374%29.png)

5. Save the source settings

