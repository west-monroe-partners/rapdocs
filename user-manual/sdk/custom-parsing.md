# Custom Parsing

The Custom Parsing SDK will allow users to define their own file parsing code in a Databricks Notebook or a custom Jar. It is intended to be used with files picked up by the Intellio Agent. This page is a "Hello World" for custom ingestion. Full Scaladoc for the SDK can be found [here](https://docs.intellio.wmp.com/com/wmp/intellio/dataops/sdk/IngestionSession.html).

## Configuring a Custom Parsing Source

### Creating a Custom Source



Any file type source in IDO can be made into a Custom Parse source.  For this example, we will be using a CSV file. 

In order to create a Custom Parsing source, users should first select a File Connection Type. After choosing to ingest a file, users will be able to chose which type of file they are ingesting. For the purpose of this example, we will use Delimited, but note that ANY file type can be ingested through the "Other" type.

Finally, we will choose a parser. In this example we will choose Custom Notebook to use custom Databricks notebook code to parse our data. Note that for the "Other" file type only the Custom parsers are available. 

With Custom Notebook selected. We will now fill out the Notebook Path parameter to point toward our Databricks notebook.

![](../../.gitbook/assets/image%20%28373%29.png)



After selecting an initiation type, a location for the Notebook/JAR will need to be provided. If the user plans to run the notebook manually, as we will in this example, this can set to "N/A". In the future, the notebook path can be found in the Databricks workspace tab.



