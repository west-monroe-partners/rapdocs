---
description: >-
  The Custom Ingestion SDK will allow users to define their own data acquisition
  code in a Databricks Notebook or a custom Jar. By writing a single scala
  function that creates a dataframe of data, users
---

# Custom Ingestion

## Configuring a Custom Ingestion Source

### Creating a Custom Connection

Because DataOps cannot track all of the different potential connection types that can be used in user notebooks, a generic Custom Connection type has been created. The custom connection type will allow users to fill out two fields: Private and Public connection parameters. Public connection parameters will be stored unencrypted for easy access and editing. Private connection parameters will be encrypted and obfuscated when seen in the UI. Each of these parameters should be created as a set of key-value pairs, following standard JSON synatx. We will cover how these connections are accessed in user code as part of a later section.

![Custom Connection](../.gitbook/assets/image%20%28334%29.png)

### Creating a Custom Source

In order to create a Custom Ingestion source, users should use the Custom radio button in the source configuration page. After clicking the Custom button, useres will first be asked to select a connection from any of the preconfigured custom connections. Users will also have the option to select whether they want their code to be run as a Notebook or as a JAR. After selecting one, a location for the Notebook/JAR will need to be provided. If only running manually from the notebook, this can set to "N/A". All Custom Ingestion sources are setup as Scheduled sources by default.

![Custom Source Screen](../.gitbook/assets/image%20%28336%29.png)

### Setting up your cluster

### Creating your Notebook

Below is a sample of notebook code that sets up an ingestion session and then queries the DataOps datatypes table. We will break it down piece by piece.

_`import com.wmp.intellio.dataops.sdk._`_ 

_`import org.apache.spark.sql.DataFrame`_

_`import play.api.libs.json._`_

_`val session = new IngestionSession("<YourEnvironmentName>", "<YourSourceName>")`_ 

_`val customParams = session.customParameters`_ 

_`val connection = session.connectionParameters`_

_`def ingestDf(): DataFrame = {  
session.log("About to query the data.", "I")`_ 

_`spark.sql("SELECT * FROM datatypes") }`_

_`session.ingest(ingestDf)`_

#### Imports

The first three lines are standard import statements. They are needed to utilize all of the DataOps SDK functionality.

#### Ingestion Session

The 4th line creates a new DataOps ingestion session. When this line is run, a new input record and a new process record will be created in DataOps to track the ingetsion process. It will also begin a heartbeat that constantly communicates with DataOps to ensure the job has not crashed. 

#### Accessing Custom Parameters

The 5th line access the custom parameters created in the source configuration. It will be a JsObject. See documentation for Play JSON here: [https://www.playframework.com/documentation/2.8.x/ScalaJson](https://www.playframework.com/documentation/2.8.x/ScalaJson)

#### Accessing Connection Parameters

The 6th line accesses the connection parameters for the source's custom connection. It will be a JsObject. See documentation for Play JSON here: [https://www.playframework.com/documentation/2.8.x/ScalaJson](https://www.playframework.com/documentation/2.8.x/ScalaJson) This function can also be called with a connection ID or name in order to access connections besides the one configured on the source itself. 

#### Creating The DataFrame Function

The 7th-9th lines are the key part of the ingestion where the custom user code will go. These lines define a function that returns a dataframe. In the example, the code will write a log to DataOps, then run a spark query, returning a dataframe. Replace the code within ingestDf with custom code in order to run your code.

#### Executing The Ingestion

The 10th line runs the custom ingest. It pulls the data as specified in the ingestDf function, normalizes it, and sends it to the DataOps Datalake.

### Running the Custom Ingestion

The custom ingestion can be run in one of three ways

1. Execute the Notebook directly in Databricks. This will allow for fast troubleshooting and development iteration. You must attach the Maven repository to your cluster before running. The coordinates for the current Maven version of the SDK are com.wmp.intellio:dataops-sdk\_2.12:0.3.1
2. Execute a Pull Now on the Custom Source. This will automatically handle attaching the SDK code to your cluster.
3. Set a schedule for the Custom Source.

### Advanced Patterns

* Custom Parameters and custom connections can be used to allow a single notebook to connect to multiple different data sources. i.e. Create a generic SalesForce connector, then specify the table name in the custom parameters.
* Latest tracking fields are available for each session. They include latest timestamp, latest sequence, extract datetime, and input id. They can be accessed with session.latestTrackingFields

