---
description: >-
  The Details tab for an Output allows a user to specify key information about
  the Output including the Connection to use and the Output Type.
---

# !! Output Details

## Details Tab

In the Edit Output screen, users can see the various components that make up an Output, including tabs for Output Details, Mappings, Manual Output, and Output History. When initially configuring an Output, this is the only visible tab.

![](../../.gitbook/assets/image%20%28308%29.png)

## Initial Parameters

* **Name:** The name of the Output. This will be displayed on the Outputs screen when browsing Outputs. To ensure Outputs are organized easily searchable, follow the [Naming Conventions](output-details.md).
* **Description:** The description of the Output.
* **Active:** If set to Active, the Output will be immediately available for use.

### Output Type

It is important to decide which Output Type an Output is. There are four main types described below. The parameters available will dynamically change depending on users' selections.

{% tabs %}
{% tab title="File" %}
RAP will output to a **File** using a [File Connection](../connections-configuration.md#file).

There are five Output File Types: **Avro**, **CSV**, **JSON**, **Parquet**, or **Text**. Parameter selections will update dynamically depending on the selection.

* An **Avro** file is a plain-text comma-separated file. Common and easy to import, not very suitable for large files or complex data.
* A **CSV** file is a plain-text comma-separated file. Common and easy to import, not very suitable for large files or complex data.
* A **JSON** file.
* A **Parquet** file.
* A **Text**.
{% endtab %}

{% tab title="Table" %}
RAP can output and refresh data to a database **Table** using a [Table Connection](../connections-configuration.md#table).

There are three table output drivers: Snowflake, SQL Server, and Postgres.
{% endtab %}

{% tab title="Virtual" %}
DataOps can output data to a database view in the connected Databricks environment, otherwise known as a **Virtual** table.
{% endtab %}
{% endtabs %}

## Output Parameters

{% hint style="info" %}
Asterisks \(\*\) in the Parameter Name column mean the Parameter is mandatory and must be changed by users.
{% endhint %}

### Output

<table>
  <thead>
    <tr>
      <th style="text-align:left">Appears Under</th>
      <th style="text-align:left">Parameter Name</th>
      <th style="text-align:center">Default Value</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:center">Advanced</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Table, File, SFTP</td>
      <td style="text-align:left">connection_name*</td>
      <td style="text-align:center"></td>
      <td style="text-align:left">Connection name for the destination</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">Virtual</td>
      <td style="text-align:left">view_schema*</td>
      <td style="text-align:center"></td>
      <td style="text-align:left">Schema name for Output view</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">File Type: Parquet</td>
      <td style="text-align:left">file_mask</td>
      <td style="text-align:center">
        <p><code>FileName</code>
        </p>
        <p><code>&lt;TSHH12MISS&gt;.PARQUET</code>
        </p>
      </td>
      <td style="text-align:left">File mask for output file</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">SFTP, File Type: CSV</td>
      <td style="text-align:left">file_mask</td>
      <td style="text-align:center">
        <p><code>FileName</code>
        </p>
        <p><code>&lt;TSHH12MISS&gt;.csv</code>
        </p>
      </td>
      <td style="text-align:left">File mask for output file</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">Virtual</td>
      <td style="text-align:left">view_name*</td>
      <td style="text-align:center"></td>
      <td style="text-align:left">Name for Output view</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">Virtual</td>
      <td style="text-align:left">effective_filter</td>
      <td style="text-align:center">effective_range</td>
      <td style="text-align:left">Effective filter for Time Series Output Sources</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">Virtual, Table, File</td>
      <td style="text-align:left">key_history</td>
      <td style="text-align:center">FALSE</td>
      <td style="text-align:left">Output key history for Key Output Sources - ignore for Time Series sources</td>
      <td
      style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">File, SFTP</td>
      <td style="text-align:left">partition</td>
      <td style="text-align:center">segment</td>
      <td style="text-align:left">Partitioning strategy for files Options: input, segment</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">File, SFTP</td>
      <td style="text-align:left">
        <p>limit_by</p>
        <p>_effective_range</p>
      </td>
      <td style="text-align:center">FALSE</td>
      <td style="text-align:left">Output uses effective range calculations to limit data for Time Series
        sources</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">Table</td>
      <td style="text-align:left">delete</td>
      <td style="text-align:center">none</td>
      <td style="text-align:left">Choice of how we handle the output data into the destination - Options:
        none, all, input, key, range</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">Table</td>
      <td style="text-align:left">table_name*</td>
      <td style="text-align:center"></td>
      <td style="text-align:left">Table name for destination table</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">Table</td>
      <td style="text-align:left">table_schema*</td>
      <td style="text-align:center"></td>
      <td style="text-align:left">Schema name for destination table</td>
      <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">Driver: Snowflake</td>
      <td style="text-align:left">
        <p>temp_file</p>
        <p>_output_location</p>
      </td>
      <td style="text-align:center"></td>
      <td style="text-align:left">Sets the location of the temp file used for Output processing.</td>
      <td
      style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">Table</td>
      <td style="text-align:left">batch_size</td>
      <td style="text-align:center">1048000</td>
      <td style="text-align:left">Batch size for Output loading</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">File Type: CSV</td>
      <td style="text-align:left">column_delimiter</td>
      <td style="text-align:center">,</td>
      <td style="text-align:left">Column delimiter character for Output File</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">File Type: CSV</td>
      <td style="text-align:left">file_type</td>
      <td style="text-align:center">delimited</td>
      <td style="text-align:left">Options: delimited</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">File Type: CSV</td>
      <td style="text-align:left">text_qualifier</td>
      <td style="text-align:center"></td>
      <td style="text-align:left">Text qualifier character for delimited files</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">Table</td>
      <td style="text-align:left">delete_batch_size</td>
      <td style="text-align:center">1000</td>
      <td style="text-align:left">When deleting data from a destination table prior to loading for refresh,
        how many records to delete at once</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">Table</td>
      <td style="text-align:left">manage_table</td>
      <td style="text-align:center">TRUE</td>
      <td style="text-align:left">True or false, decides if you want table to be altered if there are missing
        columns</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">File, Table</td>
      <td style="text-align:left">postgres_concurrency</td>
      <td style="text-align:center">1</td>
      <td style="text-align:left">Thread count when reading data from Postgres</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">File Type: CSV</td>
      <td style="text-align:left">line_terminator</td>
      <td style="text-align:center">\n</td>
      <td style="text-align:left">Line endings for csv records</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">Driver: SQL Server</td>
      <td style="text-align:left">
        <p>sql_server</p>
        <p>_concurrency</p>
      </td>
      <td style="text-align:center">1</td>
      <td style="text-align:left">Thread count on SQL Server side</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">Table, File</td>
      <td style="text-align:left">fetch_size</td>
      <td style="text-align:center">5000</td>
      <td style="text-align:left">Fetch size for result sets from Postgres</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">File Type: CSV</td>
      <td style="text-align:left">compression</td>
      <td style="text-align:center">none</td>
      <td style="text-align:left">Compression for file outputs. Options: none, gzip</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">Table, File</td>
      <td style="text-align:left">
        <p>allow_output</p>
        <p>_regeneration</p>
      </td>
      <td style="text-align:center">TRUE</td>
      <td style="text-align:left">If set to false, the output will not be generated if triggered by an output
        reset or validation reset</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">Driver: SQL Server</td>
      <td style="text-align:left">create_cci_on_table</td>
      <td style="text-align:center">TRUE</td>
      <td style="text-align:left">Create a clustered columnstore index maintenance job on the table (only
        applies to outputs with time series data)</td>
      <td style="text-align:center">Y</td>
    </tr>
  </tbody>
</table>

\#\#\# Output Retention

<table>
  <thead>
    <tr>
      <th style="text-align:left">Process Type</th>
      <th style="text-align:left">Filter Appears Under</th>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Default Value</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">CSV, Parquet</td>
      <td style="text-align:left">File</td>
      <td style="text-align:left">archive_files</td>
      <td style="text-align:left">1 year</td>
      <td style="text-align:left">How long to retain files in the Output Archive S3 folder</td>
    </tr>
    <tr>
      <td style="text-align:left">CSV, Parquet</td>
      <td style="text-align:left">File</td>
      <td style="text-align:left">buffer_files</td>
      <td style="text-align:left">0</td>
      <td style="text-align:left">
        <p>How long to retain files in the Output Archive S3 folder</p>
        <p><em>(Legacy Compatability)</em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">CSV, Parquet</td>
      <td style="text-align:left">File</td>
      <td style="text-align:left">temporary_files</td>
      <td style="text-align:left">14 days</td>
      <td style="text-align:left">How long to retain temporary files in S3</td>
    </tr>
  </tbody>
</table>

