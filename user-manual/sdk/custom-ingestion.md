# Custom Ingestion

The Custom Ingestion SDK will allow users to define their own data acquisition code in a Databricks Notebook or a custom Jar. This page is a "Hello World" for custom ingestion. Full Scaladoc for the SDK can be found [here](https://docs.intellio.wmp.com/com/wmp/intellio/dataops/sdk/IngestionSession.html).

## Configuring a Custom Ingestion Source

### Creating a Custom Connection

Because DataOps cannot track all of the different potential connection types that can be used in user notebooks, a generic Custom Connection type has been created. The custom connection type will allow users to fill out two fields: Private and Public connection parameters. Public connection parameters will be stored unencrypted for easy access and editing. Private connection parameters will be encrypted and obfuscated when seen in the UI. Each of these parameters should be created as a set of key-value pairs, following standard JSON syntax. We will cover how these connections are accessed in user code as part of a later section. **Dummy connection for now**

![Custom Connection](<../../.gitbook/assets/image (334).png>)

### Creating a Custom Source

In order to create a Custom Ingestion source, users should use the Custom radio button in the source configuration page. After clicking the Custom button, users will first be asked to select a connection from any of the preconfigured custom connections.&#x20;

Users will also need to select a Cluster Configuration to run the ingestion. Instructions for doing so can be found [here](../system-configuration/cluster-and-process-configuration-overview/cluster-configuration/cluster-configuration-for-custom-processing-steps.md).

All Custom Ingestion sources are setup as Scheduled sources by default.

![](<../../.gitbook/assets/image (381) (1).png>)



### Creating your first Notebook

Below is a sample of notebook code that sets up an ingestion session and then queries the DataOps datatypes table. A line by line breakdown can be found below.  Users will also need to replace the _**`<DataOpsSourceName>` **_ with the name of the associated custom DataOps source.

{% hint style="info" %}
_`First parameter,`**`<DataOpsEnvironmentName>`**` ``had been deprecated in release 2.5 and is no longer required.`_&#x20;
{% endhint %}

```
import com.wmp.intellio.dataops.sdk._
import org.apache.spark.sql.DataFrame

val session = new IngestionSession("<DataOpsEnvironmentName>", "<DataOpsSourceName>") 

def ingestDf(): DataFrame = {
    val values: List[Int] = List(1,2,3,4,5) 
    val df: DataFrame = values.toDF()
    return df
}

session.ingest(ingestDf)
```

#### Imports - Line 1-2

The first three lines are standard import statements. They are needed to utilize all of the DataOps SDK functionality.

#### Ingestion Session - Line 4

The 4th line creates a new DataOps ingestion session. When this line is run, a new input record and a new process record will be created in DataOps to track the ingetsion process. It will also begin a heartbeat that constantly communicates with DataOps to ensure the job has not crashed.&#x20;



#### Creating The DataFrame Function - Line 6-10

The 7th-9th lines are the key part of the ingestion where the custom user code will go. These lines define a function that returns a dataframe. In the example, the code will write a log to DataOps, then run a spark query, returning a dataframe. Replace the code within ingestDf with custom code in order to run your custom code.

#### Executing The Ingestion - Line 12

The 10th line runs the custom ingest. It pulls the data as specified in the ingestDf function, normalizes it, and sends it to the DataOps Datalake.

### Running the Custom Ingestion

The custom ingestion can be run in one of three ways

1. Execute the Notebook directly in Databricks. This will allow for fast troubleshooting and development iteration. You must attach the SDK jar to your cluster before running. The coordinates for the current version of the SDK are S3://\<YourDatalakeBucket>/dataops-sdk.jar for AWS or TBD for Azure
2. Execute a Pull Now on the Custom Source. This will automatically handle attaching the SDK code to your cluster.
3. Set a schedule for the Custom Source.

### Advanced Patterns

* Custom Parameters and custom connections can be used to allow a single notebook to connect to multiple different data sources. i.e. Create a generic SalesForce connector, then specify the table name in the custom parameters.
* Latest tracking fields are available for each session. They include latest timestamp, latest sequence, extract datetime, and input id. They can be accessed with session.latestTrackingFields.{sTimestamp, sSequence, extractDatetime, inputId}
