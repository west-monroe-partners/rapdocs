# Output Settings

In the Output Settings screen, users can see the various components that make up an Output, including tabs for Output Settings, Mappings, and Output History. When initially configuring an Output, this is the only visible tab.

![](<../../.gitbook/assets/image (308).png>)

## Initial Parameters

* **Name:** The name of the Output. Every output in the DataOps environment must have a unique name.
* **Description:** The description of the Output.
* **Active:** If set to Active, the Output will be immediately available for use, and any sources connected to the output through a Channel will automatically run Output at the end of all processes.

### Output Type

It is important to decide which Output Type an Output is. There are four main types described below. The parameters available will dynamically change depending on users' selections.

{% tabs %}
{% tab title="File" %}
DataOps will output a **File** using a File Connection.

There are five Output File Types: **Avro**, **CSV**, **JSON**, **Parquet**, or **Text**. Parameter selections will update dynamically depending on the selection.
{% endtab %}

{% tab title="Table" %}
DataOps can output and refresh data to a database **Table** using a Table Connection.

There are three table output drivers: Snowflake, SQL Server, and Postgres.
{% endtab %}

{% tab title="Virtual" %}
Rather than push data out to a separate system, DataOps can also manage a database view on top of the Hub tables within the connected Databricks environment, otherwise known as a **Virtual Output**.
{% endtab %}
{% endtabs %}

## Output Parameters

{% hint style="info" %}
Asterisks (\*) in the Parameter Name column indicate mandatory parameters
{% endhint %}

### Output

| Appears Under                       | Parameter Name           |   Default Value  | Description                                                                                                                    |
| ----------------------------------- | ------------------------ | :--------------: | ------------------------------------------------------------------------------------------------------------------------------ |
| Table, File                         | Connection\*             |                  | Name of the DataOps connection used to write the output to the desired destination                                             |
| Table                               | Table Name\*             |                  | Name of output table to be written in the target DB                                                                            |
| Table                               | Table Schema\*           |                  | Name of the schema the output table will be written to in the target DB.                                                       |
| Virtual                             | View Name\*              |                  | Name of the Virtual Table/View that will appear in the Databricks environment once the output is processed.                    |
| File Type: All                      | file\_name               |        ``        | File name for output file. For all except text, extension will be added automatically.                                         |
| Column Delimiter                    | ,                        |                  | Column delimiter character for Output File                                                                                     |
| File Type: CSV                      | compression              |       none       | Compression for file outputs. Options: none, gzip                                                                              |
| Virtual                             | effective\_filter        | effective\_range | Effective filter for Time Series Output Sources                                                                                |
| File Type: Text                     | file\_extension\*        |                  | Extension that will be appended to the file name when DataOps writes text file outputs                                         |
| File Type: All                      | Single File              |       TRUE       | Toggle for whether or not the output should be written to multiple files or one single file. Multiple files is more performant |
| File Type: Avro, CSV, JSON, Parquet | Limit By Effective Range |       FALSE      | Output uses effective range calculations to limit data for Time Series sources                                                 |
| Line Terminator                     | \n                       |                  | Line endings for csv records                                                                                                   |
| Table                               | Manage Table             |       TRUE       | Decides if you want table to be altered if there are missing columns, or automatically updated if new columns are added        |
| File: CSV                           | Text Qualifier           |         "        | text qualifier character for delimited files                                                                                   |

### Output Retention

| Appears Under           | Parameter        | Default Value | Description                                            |
| ----------------------- | ---------------- | ------------- | ------------------------------------------------------ |
| File Type: CSV, Parquet | archive\_files   | 1 year        | How long files will remain in archive folder.          |
| File Type: CSV, Parquet | buffer\_files    | 0             | Interval to retain files in fast output buffer storage |
| File Type: CSV, Parquet | temporary\_files | 14 days       | Amount of time temporary files will remain stored      |

### Output Alerts (AWS ONLY)

| Appears Under | Parameter             | Default Value | Description                                                                           |
| ------------- | --------------------- | ------------- | ------------------------------------------------------------------------------------- |
| All           | Output Failure Topics |               | List of AWS Simple Notification Service topic ARNs to be notified upon Output Failure |
| All           | Output Success Topics |               | List of AWS Simple Notification Service topic ARNs to be notified upon Output Success |

### Post Output Commands \*

When Custom Notebook value is selected, Custom Cluster Configuration for the Post-Output step needs to be [selected](../system-configuration/cluster-and-process-configuration-overview/cluster-configuration/cluster-configuration-for-custom-processing-steps.md#configure-cluster-for-custom-post-output).
