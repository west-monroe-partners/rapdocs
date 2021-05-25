---
description: >-
  The Details tab for a Source allows a user to specify key information about
  the Source including input types and file types.
---

# !! Details

## Settings Tab

When creating a new Source, only the Settings tab is available. Users configure both the Input and Staging processing steps for a Source via the Source Details Tab. On the Edit Settings Screen, users can see the various components that make up a Source, including tabs for Source Settings, Dependencies, Relations, Rules, Inputs, and a view of the Source data \(Date View\).

After the Source is created you can access the Settings tab at any time by clicking on the Settings tab in the upper left.

![Source Details Tab](../../../.gitbook/assets/rap-source-details.png)

## Initial Parameters

{% hint style="info" %}
Asterisks \(\*\) mean the Parameter is mandatory and must be specified by users.
{% endhint %}

* **Name\*:** The name of the Source. The Name must be unique. This will be displayed on the Sources screen when browsing Sources. To ensure Sources are organized easily searchable, follow [Naming Conventions](https://intellio.gitbook.io/dataops/v/master/best-practices/naming-conventions).
* **Description\*:** The description of the Source.
* **Active\*:** If set to Active, the Source will run as specified.
* **Agent\*:** The [Agent ]()that is used to monitor and manage incoming data. The RAP Agent installs on local client machines, acquires files from local file storage, and uploads them to the RAP application.
* **Default:** Sets the previously selected Agent to be the default Agent when creating any new Sources.

### Data Refresh Types

A Data Refresh Type specifies how RAP should handle processing and refreshing the data. The five types are described below. The parameters available will dynamically change depending on the user's selection.

{% tabs %}
{% tab title="Key" %}
Sources with the **Key** refresh type contain a unique identifier or _key_ tied to a logical entity. These can be used as lookups from other sources. Sources with a refresh type other than Key cannot be used as lookups. In the terminology of traditional star schema models, Key Sources are analogous to Dimensions.
{% endtab %}

{% tab title="Timestamp" %}
**Timestamp** sources identify changes in data using a column that contains the date and/or time for each record. Collectively, these values represent the time range of the entire data set. Data with newer time ranges replace data with older time ranges.
{% endtab %}

{% tab title="Sequence" %}
**Sequence** sources identify changes in data using a column containing integer numbers that follow a sequence. Data with higher value sequences replace data with lower value sequences.
{% endtab %}

{% tab title="Full" %}
**Full** sources replace the data completely whenever RAP ingests new data into the source. 
{% endtab %}

{% tab title="None" %}
**None** sources do not track changes in data. Instead, RAP appends any new data to the existing data.
{% endtab %}
{% endtabs %}

### **Connection Types**

A Connection Type specifies what format and with what cadence data should be accessed from a connection. There are three main types described below. The parameters available will change dynamically depending on the users's selection.

{% tabs %}
{% tab title="File" %}
RAP supports 2 types of files: **Delimited** and **Fixed Width**. Parameter selections will update dynamically depending on the selection.

* A **Delimited** file consists of lines indicated by line ending characters, and has fields separated by a delimiter. The most common format is a CSV, where each each field is separated by a comma. 
* Data in a **Fixed Width** text file consists of records with constant character length and optional line ending characters. Each column has a fixed width, specified in characters, which determines the maximum amount of data it can contain. No delimiters are used to separate the fields in the file.
{% endtab %}

{% tab title="Loopback" %}

{% endtab %}

{% tab title="SFTP" %}
**SFTP**, or Secure File Transfer Protocol, is a method of transferring files between machines over a secure connection.
{% endtab %}

{% tab title="Table" %}
A **Table** is data that exists in a database. Upon selecting this option, a parameter will appear allowing the user to query the database using SQL.
{% endtab %}
{% endtabs %}

### Additional Inputs

| Filter Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| Connection Type: File | file\_mask\* | .csv | Enter the file name or file name pattern to watch for | N |
| Connection Type: Table | pg\_fetch\_size |  | Fetch size for JDBC connection | Y |
| All | connection\_name\* |  | Connection name of the source | N |
| Connection Type: File and SFTP | delete\_file | TRUE | Remove file from source system when input completes | Y |
| Connection Type: File and SFTP | post\_processing\_folder |  | Optional folder to move file to on source system once input is complete | Y |
| Connection Type: Table | source\_query\* | SELECT \* FROM | Query to acquire the source data | N |

### Schedule

Additional documentation on how to specify a cron schedule can be found at the [Cron Trigger Tutorial](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)

| Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| Initiation Type: Scheduled | seconds |  | Cron Field - Allowed Values: 0-59 - Allowed Special Characters: , - \* / | Y |
| Initiation Type: Scheduled | minutes |  | Cron Field - Allowed Values: 0-59 - Allowed Special Characters: , - \* / | Y |
| Initiation Type: Scheduled | hours |  | Cron Field - Allowed Values: 0-23 - Allowed Special Characters: , - \* / | Y |
| Initiation Type: Scheduled | day\_of\_month |  | Cron Field - Allowed Values: 1-31 - Allowed Special Characters: , - \* ? / L W C | Y |
| Initiation Type: Scheduled | month |  | Cron Field - Allowed Values: 0-11 or JAN-DEC - Allowed Special Characters: , - \* / | Y |
| Initiation Type: Scheduled | day\_of\_week |  | Cron Field - Allowed Values: 0-6 \(0=Monday 6=Sunday\) - Allowed Special Characters: , - \* ? / L C \# | Y |
| Initiation Type: Scheduled | error\_retry\_count | 3 | Number of times an input can be retried before failing | Y |
| Initiation Type: Scheduled | error\_retry\_wait | 60 | Amount of time to wait between error retries \(in seconds\) | Y |
| Initiation Type: Scheduled | disable\_schedule | FALSE | Disable schedule | Y |

### Staging

_\*Any parameter under the File Type option requires the user to have chosen a Connection Type of either File or SFTP._

| Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| All | compression | none | Specifies if reading from a compressed file | Y |
| All | error\_threshold | 1 | Number of errors the staging phase will endure before failing the entire phase | Y |
| All | line\_terminator | \r\n | Specifies character used to terminate lines in the file. Default is \n for unix and \r\n for Windows | Y |
| All | obfuscated\_columns |  | Array of column keys that will be hashed via MD5 before storing | Y |
| All | rows\_to\_skip | 0 | Integer indicating how many rows to skip | Y |
| All | convert\_null\_identifiers |  | Array of text NULL identifiers that will be converted to Postgres NULL during staging | Y |
| All | update\_landing\_timestamp | TRUE | 0/1 specifies if landing start/end timestamps should be updated from the date\_column data | Y |
| All | trim\_headers | none | Choose what sides to trim whitespace on the header line or fields | Y |
| All | trim\_data | both | Choose what sides to trim whitespace on the data elements | Y |
| All | disable\_profiling | FALSE | Check to disable automatic data profiling for the source | Y |
| Refresh Type: All \(except Key\) | batch\_size\* | 1000000 | Maximum size of internal tables. Use smaller values for higher concurrency in later stages | Y |
| Refresh Type: All \(except Key\) | concurrency\* | 1 | Number of processing cores involved in staging phase for Time Series Sources. Minimum number of internal tables | Y |
| Refresh Type: All \(except Key\) | partition\_column |  | Column\(s\) that will be hashed to calculate where data goes during partitioning \(Can be composite\). Used for Time Series Sources with downstream Window Function Processing | Y |
| Refresh Type: Key, Timestamp | date\_column\* |  | Event date column | N |
| Refresh Type: Key, Timestamp | datetime\_format | M/d/yyyy H:m:s | Java Datetime format for the date\_column | N |
| Refresh Type: Key, Timestamp | get\_date\_from\_file\_name | FALSE | Used to apply the date time from the file name as the event time to all records | Y |
| Refresh Type: Sequence | range\_column\* |  | Sequence column | N |
| File Type: Fixed Width | schema\* | {} | JSON Schema for fixed width file types | N |
| Refresh Type: Key | cdc\_process\* | N,C | Customization of what types of records to process as part of Change Data Capture. Values are \(N\)ew, \(C\)hanged, \(O\)ld, \(U\)nchanged | Y |
| Refresh Type: Key | cdc\_store\_history | FALSE | 0/1 indicating whether to store the history of changes for each input | Y |
| Refresh Type: Key, Timestamp | get\_date\_from\_file\_name | FALSE | Used to apply the date time from the file name as the event time to all records | Y |
| Refresh Type: Key | exclude\_from\_cdc |  | Array of column names to be excluded from the CDC calculation | Y |
| Refresh Type: Key | key\_columns\* |  | Array of one or more columns representing the unique identifier for records in this source | N |
| File Type: Delimited | column\_delimiter\* | , | Column delimiter character in the Input file | Y |
| File Type: Delimited | column\_headers |  | Array of strings describing the column headers. Used when files do not contain headers. | Y |
| File Type: Delimited | header\_qualifier | " | Qualifier character for the header row for delimited files | Y |
| File Type: Delimited | text\_qualifier | " | Qualifier character for data rows for delimited files | Y |
| File Type: Delimited | extra\_column\_prefix | _\_extra\_column_ | Column prefix appended when extra columns are found in the headers. Useful when headers are manually set, but new columns appear in a file. | Y |
| File Type: Delimited | ignore\_missing\_headers | FALSE | Place nulls in columns if row comes in with less columns than the manually specified  headers. | Y |
| File Type: Delimited | allow\_extra\_columns | FALSE | Use column prefix to create new columns when more columns come in than specified in Column Headers parameter | Y |

### Retention

| Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| All | archive files | 1 year | Specifies period of time from input completion after which failed files are removed from Input archive folder | Y |
| All | archive files failed | 1 year | Specifies period of time from input completion after which files are removed from Input archive folder | Y |
| All | buffer files | 0 | Specifies period of time from output completion after which output files are moved from fast output buffer zone to long term output storage _\(Legacy Compatibility\)_ | Y |
| All | buffer files failed | 7 days | Specifies period of time from input completion after which files are moved from Input buffer zone to long term Landing folder storage for failed inputs \(output\_status\_code &lt;&gt; ‘P’\) | Y |
| Refresh Type: All \(except Key\) | data tables | 1 year | Specifies period of time from processing completion after which data tables are dropped \(for Time Series Sources\) | Y |
| Refresh Type: Key | data tables | -1 | Specifies period of time from processing completion after which data is deleted \(for Keyed sources\) | Y |
| All | process information | 2 years | Specifies period of time from processing completion after which data in processing tables \(input, landing, output\_send and associated history tables\) is deleted | Y |
| All | work tables | 1 day | specifies period of time from processing completion after which work tables \(key_, ts_\) are dropped. | Y |
| All | work tables failed | 7 days | specifies period of time from input completion after which work tables are dropped for failed inputs. | Y |

### Alerts

| Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| All | distribution\_list |  | Comma separated list of email addresses to receive alerts | Y |
| All | enable\_failure | FALSE | Enables alert generation for processing failures | Y |
| All | enable\_success | FALSE | Enables alert generation for successful processing events | Y |

