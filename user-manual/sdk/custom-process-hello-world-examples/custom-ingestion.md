# Custom Ingestion

The Custom Ingestion SDK will allow users to define their own data acquisition code in a Databricks Notebook. This page is a "Hello World" for custom ingestion. A full Scaladoc detailing all of the code options availabe in the SDK can be found [here](https://docs.intellio.wmp.com/com/wmp/intellio/dataops/sdk/IngestionSession.html).

## Configuring a Custom Ingestion Source

### Creating a Custom Source

In order to create a Custom Ingestion source, users should use the Custom radio button in the source configuration page. Users will also need to select a Cluster Configuration to run the ingestion. Instructions for doing so can be found [here](../../system-configuration/cluster-and-process-configuration-overview/cluster-configuration/cluster-configuration-for-custom-processing-steps.md).

All Custom Ingestion sources are setup as Scheduled sources by default.

![](<../../../.gitbook/assets/image (381) (1).png>)



### Creating your first Notebook

Below is a sample of notebook code that sets up an ingestion session and then returns a dataframe of the numbers 1 through 5. A line by line breakdown can be found below. The code below can be copy pasted into a Databricks notebook, but users will need to replace the _**`<DataOpsSourceName>` **_ with the name of the Custom DataOps Source created in the step above.

```
import com.wmp.intellio.dataops.sdk._
import org.apache.spark.sql.DataFrame

val session = new IngestionSession("<DataOpsSourceName>") 

def ingestDf(): DataFrame = {
    val values: List[Int] = List(1,2,3,4,5) 
    val df: DataFrame = values.toDF()
    return df
}

session.ingest(ingestDf)
```

#### Imports - Line 1-2

The first two lines are standard import statements. They are needed to utilize all of the DataOps SDK functionality. 90% of Custom Ingest Notebooks will have these.

#### Ingestion Session - Line 4

The 4th line creates a new DataOps ingestion session. When this line is run, a new input record and a new process record will be created in DataOps to track the ingetsion process. It will also begin a heartbeat that constantly communicates with DataOps to ensure the job has not crashed.&#x20;

#### Creating The DataFrame Function - Line 6-10

The 6th-10th lines are the key part of the ingestion where the custom user code will go. These lines define a function that returns a dataframe. In the example the code will create a dataframe of numbers 1 through 5, returning a dataframe. In the future, replace the code within ingestDf with custom code in order to run your custom code.

#### Executing The Ingestion - Line 12

The 12th line runs the custom ingest. It pulls the data as specified in the ingestDf function, normalizes it, and sends it to the DataOps Datalake.

### Running the Custom Ingestion

To execute this Hello World Example. Simply run the Databricks cell with the code snippet. A progress bar will display at the bottom of the cell, and once it is finished a brand new input will be present in IDO for the Custom Source!

## Using Custom Code

In order to run your own Custom code, keep in mind that the ingestDf method in the example above MUST return a Dataframe. Replace lines 7-9 with your custom code that returns a Dataframe to send that data to IDO.



