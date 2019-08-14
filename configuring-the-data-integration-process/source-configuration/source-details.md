---
description: >-
  The Details tab for a Source allows a user to specify key information about
  the Source including input types and file types.
---

# Details

## Details Tab

On the Edit Source Screen, users can see the various components that make up a Source, including tabs for Source Details, Dependencies, Validations, Enrichments, Inputs, and a view of the Source data.

When creating a new Source, only the Source Details tab is available. Users configure both the Input and Staging processing steps for a Source via the Source Details Tab.

![Details Tab](../../.gitbook/assets/image%20%28144%29.png)

## Initial Parameters

{% hint style="info" %}
Asterisks \(\*\) mean the Parameter is mandatory and must be specified by users.
{% endhint %}

* **Name\*:** The name of the Source. The Name must be unique. This will be displayed on the Sources screen when browsing Sources. To ensure Sources are organized easily searchable, follow [Naming Conventions](../../common-use-cases/naming-convention.md).
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
**File Pull** ingests a flat file at a scheduled time and cadence. A schedule - specified in UTC - will run and retry based on an error count. 

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
| Input Type: File Pull or Table Pull | seconds |  | Cron Field - Allowed Values: 0-59 - Allowed Special Characters: , - \* / | N |
| Input Type: File Pull or Table Pull | minutes |  | Cron Field - Allowed Values: 0-59 - Allowed Special Characters: , - \* / | N |
| Input Type: File Pull or Table Pull | hours |  | Cron Field - Allowed Values: 0-23 - Allowed Special Characters: , - \* / | N |
| Input Type: File Pull or Table Pull | day\_of\_month |  | Cron Field - Allowed Values: 1-31 - Allowed Special Characters: , - \* ? / L W C | N |
| Input Type: File Pull or Table Pull | month |  | Cron Field - Allowed Values: 0-11 or JAN-DEC - Allowed Special Characters: , - \* / | N |
| Input Type: File Pull or Table Pull | day\_of\_week |  | Cron Field - Allowed Values: 0-6 \(0=Monday 6=Sunday\) - Allowed Special Characters: , - \* ? / L C \# | N |
| Input Type: File Pull or Table Pull | error\_retry\_count | 3 | Number of times an input can be retried before failing | N |
| Input Type: File Pull or Table Pull | error\_retry\_wait | 60 | Amount of time to wait between error retries \(in seconds\) | N |
| Input Type: File Pull or Table Pull | disable\_schedule | FALSE | Disable schedule | Y |

### Staging

| Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| All | compression | none | specify if we are reading from a compressed file | Y |
| All | error\_threshold | 1 | Number of errors the staging phase will endure before failing the entire phase | Y |
| All | line\_terminator | \r\n | specifies character used to terminate lines in the file. Default is \n for unix systems and \r\n for Windows | Y |
| All | obfuscated\_columns |  | array of column keys that will be stored hashed\(sensitive\) | Y |
| All | rows\_to\_skip | 0 | integer indicating how many rows to skip | Y |
| All | convert\_null\_identifiers |  | array of text NULL identifiers that will be converted to Postgres NULL during staging | Y |
| All | update\_landing\_timestamp | TRUE | 0/1 specifies if landing start/end timestamps should be updated from the date\_column data | Y |
| All | trim\_headers | none | Choose location\(s\) to trim whitespace on header columns | Y |
| All | trim\_data | both | Choose location\(s\) to trim whitespace on header columns | Y |
| All | disable\_profiling | FALSE | Check to disable automatic data profiling for the source | Y |
| Source Type: Time Series | batch\_size\* | 1000000 | Configurable batch size - sets limit of landing table size. | Y |
| Source Type: Time Series | concurrency\* | 1 | Number of processing cores to involve in staging phase. | Y |
| Source Type: Time Series | partition\_column |  | Column\(s\) that will be hashed to calculate where data goes during partitioning \(Can be composite\) | Y |
| Time Series Range Type: Timestamp | date\_column\* |  | event date key formated as array with 1 value | N |
| Time Series Range Type: Timestamp | datetime\_format | M/d/yyyy H:m:s | datetime format for the event\_date\_key column | N |
| Time Series Range Type: Timestamp | get\_date\_from\_file\_name | FALSE | use datetime format parameter to read date in file name as event date for the input | Y |
| Time Series Range Type: Sequence | range\_column\* |  | Sequence tracking column | N |
| File Type: Fixed Width | schema\* | {} | Json Schema for fixed width file types | N |
| Source Type: Key | cdc\_process\* | N,C | array of CDC statuses to process and store. Values are \(N\)ew, \(C\)hanged, \(O\)ld, \(U\)nchanged | Y |
| Source Type: Key | cdc\_store\_history | FALSE | 0/1 indicating whether to store history of changes for each input | Y |
| Source Type: Key | date\_column |  | event date key formated as array with 1 value | Y |
| Source Type: Key | datetime\_format | M/d/yyyy H:m:s | datetime format for the event\_date\_key column | Y |
| Source Type: Key | get\_date\_from\_file\_name | FALSE | Use datetime format to use date in file name as event date for the input | Y |
| Source Type: Key | exclude\_from\_cdc |  | array of column names to be exceluded from CDC calculation | Y |
| Source Type: Key | key\_columns\* |  | array of one or more column keys representing primary key for this source | N |
| File Type: Delimited | column\_delimiter\* | , | column delimiter character | Y |
| File Type: Delimited | column\_headers |  | array of strings describing the column headers | Y |
| File Type: Delimited | header\_qualifier | " | header qualifier character for delimited files | Y |
| File Type: Delimited | text\_qualifier | " | text qualifier character for delimited files | Y |
| File Type: Delimited | extra\_column\_prefix | _\_extra\_column_ | Column prefix appended when extra columns are found in the headers, only applied if columns are named in Column Headers parameter | Y |
| File Type: Delimited | ignore\_missing\_headers | FALSE | Place nulls in columns if row comes in with less values than header specifies | Y |
| File Type: Delimited | allow\_extra\_columns | FALSE | Use column prefix to create new columns when more columns come in than specified in Column Headers parameter | Y |

### Retention

| Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| All | archive files | 1 year | specifies period of time from input completion after which failed files are removed from Input archive folder \(landing\) | Y |
| All | archive files failed | 1 year | specifies period of time from input completion after which files are removed from Input archive folder \(landing\) | Y |
| All | buffer files | 0 | specifies period of time from output completion after which output files are moved from fast output buffer zone to long term output storage | Y |
| All | buffer files failed | 7 days | specifies period of time from input completion after which files are moved from Input buffer zone to long term Landing folder storage for failed inputs \(output\_status\_code &lt;&gt; ‘P’\) | Y |
| Source Type: Time Series | data tables | 1 year | specifies period of time from processing completion after which data table is dropped \(for ts sources\) | Y |
| Source Type: Key | data tables | -1 | specifies period of time from processing completion after which data is deleted \(for key sources\) | Y |
| All | process information | 2 years | specifies period of time from processing completion after which data in processing tables \(input, landing, output\_send and associated history tables\) is deleted | Y |
| All | work tables | 1 day | specifies period of time from processing completion after which work tables \(key_, ts_\) are dropped. | Y |
| All | work tables failed | 7 days | specifies period of time from input completion after which work tables are dropped for failed inputs. | Y |

### Alerts

| Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| All | distribution\_list |  |  | Y |
| All | enable\_failure | FALSE | Enables alert generation for processing failures | Y |
| All | enable\_success | FALSE | Enables alert generation for successful processing events | Y |

