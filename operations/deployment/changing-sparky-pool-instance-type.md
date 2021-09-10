# Changing Sparky Pool Instance Type

The "sparky-pool" cluster pool in Databricks is what backs all the jobs that run in IDO with "DataOps Managed" cluster type selected. Sometimes, it may be necessary to change the default instance type that the pool is created with \(m5.xlarge\).

The main reason for changing the instance type is insufficient instance capacity for spot instances in AWS. If this problem is happening in your environment, you should see heartbeat errors in IDO and clusters failing to launch with the following error:

> Unexpected failure while waiting for the cluster \(0909-172847-query420\) to be ready.Cause Unexpected state for cluster \(0909-172847-query420\): AWS\_INSUFFICIENT\_INSTANCE\_CAPACITY\_FAILURE\(CLIENT\_ERROR\): aws\_api\_error\_code:InsufficientInstanceCapacity,aws\_error\_message:There is no Spot capacity available that matches your request.

To change the instance type, first find an instance that makes sense for your environment using the [AWS Spot Instance Advisor](https://aws.amazon.com/ec2/spot/instance-advisor/). Ideally, an instance with &lt;5% frequency of interruption would be the choice.

{% hint style="info" %}
IDO Engineering team currently recommends using an m5n.xlarge instance
{% endhint %}

Navigate to Terraform and open up the Variables tab for the workspace that we're updating. Scroll down to the "instanceType" variable, and change the value to the instance type that's being changed to.

![](../../.gitbook/assets/image%20%28372%29.png)

Queue up a Terraform Plan and make sure that it's only changing ECS Services, Sparky Pool, and Secrets Manager. You may also see a Databricks cluster named "init-cluster" being created - this is okay. 

Apply the plan and make sure the Deployment container runs. If the Deployment container can't run due to processing in the environment, that's okay - we just need the core processing containers to restart to pick up the new sparky-pool id. In this case, just stop the tasks for Core, API, and Agent. They will start back up and grab the new pool id.

{% hint style="danger" %}
After Terraform is applied, the Core, API, and Agent containers MUST BE RESTARTED before trying to run any processing. If this doesn't happen, then your clusters will fail with errors saying that you've provided them an cluster pool id that doesn't exist.
{% endhint %}



