# Cluster Configuration Example - Databricks 9.1

IDO 2.5.0 now offers support for Databricks Runtime 9.1. This version utilizes Spark version 3.1 and has been shown in our internal testing to run substantially faster than the Databricks 7.3 Runtime used in prior IDO verisons. This page will demonstrate setting up and applying a Cluster Configuration that utilizes Databricks 9.1.

## Navigating to the Custom Cluster Configuration Page

From any IDO page, click the hamburger menu icon in the top left, navigate to System configuration, then click Cluster Configurations. This will bring users to the Cluster Configuration homepage

![Navigating to Cluster Configurations](<../../../../.gitbook/assets/image (385) (1) (1) (1) (1).png>)

## Starting a New Cluster Config

Clicking the "New Cluster" button in the top right will bring the user to the new Cluster Configuration creation page. For this example, we will name the Cluster Configuration "9.1 Demo" Notice that by default the cluster uses Databricks version 7.3.

![A new cluster config with default 7.3 Databricks Runtime](<../../../../.gitbook/assets/image (382) (1) (1) (1) (1).png>)

## Switching to Databricks 9.1

Simply click on the Spark Version dropdown under the "Cluster Configuration" parameters category to access the available versions of the Databricks Runtime. Scroll down and click 9.1.x-scala2.12 to change the runtime to 9.1.&#x20;

## Saving the configuration

Click save. The Cluster Config has been successfully created! Notice that a "Job ID" has been added just below the Description field. Clicking this link will take the user to the Databricks job associated with this configuration. Now that the configuration has been saved. It will have to be applied to a Process Configuration in order to attach it to IDO Sources. See [here](../process-configuration/process-configuration-example-databricks-9.1.md) for instructions to do so.
