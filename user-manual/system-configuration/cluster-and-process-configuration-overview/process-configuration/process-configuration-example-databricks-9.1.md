# Process Configuration Example - Databricks 9.1

IDO 2.5.0 now offers support for Databricks Runtime 9.1. This version utilizes Spark version 3.1 and has been shown in our internal testing to run substantially faster than the Databricks 7.3 Runtime used in prior IDO verisons. This page will demonstrate setting up and applying a Process Configuration that utilizes Databricks 9.1. The steps in this example require previously setting up a Cluster Configuration that utilizes Databricks Runtime 9.1. Instructions to do so can be found [here](../cluster-configuration/cluster-configuration-example-databricks-9.1.md).

## Navigating to the Process Configuration Page

From any IDO page, click the hamburger menu icon in the top left, navigate to System Configuration, then click Process Configurations. This will bring users to the Process Configuration homepage.

![Navigating to Cluster Configurations](<../../../../.gitbook/assets/image (385) (1) (1) (1).png>)

## Starting a New Process Config

Clicking the "New Process Configuration" button in the top right will bring the user to the new Process Configuration creation page. For this example, we will name the Process Configuration "9.1 Demo" Notice that by default the Process Configuration uses "Default Sparky Configuration"

![A new Process Configuration using the Default Sparky Configuration](<../../../../.gitbook/assets/image (380) (1) (1).png>)

## Switching to Databricks 9.1

Simply click on the Cluster Config dropdown under the Description field to access the available Cluster Configurations. Scroll down and click 9.1 Demo to change the associated Cluster Configuration to the 9.1 configuration created in previous steps.

![The Process Configuration now uses the 9.1 Demo Cluster Config](<../../../../.gitbook/assets/image (386) (1) (1) (1).png>)

## Saving the configuration & applying to Sources

Click save. The Process Config has been successfully created! Now that the configuration has been saved. It can be applied to Sources. To apply the configuration to a Source, navigate to the desired Source Settings page. Look for the Process Config drop down on the page and select the 9.1 Demo config that was just created. Hit save.

![A source configured to use the 9.1 Demo Process Config](<../../../../.gitbook/assets/image (383) (1) (1).png>)

All processes on this source will now be performed using the 9.1 cluster configuration. Expect faster results!

## Known Limitations

Databricks 9.1 utlilzes an updated version of Spark in comparison to the Databricks 7.3 version that was used in IDO 2.4.3 and earlier. Known IDO issues with the 9.1 version can be found below. In the event of an issue with the 9.1 cluster, we recommend using the Process Override functionality detailed [here](process-override-example-databricks-9.1.md) to run singular processes on Databricks 7.3.

* SQL Server Output
  * SQL Server output does not work on Databricks Runtime 9.1

