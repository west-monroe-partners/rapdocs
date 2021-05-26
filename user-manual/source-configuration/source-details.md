---
description: >-
  The Details tab for a Source allows a user to specify key information about
  the Source including input types and file types.
---

# Settings

## Settings Tab

When creating a new Source, only the Settings tab is available. 

The Setting tab enables user to configure parameters that apply to the entire source across all Inputs and Processes associated with the Source.

Most of the parameters focus on where, how, and when to ingest data into the DataOps managed data lake, how to refresh data, how to track that information over time, as well as any infrastructure related configuration or tuning to help manage performance and cost of processing this dataset.

After the Source is created you can access the Settings tab at any time by clicking on the Settings tab in the upper left.

![](../../.gitbook/assets/image%20%28316%29.png)

## Base Parameters

{% hint style="info" %}
Asterisks \(\*\) mean the Parameter is mandatory and must be specified by users.
{% endhint %}

* **Name\*:** The name of the Source. The Name must be unique. This will be displayed on the Sources screen when browsing Sources. To ensure Sources are organized easily searchable, follow [Naming Conventions](https://intellio.gitbook.io/dataops/v/master/best-practices/naming-conventions).
* **Description\*:** The description of the Source.
* **Hub View Name:**  Name of view alias of the raw hub table in Databricks
* **Group Name:** Used as part of [Templates and Tokens](../validation-and-enrichment-rule-templates/)
* **Source Name Template:** Used as part of [Templates and Tokens](../validation-and-enrichment-rule-templates/)
* **Active\*:** If set to Active, the Source will run as specified.
* **Connection Type\*:** Selector to help filter the [Connection ](../connections.md)dropdown
* **Connection\*:** The [Connection ](../connections.md)to use for this source

### Connection Type Specific Parameters

{% tabs %}
{% tab title="Custom" %}
* **Initiation Type\*:** Specifies if your custom code is compiled into an executable Jar file, or is in a Databricks Notebook

#### Jar

* **Jar Location\*:** Specifies the location in the Cloud storage technology where the Jar is stored
* **Main Class Name\*:** Name of the primary class in the Jar to be run when executed

#### Notebook

* **Notebook Path**\***:** Path to the notebook within Databricks.
  * eg. /Shared/mynotebookname
{% endtab %}

{% tab title="File" %}
* **File Mask\*:** Name of file within the connection folder/path.
  * Glob syntax supported: eg. myfile\*.csv
* **File Type\*:** Serialization format of the file
* **Parser\*:** DataOps has two supported parsers for certain File Types
  * Spark: Native Spark libraries or extensions used
  * Core: DataOps custom Akka streams parser for delimited files with advanced error handling and malformed file debugging
{% endtab %}

{% tab title="Loopback" %}
* **Virtual Output\*:** Name of the virtual output this source is linked to and will pull data from
{% endtab %}

{% tab title="SFTP" %}
**SFTP**, or Secure File Transfer Protocol, is a method of transferring files between machines over a secure connection.

**See File tab for detailed parameters overview**
{% endtab %}

{% tab title="Table" %}
* **Source Query\*:** Query to run against the connection database.
  * There are tokens available to assist with filtering data on ingest to only records updated or inserted that are not already in DataOps
    * &lt;extract\_datetime&gt;
      * This will be replaced with the timestamp of the last successful ingestion process
    * &lt;latest\_sequence&gt;
      * This will be replaced with MAX\(s\_sequence\) from all inputs previously processed
    * &lt;latest\_timestamp&gt;
      * This will be replaced with MAX\(s\_update\_timestamp\) for Keyed or MAX\(s\_timestamp\) for Timeseries refresh types over all inputs previously processed
{% endtab %}
{% endtabs %}

## Data Refresh Types

A Data Refresh Type specifies how DataOps should handle processing, refreshing, and storing the data. The five types are described below. The parameters available will dynamically change depending on the user's selection.

{% tabs %}
{% tab title="Full" %}
**Full** sources assume each batch of data contains the most recent version of all data for all history. It is a full truncate and reload style refresh.

Full refresh is the most simple and can process any data. It's often useful to start with Full refresh if you do not yet know which more specific and performant alternative to use.
{% endtab %}

{% tab title="Key" %}
Sources with the **Key** refresh type contain a unique identifier or _key_ tied to a logical entity.

Key refresh is often the most convenient logically, but has performance trade-offs at scale when compared to None, Sequence, or Timestamp. If possible, those alternatives are preferred, but not always logically possible.
{% endtab %}

{% tab title="Timestamp" %}
**Timestamp** sources identify changes in data using a column that contains the date and/or time for each record. This is most commonly used with Event or IOT data that is written once and only once and has a monotonically increasing timestamp tracking field.

It is also often useful for performance optimization vs Keyed if the source data has a defined period where records can be updated, after which they are guaranteed to be static, such as in monthly finance and accounting datasets.
{% endtab %}

{% tab title="Sequence" %}
**Sequence** sources identify changes in data using a column that contains a monotonically increasing ID tracking field and follows a write-once pattern.

It is also often useful for performance optimization vs Keyed if the source data has a defined range where records can be updated, after which they are guaranteed to be static.
{% endtab %}

{% tab title="None" %}
**None** is used when it can be assumed that all data from new Inputs can be considered New.

This is useful for datasets that have an upstream CDC process and can guarantee once-and-only-once delivery of Data to DataOps.

This is the most performant Refresh type, as CDC and the expensive portions of Refresh are skipped. 
{% endtab %}
{% endtabs %}

The diagram below can be used as base guidance for which Refresh Type to select for what types of source datasets.

Data Refresh selection is one of the most important decisions for design, and while this diagram provides base guidance, careful consideration and understanding of how the data is generated and will be used is required to make the correct decision.

![](../../.gitbook/assets/image%20%28348%29.png)

## Initiation Type

This determines how new Inputs are generated within DataOps. Most Connections Types only currently support [Scheduled](../schedules.md) pulls of data from the Connection, however File Connections include a watcher feature that will automatically begin processing any new files moved or generated in the Connection folder that also match the configured File Mask.

With Custom Connection Type Sources utilizing the [SDK](../sdk/), it is possible to have a source be both scheduled and/or initialized from outside of DataOps utilizing the methods within the SDK and the Databricks APIs, providing maximum flexibility for any custom integration with 3rd party tools or in-house built applications 

## Cluster Type

* **Custom:** Allows you to specify the infrastructure used by DataOps to process the data. You are able to specify any parameters available in the "existing\_cluster\_id OR new\_cluster" and "libraries" of the [Databricks Jobs API](https://docs.databricks.com/dev-tools/api/latest/jobs.html).
  * These parameters should be structured as a single JSON object with the top level keys set as the relevant Field Names under the API docs from Databricks listed above.
  * Here are some examples:

```text
{
"new_cluster": {
  "spark_version": "7.3.x-scala2.12",
  "node_type_id": "r3.xlarge",
    "aws_attributes": {
      "availability": "ON_DEMAND"
    },
    "num_workers": 10
  },
  "libraries": [
    {
      "jar": "dbfs:/my-jar.jar"
    },
    {
      "maven": {
        "coordinates": "org.jsoup:jsoup:1.7.2"
      }
    }
  ]       
}
```

```text
{
    "existing_cluster_id":"0512-152727-third1"
}
```

* **DataOps Managed:** This setting with use the default cluster configuration for the environment, optionally tune-able via the Performance & Cost detailed parameters section.

## Advanced Parameters

Depending on the selections made in the required parameters section, the advanced parameters section will provide various sub-settings to help configurators tune the jobs to their needs. Descriptions for each are included in the UI. Please submit a support ticket if the descriptions in the UI do not adequately explain the functionality of a specific parameter.

