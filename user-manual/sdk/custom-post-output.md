# Custom Post Output

The Custom Post Output SDK will allow users to define code in a Databricks Notebook or a custom Jar that will be run after the output process. This page is a "Hello World" for custom post output. Full Scaladoc for the SDK can be found [here](https://docs.intellio.wmp.com/com/wmp/intellio/dataops/sdk/PostOutputSession.html).&#x20;

## Creating a Custom Post Output&#x20;

Any output can have post output commands attached to it. Simply select whether the source should run its post output commands via a Notebook or a Jar in the Custom Post Output section.&#x20;

Users will have the option to select whether they want their code to be run as a Notebook or as a JAR. For this demo we will use a notebook.&#x20;

After selecting an initiation type, a location for the Notebook/JAR will need to be provided. If the user plans to run the notebook manually, as we will in this example, this can set to "N/A". In the future, the notebook path can be found in the Databricks workspace tab.&#x20;

### Specifying a Custom Cluster

Cluster configuration is a very important part of ensuring reliable execution of your custom job. Please refer to this[ ](custom-post-output.md#creating-a-custom-post-output) [page ](custom-post-output.md#creating-a-custom-post-output)for a detailed overview of options.

{% hint style="danger" %}
Specifying interactive clusters for job execution is not supported - especially the existing mini-sparky cluster that DataOps uses for query validation and Data Viewer.

While potentially viable in specific situations, DataOps is not responsible for state clearing and results controls. Developers must handle these within their notebooks to guarantee reliable results and stability.&#x20;
{% endhint %}

### Creating your first Notebook

Below is a sample of notebook code that sets up a post output session and then prints a simple Hello World.  A line by line breakdown can be found below.&#x20;

{% hint style="info" %}
_`First parameter,`**`<DataOpsEnvironmentName>`**` ``had been deprecated in release 2.5 and is no longer required.`_&#x20;
{% endhint %}

&#x20;Users will need to replace the _**`<DataOpsOutputName> and <DataOpsOutputChannelName>` **_ with the name of the associated custom DataOps output and output channel respectively.

```
import com.wmp.intellio.dataops.sdk._

val session = new PostOutputSession("<DataOpsEnvironmentName>", "<DataOpsOutputName>", "<DataOpsOutputSourceName>") 

def helloWorld(): Unit = {
log("Hello World!")
println("Hello World!")
}

session.run(helloWorld)
```

#### Import - Line 1

The first line is a standard import statement. It is needed to utilize all of the DataOps SDK functionality.

#### Post Output Session - Line 3&#x20;

The 3rd line creates a new DataOps Post Output session. When this line is run, a new process record will be created in DataOps to track the process. It will also begin a heartbeat that constantly communicates with DataOps to ensure the job has not crashed.



#### Creating The helloWorld Function - Line 5-7

The 5th-7th lines are the key part of the post output where the custom user code will go. These lines define a function that is of Unit type. In the example, the code will write a log to DataOps, then print Hello World as well. Replace the code within helloWorld with custom code in order to run your custom code.

#### Executing The Custom Post Output - Line 10

The 10th line runs the custom post output. It pulls the data as specified in the helloWorld function.

### Running the Custom Post Output

The custom post output can be run in one of three ways

1. Execute the Notebook directly in Databricks. This will allow for fast troubleshooting and development iteration. You must attach the SDK jar to your cluster before running. The coordinates for the current version of the SDK are S3:///dataops-sdk.jar for AWS or TBD for Azure&#x20;
2. Execute a Reset Output on the source connected to the output. This will automatically handle attaching the SDK code to your cluster.&#x20;
3. Pull a new input for the attached source. It will automatically run post output after output finishes.

### Advanced Patterns

* Custom Parameters and custom connections can be used to allow a single notebook to connect to multiple different data sources. i.e. Create a generic SalesForce connector, then specify the table name in the custom parameters.&#x20;
* Custom ingest sessions can be embedded within custom post output sessions to create a loopback. See the example below:

```
import com.wmp.intellio.dataops.sdk._
import play.api.libs.json._
import org.apache.spark.sql.DataFrame

//Create Post output session
val postOutputSession = new PostOutputSession("development","POstOutput","Demo Company 1 SalesOrderDetail")

//Post output logic
def postOutputCode(): Unit = {
  //spark.sql("CREATE OR REPLACE VIEW customPostOutput AS SELECT * FROM development.hub_1")
  
  //Ingestion Session to loopback source
  val ingestSession = new IngestionSession("development", "CustomIngestTest")
  
  //Ingestion Logic
  def ingestionCode(): DataFrame = {
    spark.sql("SELECT * FROM customPostOutput")
  }
  //Run ingest
  ingestSession.ingest(ingestionCode)
}

//Run post output code
postOutputSession.run(postOutputCode)
```
