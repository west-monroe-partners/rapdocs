# Process Parameters Documentation

## **Standard Packages Table – Technical Documentation**

Overview: The Standard Packages table provides a detailed view into the parameters required by each RAP data processing phase.

Parameters drive the Input, Staging, and Output phases. At each phase, certain circumstances dictate what set of parameters are necessary to drive the process. This document will outline each of these circumstances and describe the significance of each parameter.

\* Indicates mandatory field

^ Indicates field /phase is not yet implemented

### **Input Phase**

For Input, four separate _input types_ dictate the set of parameters needed to execute the phase: file push, file pull, table, FTP pull. Whichever scenario best fits the platform user’s needs gets selected during Source configuration, changing the set of required parameters. The required parameters for each acquisition type are below:

#### _File Push_

| **Parameter Name** | **Description** | **Allowed Values** | **Default** |
| :--- | :--- | :--- | :--- |
| input\_path | Location of the Source file, typically in the _Inbox_ folder. | “” |  |
| file\_mask | Name of file, using wildcards | “” | \* |

#### _File Pull_

| **Parameter Name** | **Description** | **Allowed Values** | **Defaults** |
| :--- | :--- | :--- | :--- |
| input\_path | Location of the Source file, typically in the _Inbox_ folder. | “” |  |
| file\_mask | Name of file, using wildcards | “” | \* |

#### _Table_

| **Parameter Name** | **Description** | **Allowed Values** | **Defaults** |
| :--- | :--- | :--- | :--- |
| source\_query | SQL Query that represents the desired data to bring into the platform. | “” | SELECT \* FROM |
| database | Specifies the database in which the source data is available. | sql\_server, postgres, oracle |  |
| connection\_string | Connection to source database |  |  |

_^FTP Pull_

| **Parameter Name** | **Description** | **Allowed Values** |
| :--- | :--- | :--- |
| file\_mask | Name of file, using wildcards | integer |
| input\_path | Location of the Source file, typically in the _Inbox_ folder. | “” |

### **Staging Phase**

Staging phase has two Source types that differentiate the required parameters: Keyed and Time Series sources. Keyed sources contain a primary/business key and are often used for lookups, while Time Series sources are transactional and are identified by an event, timestamp, etc. The required parameter types for each source type are below:

#### _Key_

| **Parameter Name** | **Description** | **Allowed Values** | **Default** |
| :--- | :--- | :--- | :--- |
| \*datetime\_format | Datetime format of the date\_column | Datetime formats \(look up\) | M/d/yyyy H:m:s |
| exclude\_from\_cdc | Array of column names to be excluded from CDC calculation. | “” |  |
| cdc\_store\_history | Value indicating whether to store history of changes for a given Input. | 0, 1 | 0 |
| ^file\_type | Structure of the file. | Delimited | delimited |
| cdc\_process | Array of CDC statuses to process and store. | \[N, C, O, U\] | N,C |
| text\_qualifier | Text qualifier for delimited files. | “” | “ |
| \*key\_columns | Array of one or more column keys representing primary key for Source. | “” |  |
| ^comment\_prefix | Rows beginning with this prefix will be skipped. | “” |  |
| \*date\_column | Field containing timestamps of last time data was updated. |  |  |
| rows\_to\_skip | Number of rows to skip before processing. | integer | 0 |
| column\_delimiter | Column delimiter character | ‘’ | , |
| update\_landing\_timestamp | Value indicating if landing start and end timestamps should be updated from the date\_column. | 0, 1 | 1 |
| obfuscated\_columns | Array of sensitive columns that should be obfuscated by the platform \(i.e. PII\) | “” |  |
| column\_headers | Array of strings representing the column headers. | “” |  |
| compression | If file is compressed, specify type of compression |  |  |
| error\_threshold | The amount of staging errors an input can endure before failing the entire staging phase. |  | 1 |
| line\_terminator | Special newline character in the source file \(\r\n OR \n\) |  | \n |

#### _Time Series_

| **Parameter Name** | **Description** | **Allowed Values** | **Default** |
| :--- | :--- | :--- | :--- |
| \*datetime\_format | Datetime format of the date\_column. |  | M/d/yyyy H:m:s |
| ^file\_type | Structure of the file. | Delimited | delimited |
| text\_qualifier | Text qualifier for delimited files. | “” | “ |
| ^comment\_prefix | Rows beginning with this prefix will be skipped. | “” |  |
| \*date\_column | Field containing timestamps of last time data was updated | “” |  |
| rows\_to\_skip | Number of rows to skip before processing. | integer | 0 |
| column\_delimiter | Column delimiter character | ‘’ | , |
| update\_landing\_timestamp | Value indicating if landing start and end timestamps should be updated from the date\_column. | 0, 1 | 1 |
| column\_headers | Array of strings representing the column headers. Used if file has no headers. | “” |  |
| batch\_size | Limit of number of records in a Landing table. | “” | 1,000,000 |
| concurrency | Number of supported threads for multi-threaded processing. | Integer | 1 |
| obfuscated\_columns | Array of sensitive columns that should be obfuscated by the platform \(i.e. PII\) | “” |  |
| compression | If file is compressed, specify type of compression |  |  |
| line\_terminator | Special newline character in the source file \(\r\n or \n\) |  | \n |
| error\_threshold | The amount of staging errors an input can endure before failing the entire staging phase. |  | 1 |

For more information on the datetime\_format field, reference this documentation:

http://www.sdfonlinetester.info/\#

### **Output** **Phase**

RAP supports three types of Outputs, each requiring different parameters: Table, Flat File, and Platform Source.

#### _Flat File_

| **Parameter Name** | **Description** | **Allowed Values** | **Default** |
| :--- | :--- | :--- | :--- |
| file\_type | Structure of the file. | delimited | delimited |
| text\_qualifier | Text qualifier for delimited files. | “” | “ |
| column\_delimiter | Column delimiter character | ‘’ | , |
| partition | Indicates how to slice the Output | Segment, input | segment |
| \*file\_mask | Name of the Output file. |  | FileName&lt;TSHH&gt;.csv |
| \*output\_path | Location of the Output folder. |  | /output |
| ^scope |  |  |  |

#### _Table_

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Parameter Name</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
      <th style="text-align:left"><b>Allowed Values</b>
      </th>
      <th style="text-align:left"><b>Default</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">^scope</td>
      <td style="text-align:left">Indicates how to slice the Output</td>
      <td style="text-align:left">Segment, input, none</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">delete</td>
      <td style="text-align:left">Indicates the data to delete in destination table</td>
      <td style="text-align:left"></td>
      <td style="text-align:left">none</td>
    </tr>
    <tr>
      <td style="text-align:left">*table_schema</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">*table_name</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">*connection_string</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">manage_table</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left">1</td>
    </tr>
    <tr>
      <td style="text-align:left">database</td>
      <td style="text-align:left">Select which database driver to use</td>
      <td style="text-align:left">sql_server, postgres, redshift</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">delete_batch_size</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left">1000</td>
    </tr>
    <tr>
      <td style="text-align:left">temp_file_output_location</td>
      <td style="text-align:left">
        <p>For Redshift Outputs, this indicates the location of a temporary file
          created for output processing.</p>
        <p>Only available for Redshift.</p>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left">s3://</td>
    </tr>
  </tbody>
</table>#### _Output Source Parameters_

| **Parameter Name** | **Description** | **Allowed Values** | **Default** |
| :--- | :--- | :--- | :--- |
| file\_type | Structure of the file. | delimited |  |
| text\_qualifier | Text qualifier for delimited files. | “” |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |
|  |  |  |  |

### **Retention**

RAP periodically cleans up temporary work tables, persisted data tables, and file system files according to the parameters below. Each parameter defines how long to keep each entity before discarding.

| **Parameter Name** | **Description** | **Allowed Values** |
| :--- | :--- | :--- |
| work tables | Interval to retain work tables. |  |
| work tables failed | Interval to retain work tables for failed tables. |  |
| data tables | Interval to retain data tables. |  |
| buffer files | Specifies period of time from Input completion after which files are moved from fast Input buffer zone to long term archive. |  |
| buffer files failed | Specifies period of time from Input completion after which files are moved from fast Input buffer zone to long term archive for failed files. |  |
| archive files | Interval to retain archive files. |  |
| archive files failed | Interval to retain buffer files. |  |
| process information | Interval to retain metadata for each processing phase. |  |

The diagram below demonstrates the difference between _buffer_ and _archive files_:

![](../.gitbook/assets/image%20%28201%29.png)

