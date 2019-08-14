---
description: >-
  The Details tab for an Output allows a user to specify key information about
  the Output including the Connection to use and the Output Type.
---

# Output Details

## Details Tab

In the Edit Output screen, users can see the various components that make up an Output, including tabs for Output Details, Mappings, Manual Output, and Output History. When initially configuring an Output, this is the only visible tab.

![Output Details Tab](../../.gitbook/assets/image%20%28120%29.png)

## Initial Parameters

* **Name** - The name of the Output. This will be displayed on the Outputs screen when browsing Outputs. To ensure Outputs are organized easily searchable, follow the [Naming Conventions](../../common-use-cases/naming-convention.md#outputs).
* **Description** - The description of the Output.
* **Active** - If set to Active, the Output will be immediately available for use.

### Output Type

It is important to decide which Output Type an Output is. There are four main types described below. The parameters available will dynamically change depending on users' selections.

{% tabs %}
{% tab title="File" %}
RAP will output to a **File** using a [File Connection](../connections-configuration.md#file). 

There are two Output File Types: **CSV** or **Parquet**. Parameter selections will update dynamically depending on the selection.

* A **Delimited** file is a plain-text comma-separated file. Common and easy to import, not very suitable for large files or complex data.
* A  **Parquet** file uses a free and open-source column-oriented data storage format of the Apache Hadoop ecosystem.
{% endtab %}

{% tab title="Table" %}
RAP can output and refresh data to a database **Table** using a [Table Connection](../connections-configuration.md#table).

There are four table output drivers: Snowflake, SQL Server, Postgres, and Redshift.
{% endtab %}

{% tab title="SFTP" %}
RAP can send files via **SFTP** connection in a CSV format. SFTP \(SSH File Transfer Protocol\) is a file protocol used to access files over an encrypted SSH transport.
{% endtab %}

{% tab title="Virtual" %}
RAP can output data to a database view, otherwise known as a **Virtual** table.
{% endtab %}
{% endtabs %}

## Output Parameters

{% hint style="info" %}
Asterisks \(\*\) mean the Parameter is mandatory and must be changed by users.
{% endhint %}

### Output

| Filter Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| Table, File, SFTP | connection\_name\* |  | Connection name for the destination | N |
| Virtual | view\_schema\* |  | Schema name for Output view | N |
| File Type: Parquet | file\_mask | `FileName<TSHH12MISS>.PARQUET` | file mask for output file | N |
| SFTP, File Type: CSV | file\_mask | `FileName<TSHH12MISS>.csv` | file mask for output file | N |
| Virtual | view\_name\* |  | Name for Output view | N |
| Virtual | effective\_filter | effective\_range | Effective filter for Time Series Output Sources | N |
| Virtual, Table, File | key\_history | FALSE | Output key history for Key Output Sources - ignore for Time Series sources | N |
| File, SFTP | partition | segment | Partitioning strategy for files {input, segment} | N |
| File, SFTP | limit\_by\_effective\_range | FALSE | Output uses effective range calculations to limit data for Time Series sources | N |
| Table | delete | none | Choice of how we handle the output data into the destination - Options: none, all, input, key, range | N |
| SFTP | local\_path\* |  | Local path to write files to before SFTP transfer begins | N |
| SFTP | output\_path\* |  | SFTP destination path | N |
| Table | table\_name\* |  | Table name for destination table | N |
| Table | table\_schema\* |  | Schema name for destination table | N |
| Driver: Redshift, Snowflake | temp\_file\_output\_location |  | Sets the location of the temp file used for Output processing. | N |
| Table | batch\_size | 1048000 | Batch size for Output loading | Y |
| SFTP, File Type: CSV | column\_delimiter | , | column delimiter character | Y |
| SFTP, File Type: CSV | file\_type | delimited | options: delimited | Y |
| SFTP, File Type: CSV | text\_qualifier |  | text qualifier character for delimited files | Y |
| Table | delete\_batch\_size | 1000 | RAP performs pre-load truncations in batches - this sets the batch size | Y |
| Driver: Redshift | delete\_temp\_file | TRUE | 0/1 flag for whether or not to delete the temporary file used for Output processing upon completion of phase | Y |
| Table | manage\_table | TRUE | True or false, decides if you want table to be altered if there are missing columns | Y |
| File, SFTP, Table | postgres\_concurrency | 1 | Thread count on Postgres side | Y |
| SFTP, File Type: CSV | line\_terminator | \n | Line endings for csv records | Y |
| Driver: SQL Server | sql\_server\_concurrency | 1 | Thread count on SQL Server side | Y |
| Table, File, SFTP | fetch\_size | 5000 | Fetch size for result sets from Postgres | Y |
| SFTP, File Type: CSV | compression | none | Compression for file outputs {none,gzip} | Y |
| Table, File, SFTP | allow\_output\_regeneration | TRUE | If set to false, the output will not be generated if triggered by an output reset or validation reset | Y |
| Driver: SQL Server | create\_cci\_on\_table | TRUE | create a clustered columnstore index on the table \(only applies to outputs with time series data\) | Y |

### Output Retention

| Process Type | Filter Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- | :--- |
| CSV, Parquet | File | archive\_files | 1 year | interval to retain file in archive | Y |
| CSV, Parquet | File | buffer\_files | 0 | interval to retain files in fast output buffer storage | Y |
| CSV, SFTP, Parquet | File, SFTP | temporary\_files | 14 days | interval to retain temporary files | Y |

