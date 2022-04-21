# Process Override Example - Databricks 9.1

IDO 2.5.0 now offers support for Databricks Runtime 9.1. This version utilizes Spark version 3.1 and has been shown in our internal testing to run substantially faster than the Databricks version 7.3 used in older versions of IDO. However, some processes such as SQL Server Output, do not work correctly on Databricks Runtime 9.1. This page will instruct users on how to create a process override to run all processes on Databricks 9.1 except for Output related processes. It will leverage the 9.1 Demo Cluster Configuration and 9.1 Demo Process Configuration found [here](../cluster-configuration/cluster-configuration-example-databricks-9.1.md) and [here](process-configuration-example-databricks-9.1.md) respectively as examples.



## Navigating to the Existing Process Configuration Page

From any IDO page, click the hamburger menu icon in the top left, navigate to System Configuration, then click Process Configurations. This will bring users to the Process Configuration homepage. Then click the name of the Process Configuration to add overrides to. In this case, 9.1 Demo

![Navigating to Cluster Configurations](<../../../../.gitbook/assets/image (385) (1) (1) (1) (1) (1).png>)

## Adding the Process Overrides

From the Process Configuration page, click Add Override to create a new Override. This will add a new row to the Process Overrides table.&#x20;

For this example, we would like to override processes related to Output in order to avoid errors with Databricks Runtime 9.1. To do so, click the Select Process Type dropdown and the click on Output. Under the Cluster Configuration, we will select Default Sparky Configuration as it still runs on Databricks Runtime 7.3. However, users could use any Cluster Configuration configured to run Databricks 7.3. Repeat these steps for process type "Manual Reset Output" and "Manual Reset All Output" in order to ensure all Output processes use the correct cluster type. Then click Save.

![Three process overrides added to the 9.1 Demo Process Configuration](<../../../../.gitbook/assets/image (381) (1) (1) (1) (1) (1).png>)

## Further Steps

No further steps are required for Output processes to run on Databricks 7.3 Runtime. IDO will automatically handle switching between the clusters as it processes data.
