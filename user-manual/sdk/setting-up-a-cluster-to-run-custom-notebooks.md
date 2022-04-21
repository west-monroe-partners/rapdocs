# Setting up a Cluster to run Custom Notebooks

Before creating any custom Notebooks for IDO, it is best to setup the Databricks environment. This page will detail the process of creating a cluster and attaching the required libraries to it.



## Creating a Cluster

While IDO comes out of the box with two Databricks clusters, mini-sparky and data-viewer, it is recommended to setup an entirely separate cluster to run Custom Processes. This will prevent any interference between UI functionality and Custom Processes running. To start, users should navigate into the Databricks UI for their environment.

1. Within the Databricks UI, locate the Create button and the click Cluster. This will open the Cluster Creation screen where users are able to configure specific cluster instance details. For the purpose of running a basic Custom Process, we will create a fairly vanilla cluster, but more details on the options available can be found [here](https://docs.databricks.com/clusters/create.html).

![Click the Create Cluster Button to begin](<../../.gitbook/assets/image (389) (1).png>)

2\. The cluster can be configured similar to the image below.&#x20;

* The cluster can be named anything. CustomProcessCluster will be used for this example.
* Switching the Cluster mode to "Single Node" will save money, but limits the amount of data that the cluster can process. For a quick Hello World example it should be fine but consider switching to a Standard Cluster if data volumnes are larger
* The Databricks 7.3 LTS runtime is the standard runtime used in IDO, so we will use it here.
* Autoscaling local storage can be turned off
* The auto termination can be turned down to 10 minutes to save money
* The default Node Type should be fine for now. Consider adjusting based on the data that will be processed
* **Advanced Options - Users will need to set the Instance Profile! There should only be one option in the standard IDO deployment.**&#x20;

![An example cluster configuration](<../../.gitbook/assets/image (393) (1) (1).png>)

3\. Click Create Cluster at the top of the page. The cluster is now created and the user is automatically naviagated to the Cluster page! Next the SDK Libraries must be attached. From the Cluster Page, Click the Libraries tab, then click Install New.

![The LIbraries tab with the Install New button](<../../.gitbook/assets/image (396) (1) (1).png>)

4\. In the popup, click the DBFS/S3 button. For azure environments, fill in the value **dbfs:/mnt/datalake/dataops-sdk.jar** for AWS environments, fill out the value **s3://\<Environment>-datalake-\<Client>/dataops-sdk.jar** Where \<Environment> and \<Client> are values found by hovering over the Top left corner of the IDO UI (See the 2nd image below). Then click Install. Then restart the cluster by clicking the Restart button.&#x20;

![An Azure example](<../../.gitbook/assets/image (391) (1) (1).png>)

![The hover over menu for accessing Environment and Client values](<../../.gitbook/assets/image (404) (1).png>)

5\. The Cluster is ready to go. In the final step of this instruction, it will be attached to a Notebook to run code. Again click the Create button along the left-hand side of the Databricks UI. Then Click Notebook. In the resulting popup, the notebook can be named anything. For this example we will use CustomProcessNotebook. The langauge should be Scala and the Cluster should be the Cluster created in the above steps. Click Create to create the Notebook and to be redirected to the Notebook itself. You are now ready to run CustomProcess Notebooks in Databricks.

![The Create Notebook Dialog](<../../.gitbook/assets/image (402) (1) (1).png>)



The next pages contain simple HELLO WORLD examples for each of the 3 Custom Process types: Custom Ingest, Custom Parse, and Custom Post Output.
