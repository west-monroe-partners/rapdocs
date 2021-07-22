# Development Environment Setup

For environments that are underdoing daily development, it may be beneficial to stand up a singleton cluster to attach to sources that are undergoing development. This will enable the user to have an always-on cluster that will require minimal to no cluster launching time when resetting processes on the source.

## Limitations

{% hint style="danger" %}
Do not use the "rap-mini-sparky" cluster as the singleton cluster!
{% endhint %}

{% hint style="danger" %}
Do not attach a Databricks notebook to the configured cluster. It will cause issues with DataOps processing.
{% endhint %}

## Cluster Configuration

Create a new cluster in Databricks. The name can be whatever makes sense for your team. Set the following configurations on the cluster. For Instance Profile - Choose the instance profile that has been configured for your environment. In most cases, there should only be one and it should be called "&lt;environment&gt;-db-instance-profile-&lt;client&gt;"

![New Singleton Cluster Configuration](../../.gitbook/assets/image%20%28368%29.png)

Once the cluster is created, navigate to the Tags section of the Advanced Options on the cluster. Take note of the ClusterId tag, we will use for IDO configuration.

![ClusterId Value](../../.gitbook/assets/image%20%28365%29.png)

## IDO Configuration

Navigate to the source that needs to use the new singleton cluster for processing. Click on the source settings tab, and locate the parameter called "Cluster Type". Choose the "Custom" radio button and a parameter called "Custom Cluster Params" should pop up. Add a json object to this field in the format shown in the following image. Format of the json will be: {"existing\_cluster\_id":"&lt;cluster-id&gt;"}

![](../../.gitbook/assets/image%20%28370%29.png)

Navigate to the Performance & Cost parameter section on the same source and configure the parameters to match the following image. 

![](../../.gitbook/assets/image%20%28366%29.png)

Once these are complete, make sure the changes to the source are saved! Once they're saved, try launching a process that has passed previously to test functionality. As long as the cluster is running, you should only be waiting for 10-15 seconds maximum for the job to be provisioned to the cluster \(IDO will display that it is launching a cluster\) and processes should start running immediately.

