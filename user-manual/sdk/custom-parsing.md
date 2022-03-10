# Custom Parsing

The Custom Parsing SDK will allow users to define their own file parsing code in a Databricks Notebook or a custom Jar. It is intended to be used with files picked up by the Intellio Agent. This page is a "Hello World" for custom ingestion. Full Scaladoc for the SDK can be found [here](https://docs.intellio.wmp.com/com/wmp/intellio/dataops/sdk/IngestionSession.html).

## Configuring a Custom Parsing Source

Any file type source in IDO can be made into a Custom Parse source.  For this example, we will be using a CSV file.&#x20;

In order to create a Custom Parsing source, users should first select a File Connection Type. After choosing to ingest a file, users will be able to choose which type of file they are ingesting. For the purpose of this example, we will use Delimited, but note that ANY file type can be ingested through the "Other" type. Just be sure to correctly specify the file extension in the file mask parameter.

Next, we will choose a parser. In this example we will choose Custom Notebook to use custom Databricks notebook code to parse our data. Note that for the "Other" file type only the Custom parsers are available.&#x20;

![](<../../.gitbook/assets/image (385) (1).png>)

Finally, select the Cluster Config pointing to the custom cluster configuration for the parser, which will have a link to the notebook path.

