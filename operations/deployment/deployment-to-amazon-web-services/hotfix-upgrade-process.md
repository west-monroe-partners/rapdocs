# Hotfix Upgrade Process

## Prerequisites

* AWS Console Access
* Read and write access to DataOps ECS Clusters
* Read access to deployment cloudwatch logs
* RDS Query Editor access or PgAdmin access

## Running the Deployment Service

1. For hotfixes, we don't need to tweak the task definition since we're still using the same image version for our ECS containers. Head to the clusters page and click into the ECS Cluster that contains the containers that are being updated with the hotfix. When in the cluster, select the deployment Service Name and click "Update".

![Cluster and Services](../../../.gitbook/assets/d2.png)

10. Set the "Number of tasks" value from 0 to 1. Confirm that Task Definition is still on the latest. Click "Skip to review" and then "Update Service"

![Updating the Service](../../../.gitbook/assets/d3.png)

11. The service is now updated to start the deployment container, and will now start a task shortly. The task will run, perform the deployment steps in the running container, and then set the service back to 0.

## Updating External Agents 

### On-Premise Agent

1. Deployment service will update the agent jar file in S3 and trigger the auto update process- make sure Agent has autoUpdate set to true in the Agent parameters

### ECS Agent

1. Navigate to the Task Definition for the External Agent service and create a new revision. Update the image to the target version, similar to how the Deployment service was updated in the previous section steps.
2. Navigate to the Service for the External Image and update the Task Definition to the new revision \(latest\), then save the update to the service. 
3. A new task should start up with the new Task Definition and the old task should stop.
4. Confirm that the Agent is running on the target version by checking the Task Definition in the currently running external agent task.

## Monitor/Troubleshoot Deployment

1. Navigate to the Cloudwatch service in AWS and select "Log groups" on the left hand side of the page.
2. Find the log group called "/ecs/&lt;environment&gt;-deployment-&lt;client&gt;"
3. Click the logs for the latest running deployment

The Deployment service will log all of the steps it is performing, and attempt to rollback the environment to the previous version if any errors occur. 

Deployment can only run if there are no processes running in the environment. Use the RDS query editor to query the postgres database and check the **meta.process** table if Deployment is aborting due to processing running in the environment.

