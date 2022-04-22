# Custom Parsing

The Custom Parsing SDK will allow users to define their own file parsing code in a Databricks Notebook. It is intended to be used with files picked up by the Intellio Agent. This page is a "Hello World" for custom ingestion. A full Scaladoc detailing all of the code options availabe in the SDK can be found [here](https://docs.intellio.wmp.com/com/wmp/intellio/dataops/sdk/IngestionSession.html).

## Configuring a Custom Parsing Source

Any file type source in IDO can be made into a Custom Parse source.  For this example, we will be using a CSV file.&#x20;

In order to create a Custom Parsing source, users should first select a File Connection Type. After choosing to ingest a file, users will be able to choose which type of file they are ingesting. For the purpose of this example, we will use Delimited, but note that ANY file type can be ingested through the "Other" type. Just be sure to correctly specify the file extension in the file mask parameter.

Next, we will choose a parser. In this example we will choose Custom Notebook to use custom Databricks notebook code to parse our data. Note that for the "Other" file type only the Custom parsers are available.&#x20;

![](<../../.gitbook/assets/image (385) (1) (1) (1) (1) (1).png>)

Finally, select the Cluster Config pointing to the custom cluster configuration for the parser, learn more about setting up a Cluster Configuration for Custom Processes [here](../system-configuration/cluster-and-process-configuration-overview/cluster-configuration/cluster-configuration-for-custom-processing-steps.md).

## Creating your first Custom Parse Notebook

Before running a Custom Parse notebook manually, it is important to pull a file into the Source for Parsing. Hit the Pull Now button or drop a new file into a Watcher source to get a file into the Source. IDO will pull in the data with an Ingestion process, and then fail the Parsing process. You are now ready to attempt parsing the file.

### Creating your first Notebook

Below is a sample of notebook code that sets up a Custom Parsing session and reads the file picked up by the Agent. A line by line breakdown can be found below. The code below can be copy pasted into a Databricks notebook, but users will need to replace the _**`inputId` **_ with the input ID of an input from the Custom Parsing Source.

```
import com.wmp.intellio.dataops.sdk._
import org.apache.spark.sql.DataFrame

val session = new ParsingSession(<inputId>) 

def parseDf(): DataFrame = {
    val fileName: String = session.FilePath
    val df: DataFrame = spark.read.load(fileName)
    return df
}

session.ingest(parseDf)
```

#### Imports - Line 1-2

The first two lines are standard import statements. They are needed to utilize all of the DataOps SDK functionality. 90% of Custom Parsing Notebooks will have these.

#### Parsing Session - Line 4

The 4th line creates a new DataOps parsing session. When this line is run, a new process record will be created in DataOps to track the parsing process. It will also begin a heartbeat that constantly communicates with DataOps to ensure the job has not crashed.&#x20;

#### Creating The DataFrame Function - Line 6-10

The 6th-10th lines are the key part of the parsing where the custom user code will go. These lines define a function that returns a dataframe. In the example, the code will write get the filename for the input, read in the data from that file, and return it as a dataframe. In the future, replace the code within parseDf with custom code in order to run your custom code.

#### Executing The Parse - Line 12

The 12th line runs the custom parse. It parses the data as specified in the parseDf function, normalizes it, and sends it to the DataOps Datalake.

### Running the Custom Parse

To execute this Hello World Example. Simply run the Databricks cell with the code snippet. A progress bar will display at the bottom of the cell, and once it is finished a completed parsing process will be present in IDO for the Custom Source!







