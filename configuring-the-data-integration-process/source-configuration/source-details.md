---
description: >-
  The Details tab for a Source allows a user to specify key information about
  the Source including input types and file types.
---

# Details

## Details Tab

On the Edit Source Screen, users can see the various components that make up a Source, including tabs for Source Details, Dependencies, Validations, Enrichments, Inputs, and a view of the Source data.

When creating a new Source, only the Source Details tab is available. Users configure both the Input and Staging processing steps for a Source via the Source Details Tab.

![Details Tab](../../.gitbook/assets/image%20%28198%29.png)

## Initial Parameters

{% hint style="info" %}
Asterisks \(\*\) mean the Parameter is mandatory and must be specified by users.
{% endhint %}

* **Name\*:** The name of the Source. The Name must be unique. This will be displayed on the Sources screen when browsing Sources. To ensure Sources are organized easily searchable, follow [Naming Conventions]().
* **Description\*:** The description of the Source.
* **Active\*:** If set to Active, the Source will run as specified.
* **Agent\*:** The [Agent ](../../operation-guide/monitoring-the-process/rap-agent.md)that is used to monitor and manage incoming data. The RAP Agent installs on local client machines, acquires files from local file storage, and uploads them to the RAP application.
* **Default:** Sets the previously selected Agent to be the default Agent when creating any new Sources.

### Source Types

A Source Type specifies how RAP should handle processing and refreshing the data. There are two main types described below. The parameters available will dynamically change depending on a user's selection.

{% tabs %}
{% tab title="Time Series" %}
**Time Series** data has a time-based event column or a sequential ID that corresponds to an ordering in time. Examples include transactions, events, or logs. In the terminology of traditional star schema models, Time Series sources are analogous to certain types of Facts.

There are 3 types of Time Series Source Type: **Timestamp**, **Sequence**, and **None**. Parameter selections will update dynamically depending on the selection.

* **Time Series - Timestamp** data contains a column that is datetime formatted and represents the exact time an event occurred. The column \(**date\_column**\) must exist and be non-null.
* **Time Series - Sequence** data contains a column that is an integer and represents the sequential order that something occurred. The column \(**range\_column**\) must exist and be populated.
* **Time Series - None** data does not have any column that specifies a unique timestamp or unique sequential order. This option is used for exploratory data analysis for loading the data in initially, but should not be used for production data loads.
{% endtab %}

{% tab title="Key" %}
**Key** data represents data where each record has a unique identifier comprised of one or more columns. In the terminology of traditional star schema models, Key Sources are analogous to Dimensions.
{% endtab %}
{% endtabs %}

### **Input Types**

An Input Type specifies what format and with what cadence data should be accessed from a connection. There are three main types described below. The parameters available will change dynamically depending on a user's selection.

{% tabs %}
{% tab title="File Pull" %}
**File Pull** ingests a flat file at a scheduled time and cadence. A schedule - specified in UTC - can run and retry based on an error count. 

There are 2 types of File Pulls: **Delimited** and **Fixed Width**. Parameter selections will update dynamically depending on the selection.

* A **Delimited** file consists of lines indicated by line ending characters, and has fields separated by a delimiter. The most common format is a CSV, where each each field is separated by a comma. 
* Data in a **Fixed Width** text file consists of records with constant character length, and optional line ending characters. Each column has a fixed width, specified in characters, which determines the maximum amount of data it can contain. No delimiters are used to separate the fields in the file.

The schedule is based on cron logic. Additional documentation on how to specify a cron schedule can be found at the [Cron Trigger Tutorial](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html).
{% endtab %}

{% tab title="File Push" %}
**File Push** ingests a flat file immediately when available. The file location is monitored to watch for the existence of the file. 

There are 2 types of File Pulls: **Delimited** and **Fixed Width**. Parameter selections will update dynamically depending on the selection.

* A **Delimited** file consists of lines indicated by line ending characters, and has fields separated by a delimiter. The most common format is a CSV, where each each field is separated by a comma. 
* Data in a **Fixed Width** text file consists of records with constant character length, and optional line ending characters. Each column has a fixed width, specified in characters, which determines the maximum amount of data it can contain. No delimiters are used to separate the fields in the file.
{% endtab %}

{% tab title="Table Pull" %}
**Table Pull** extracts data from a database table at a scheduled time and cadence. A schedule - specified in UTC - will run and retry based on an error count.

The schedule is based on cron logic. Additional documentation on how to specify a cron schedule can be found at the [Cron Trigger Tutorial](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html).
{% endtab %}
{% endtabs %}

## Parameter Groups

### Input

| Filter Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| Input Type: File Pull or File Push | file\_mask\* | .csv | Enter the file name or file name pattern to watch for | N |
| Input Type: Table Pull | pg\_fetch\_size |  | Fetch size for JDBC connection | Y |
| All | connection\_name\* |  | Connection name of the source | N |
| Input Type: File Pull | delete\_file | TRUE | Remove file from source system when input completes | Y |
| Input Type: File Pull | post\_processing\_folder |  | Optional folder to move file to on source system once input is complete | Y |
| Input Type: Table Pull | source\_query\* | SELECT \* FROM | Query to acquire the source data | N |

### Schedule

Additional documentation on how to specify a cron schedule can be found at the [Cron Trigger Tutorial](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html)

| Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| Input Type: File Pull or Table Pull | seconds |  | Cron Field - Allowed Values: 0-59 - Allowed Special Characters: , - \* / | Y |
| Input Type: File Pull or Table Pull | minutes |  | Cron Field - Allowed Values: 0-59 - Allowed Special Characters: , - \* / | Y |
| Input Type: File Pull or Table Pull | hours |  | Cron Field - Allowed Values: 0-23 - Allowed Special Characters: , - \* / | Y |
| Input Type: File Pull or Table Pull | day\_of\_month |  | Cron Field - Allowed Values: 1-31 - Allowed Special Characters: , - \* ? / L W C | Y |
| Input Type: File Pull or Table Pull | month |  | Cron Field - Allowed Values: 0-11 or JAN-DEC - Allowed Special Characters: , - \* / | Y |
| Input Type: File Pull or Table Pull | day\_of\_week |  | Cron Field - Allowed Values: 0-6 \(0=Monday 6=Sunday\) - Allowed Special Characters: , - \* ? / L C \# | Y |
| Input Type: File Pull or Table Pull | error\_retry\_count | 3 | Number of times an input can be retried before failing | Y |
| Input Type: File Pull or Table Pull | error\_retry\_wait | 60 | Amount of time to wait between error retries \(in seconds\) | Y |
| Input Type: File Pull or Table Pull | disable\_schedule | FALSE | Disable schedule | Y |

### Staging

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
| Source Type: Time Series | batch\_size\* | 1000000 | Maximum size of internal tables. Use smaller values for higher concurrency in later stages | Y |
| Source Type: Time Series | concurrency\* | 1 | Number of processing cores involved in staging phase for Time Series Sources. Minimum number of internal tables | Y |
| Source Type: Time Series | partition\_column |  | Column\(s\) that will be hashed to calculate where data goes during partitioning \(Can be composite\). Used for Time Series Sources with downstream Window Function Processing | Y |
| Time Series Range Type: Timestamp | date\_column\* |  | Event date column | N |
| Time Series Range Type: Timestamp | datetime\_format | M/d/yyyy H:m:s | Java Datetime format for the date\_column | N |
| Time Series Range Type: Timestamp | get\_date\_from\_file\_name | FALSE | Used to apply the date time from the file name as the event time to all records | Y |
| Time Series Range Type: Sequence | range\_column\* |  | Sequence column | N |
| File Type: Fixed Width | schema\* | {} | JSON Schema for fixed width file types | N |
| Source Type: Key | cdc\_process\* | N,C | Customization of what types of records to process as part of Change Data Capture. Values are \(N\)ew, \(C\)hanged, \(O\)ld, \(U\)nchanged | Y |
| Source Type: Key | cdc\_store\_history | FALSE | 0/1 indicating whether to store the history of changes for each input | Y |
| Source Type: Key | date\_column |  | Event date column | Y |
| Source Type: Key | datetime\_format | M/d/yyyy H:m:s | Java Datetime format for the date\_column | Y |
| Source Type: Key | get\_date\_from\_file\_name | FALSE | Used to apply the date time from the file name as the event time to all records | Y |
| Source Type: Key | exclude\_from\_cdc |  | Array of column names to be excluded from the CDC calculation | Y |
| Source Type: Key | key\_columns\* |  | Array of one or more columns representing the unique identifier for records in this source | N |
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
| Source Type: Time Series | data tables | 1 year | Specifies period of time from processing completion after which data tables are dropped \(for Time Series Sources\) | Y |
| Source Type: Key | data tables | -1 | Specifies period of time from processing completion after which data is deleted \(for Keyed sources\) | Y |
| All | process information | 2 years | Specifies period of time from processing completion after which data in processing tables \(input, landing, output\_send and associated history tables\) is deleted | Y |
| All | work tables | 1 day | specifies period of time from processing completion after which work tables \(key_, ts_\) are dropped. | Y |
| All | work tables failed | 7 days | specifies period of time from input completion after which work tables are dropped for failed inputs. | Y |

### Alerts

| Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| All | distribution\_list |  | Comma separated list of email addresses to receive alerts | Y |
| All | enable\_failure | FALSE | Enables alert generation for processing failures | Y |
| All | enable\_success | FALSE | Enables alert generation for successful processing events | Y |

