# User Manual

## 1. Getting Started Tutorial

Welcome to the Rapid Analytics Platform Getting Started Tutorial! To become acquainted with the Rapid Analytics Platform \(RAP\), we will walk through an end-to-end data integration example, showcasing the entirety of RAP’s capabilities. In this tutorial, we will configure a **Source**, apply **Validation and Enrichment Rules**, and configure an **Output**, allowing the Source to flow through the entire platform.

### 1.1 Requirements

* SQL Server Management Studio installed
* Access to a RAP Demo environment with an Amazon S3 instance for flat file input & output
* RAP Agent configured and installed on your machine for flat file input

### 1.2 Dataset Download

In this tutorial, we will be using Q1 & Q1 2017 data from Divvy, a Chicago-based bike sharing service. Every quarter, Divvy releases two datasets: one transaction-level source of every Divvy ride that takes place, and one that describes the characteristics of each station around the city.

1. Click \[here\]\( [https://s3.amazonaws.com/divvy-data/tripdata/Divvy\_Trips\_2017\_Q1Q2.zip\)](https://s3.amazonaws.com/divvy-data/tripdata/Divvy_Trips_2017_Q1Q2.zip%29) to automatically begin downloading the ZIP folder **Divvy\_Trips\_2017\_Q1Q2**

This ZIP folder contains four files, but we are only concerned with **Divvy\_Stations\_2017\_Q1Q2.csv**, which includes data related to each of the Divvy Stations around Chicago. Follow the instructions below to fully configure the Source.

### 1.3 Source Configuration

#### **1.3.1 Create New Source**

1. In your web browser, navigate to the RAP User Interface [here](www.wmprapdemo.com) \(The link should take you to the **Sources** page, but if you’re not there, navigate to it through the **Menu**\)![](/images/menusources.png)

   Figure 1.3.1a

2. On the Sources page, click **New Source** in the top right corner.![](/images/newsource.png)

   Figure 1.3.1b

#### **1.3.2 General Parameters**

**Give general characteristics about the Source**

1. **Name**: Give your Source a name \(example in figure 1.1.2 below\)
2. **Description**: write a brief description of your Source for your own reference \(example in figure 1.1.2 below\)![](/images/newsourcename.png)

   Figure 1.3.2a

   > **Note**: All fields marked with an asterisk \(\*\) are \*mandatory.\*

3. **Source Type**: Select **Key** since our file, Divvy Stations, is a **Keyed Source**, as each record corresponds to a business/primary key \(the id field\)![](/images/Source%20Type.png)

   Figure 1.3.2b

<table>
  <thead>
    <tr>
      <th style="text-align:left">Source Type</th>
      <th style="text-align:left">Explanation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Keyed Source</td>
      <td style="text-align:left">
        <ul>
          <li>Identifiable by a primary/business key</li>
          <li>Typically represents a dimension in a traditional star schema model</li>
          <li>Can be used as lookup from other sources via the defined primary key (time
            series sources cannot be used as lookups)</li>
        </ul>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Time Series Source</td>
      <td style="text-align:left">
        <ul>
          <li>Identifiable by an event/transaction</li>
          <li>Typically represents a fact in a traditional star schema model</li>
          <li>Does not correspond directly to a primary key &#x2013; uniquely identified
            by a combination of a unit of time and the data of the record</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>> **Key Decision**: Selection made in this step changes the fields available in the _Staging Parameters_ section of Source Configuration and the toggle fields located above Input Parameters. You will gain key-specific parameters such as ‘Key Columns’ when choosing Key, and Time Series specific parameters such as ‘Batch Size’ when choosing Time Series. For this tutorial, we will be selecting **Key**. For Time Series configuration details, reference _Section 2.2 - Source Types_.

1. **Input Type**: Select **File Push**. This will push this file into the platform without the need for a schedule.![](/images/Input%20Type.png)

   Figure 1.3.2c

| Input Type | Explanation |
| :--- | :--- |
| File Pull | Data source is a flat file arriving in the source system at a specified interval |
| File Push | Data source is a flat file to be ingested immediately. The file location is checked at consistent intervals to watch for the existence of the file |
| Table Pull | Data source is located in a database table to be ingested at a scheduled time and cadence. |

> **Key Decision:** Selection made in this step changes the fields available in the _Schedule Parameters_ and _Input Parameters_sections of Source Configuration. For configuration details, reference _Section 2.1 - Input Types_.

**1.3.3 Input Parameters**

**Specify the location of the Source and how to access it**

The first section, _1.3.2 - General Parameters_, gives overview information, such as the **Source Type** and **Input Type**. Depending on the Input Type selected in step 4 of the _General Parameters_ section, different parameters are required in this section.

This tutorial outlines the parameters required when **File Push** is selected as the Input Type. For information about Input parameters when **File Pull** or **Table Pull** is the selected Input Type, see _Section 2.1 - Input Types_.

1. **file\_mask**: Enter **Divvy\_Stations\*.csv** as the file name of your Source

   \*\*screenshot

   The asterisk \(\*\) is used as a wildcard to denote any parts of the file name that may change over time. Divvy\_Stations\*.csv identifies any file that starts with “Divvy\_Stations” and ends with “.csv”, like Divvy\_Trips\_2017\_Q1\_Q2.csv or Divvy\_Trips\_2017\_Q3\_Q4.csv. When a new file becomes available, RAP will ingest the new file without any reconfiguration.

> **Example**: a file named “TestData04.09.2018.csv” could be masked with “TestData\*.csv. Assuming the section after TestData is a changing date field, we will now be able to pick up each day’s new file name, such as “TestData04.10.2018.csv”. When the most recent file becomes available, RAP will automatically accommodate it without any reconfiguring. If your file name never changes, you don’t need to mask anything, and can just enter the file name.

1. **input\_path**: Enter an S3 bucket connected to this instance of RAP![](/images/inputparameters.png)

   Figure 1.1.2a

#### **1.3.4 Staging Parameters**

**Specify how to read the Source**

The Staging parameters in this section tell RAP how to ingest and process your Source. Depending on the **Source Type** selected in step 3 of _Section 1.3.2 - General Parameters_, different parameters are required in this section.

This tutorial outlines the parameters required when Key is selected as the Source Type. For information about Input parameters when Time Series is the selected Source Type, see the _Section 2.2 - Source Types_.

1. To view all of the parameters, go to the bottom of the Sources page and unselect **Hide Advanced Parameters**
2. **File\_type**: Select **delimited** since our file is a comma-separated values file
3. **cdc\_process**: Type in **N,C** Array of Change Data Capture \(CDC\) statuses to process and store. All available statuses are new \(N\), changed \(C\), unchanged \(U\), and old \(O\). Typing in N,C tells RAP to only process new and changed records.
4. **date\_column**: Type in **online\_date** to indicate to RAP the last datetime a record was updated This field is critical to the Change Data Capture \(CDC\) process, so if the Source doesn’t contain this field, add it in. This field is _mandatory_.
5. **key\_columns**: Type in **id** since that is the field that uniquely identifies each row of data This must be enclosed in brackets and quotes and could be a single key or a composite key. This field is _mandatory_.
6. **rows\_to\_skip**: Type in **0** to indicate that there are no rows above the column headers in the Source file
7. **compression**: Enter **gzip** since that is the only compression type currently supported
8. **error\_threshold**: Type in **1** to specify the number of rows that can fail the parsing step before the entire input is declared failed &gt; **Example**: If the threshold is set to 100, 50 rows could be bad, and the input will still pass.
9. **column\_headers**: _leave blank_ Only use this field if the source doesn’t natively have column headers.
10. **comment\_prefix**: _leave blank_ If source file has a special character that is used for comments, enter it here.
11. **text\_qualifier**: Type " to tell RAP to ignore this delimeter
12. **datetime\_format**: Ensure that this field contains **M/dd/yyyy H:m:s** to represent the date time format of the date-related source field\(s\) &gt;Note:For valid datetime\_format values, refer to Java documentation [here](https://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html)
13. **column\_delimiter**:Type in , to indicate that commas mark the boundary between two columns in the Source data
14. **exclude\_from\_CDC**: _leave blank_ This tells RAP which field names should be ignored during the Change Data Capture \(CDC\) process. If a field does not associate with how updated a particular file is, exclude it from CDC.
15. **cdc\_store\_history**: Check the box to indicate that RAP should store the history of changes for the Input
16. **obfuscated\_columns**: Type in **name** to indicate that the name field contains sensitive data RAP has the ability to protect sensitive data by hashing any values indicated by the user to be sensitive.
17. **update\_landing\_timestamp**: Check the box to indicate that landing start and end timestamps should be updated from the **date\_column**
18. **line\_terminator**: Type in \n to specify what line ending is contained in the file Currently, RAP supports both \r\n and \n.

    \*\*Add in screenshot of all staging parameters that we're to use for the Getting Started example. Figure 1.3.4

#### **1.3.5 Source Configuration Final Steps**

1. Press **Save** - Source Configuration is complete!
2. Move **Divvy\_Stations\_2017\_Q1\_Q2.csv** into the specified **input\_path** from step \#2 of _Section 1.3.3 - Input Parameters_to initiate processing The file will immediately disappear from this folder and move to the Landing Folder, officially kicking off RAP data processing.
3. Scroll up to top of page and navigate to the **Inputs** tab. Confirm that Input and Staging phases have succeeded.
4. Scroll up to top of page, and navigate to the Inputs tab. Confirm that Input and Staging phases have succeeded.

Now that RAP can ingest a data source, we enter the Validation & Enrichment Phase to apply data validation and transformations.

### 1.4 Validation & Enrichments Configuration

Creating **Validation Rules** and **Enrichment Rules** allows you to manipulate and transform your data. Each are described in the table below:

| Type | Explanation |
| :--- | :--- |
| Validation Rule | Applies prescribed logic to the data and flags each record with either a pass, failure, or warning flag |
| Enrichment Rule | Creates a new column, depending on a calculation or a lookup from a separate source |

Below, we will walk through examples of how to create these rules using the Divvy bike data we just input in the last section.

#### **1.4.1 Validation Rule**

**Apply prescribed logic to the data to pass, warn, or fail each record**

Validation rules tell RAP to flag records that do not adhere to a prescribed logic. In this example, we want to ensure that the **city** field has _no null values_. The following steps will describe how to fail records that do not follow that rule.

1. Navigate to the **Validations** tab on the Source you are configuring![](/images/validationtab.png)

   Figure 1.2

2. In the top right corner, click **New Validation Rule**

   **Key Decision:** Choosing between Fail and Warn will not change the available configuration fields like Source Type and Input Type do, but it will change how RAP flags data validation failures:

   * **Fail**: Marks records as F
   * **Warn**: Marks records as W
   * Downstream, you may customize your Output to contain or omit certain records based on the flag generated during this phase.

3. **Name**: Give this validation rule the name **City Not Null**
4. **Description**: Add a brief description
5. **Validation Rule Type**:
6. \*\*Expression to Validate\*\*: Type in \*\*T.city IS NOT NULL\*\* to indicate that we want to implement a rule that none of the values in the city field are null
   * Value to be Returned field is a source-specific rendering of the template. Select the source field on which to apply the validation rule
   * Prepend the field name with "T." - Think "_T_ for _Table_ that we're currently working in"
   * For commonly used Validation and Enrichment patterns, refer to section \#7 of the RAP cookbook
   * Press the \` key \(top left of the keyboard\) to populate a list of available source fields
7. **When expression is true, set to**: Choose **Fail** &gt; **Note**: Choosing between Fail and Warn will not change the configuration fields, but it will change how the platform responds to data validation failures. Records that do not follow the prescribed rule will simply be marked as failed or warned. The Output phase can be configured to omit certain records based on the fail or warn flag generated by validation rules.
8. Click Save.![](/images/vrb.png)

   Figure 1.2.1

#### **1.4.2 Enrichment Rule**

Hypothetically, let's say there is a business requirement indicating that we would like to filter the location of the station based on whether it is in Chicago or not. This flag will be expressed as a 0 or 1 \(0 = not Chicago, 1 = Chicago\). Having this expressed as an integer \(as opposed to **City** column, which is stored as text\).simplifies downstream analyses, so we can use an Enrichment rule to make this alteration.

1. We are going to create an Enrichment Rule. Navigate to Enrichments tab and select **New Enrichment rule.**
2. Set Name to **Flag: Is Chicago** and provide a brief description.
3. Set "On conversion error: set to" to **Ignore**.
4. Set Operation Type as **Formula**.
5. Enriched Key Name is the name of the field created by this rule. Call it **isChicago**. Set type to **numeric**.![](/images/enrichmentrule.png)

   Figure 1.4.2a

   **Key Decision:** Selection made in this step changes the fields available in the Enrichment Rule. For configuration details, reference _Section 4 - Enrichment Rule Configuration_. Use cases for each option are detailed below:![](/images/enrichmentoperationtypes.png)

   Figure 1.4.2b

   * Formula: Create a calculated column, based on prescribed logic.
   * Lookup: Create a new column with data from another source.

6. In Return Expression, enter:

   `CASE WHEN T.city = ‘Chicago’ THEN 1 ELSE 0 END`

   **Add in screenshot of this**\(1.4.2c\)

   * Behind the hood, the Return Expression acts a SQL SELECT statement that captures the desired logic of the new column.
     * Note: The supported syntax is specific to PostgreSQL. Refer to PostgreSQL documentation: [https://www.postgresql.org/docs/](https://www.postgresql.org/docs/)

7. Click Save.
8. Click over to the Inputs page, and for the most recent Input, click on the elipssis on the far right and select **Reset Validation & Enrichment**.
9. To ensure the success of the Enrichment Rule we have just created, Navigate to **Data Viewer**. Confirm the data is there, including the new isChicago field.![](/images/dataviewer.png)

   Figure 1.4.2d

10. Once Validation & Enrichment passes, we are ready to create an Output.
11. At this point, you can also download the content of the Data Viewer into a CSV file by hitting the **Download** button located underneath the Data Viewer tab on the right of the screen.

### 1.5 Output Configuration <a id="13"></a>

Output configuration consists of two parts, each with a separate tab: **Details** and **Mappings**. **Details** provides general information about the desired Output, including what it should be called and where it should be located. **Mappings** define the logic for source-to-target mappings.

1. On Menu in the top left, navigate to **Outputs**.![](/images/menuoutput.png)

   Figure 1.5a

2. In top right corner, click Create **New Output**.![](/images/newoutput.png)

   Figure 1.5b

#### **1.5.1 Output Details**

1. Enter Output name as **Getting Started - Output** and provide a brief description.
2. Select Output Type: **File**![](/images/outputtype.png)

   Figure 1.5.1a

* **File:** Used to create a flat file Output.
* **Table**: Used to create a database table Output.

  **Key Decision:** Selection made in this step changes the fields available in the Output parameters section. For configuration details, reference the Output Types configuration section. Use cases for each option are detailed below:

1. Enter Output parameters for File Output Type \(Uncheck “Hide Advanced Parameters” to see all of these\)

   | Output Parameter | Explanation |
   | :--- | :--- |
   | file\_mask | The TSHH12MISS is a token that will be replaced with the time the file was generated.\) |
   | file\_type | Structure of the file. |
   | partition | Indicates how to slice the Output. Options are Segment or Input. Segment will create an Output for every Landing table \(processing unit\) and Input will create an Output for every input. |
   | output\_path | Location of output. |
   | text\_qualifer | If a delimiter is present in the field, the qualifier tells the platform to ignore that delimiter. |
   | column\_delimiter | Character that separates columns in a delimited file. |

2. Add Output Retention Parameters:

   | Output Parameter | Explanation |
   | :--- | :--- |
   | buffer files | How long to keep files in the buffer folder |
   | archive files | How long to keep files in the archive folder |
   | temporary files | How long to keep temporary files |

   **Add in screenshots of both sets of parameters**

3. Click Save.
4. Scroll up and click on Mappings tab.![](/images/outputmappingnavigation.png)

   Figure 1.5.1b

#### **1.5.2 Output Mappings**

1. For each desired column of the Output, click **Add Column**.
2. Make sure the Show Types and Show Descriptions toggles are on.
3. Enter the Name and Data Type of each desired field \(Description is optional\) and click **Save**. For this example, we would like **ID**, **Station Name**, **Latitude**, **Longitude**, **dpcapacity** \(number of bikes available at a station\), and **isChicago**. Here’s an example:![](/images/firstoutputcolumn.png)

   Figure 1.5.2a

4. For the remaining fields, repeat step 11 with the information below:![](/images/fulloutputmapping.png)

   Figure 1.5.2b

5. Scroll up and click on **Add Source**. Select A Source window
6. Click **New Output Source**.
7. In Select A Source, select our source, **Getting Started – Divvy Stations**.
8. We’re only concerned with Stations located in Chicago, so we’ll filter it based on the results of the isChicago field. Enter **CAST\( as int\) = 1** into the Filter Expression.
9. Operation Type: We wouldn’t like to transform the data in any other way, so keep this as **N/A**. Click **Save** and the Mappings page will automatically appear.
10. The file\_mask and output\_path parameters in this section are overrides, and we will leave them blank for now.![](/images/outputsourcemapping.png)

    Figure 1.5.2c

11. In Mappings section, you will now be able to map each of the columns for the new Output Source that we just created. Use the drop down next to each column to align our new Output Source columns with what they should align to in the Output. Click **Save**.![](/images/fulloutputmappingcomplete.png)

    Figure 1.5.2d

12. The output is configured! Click **Getting Started – Divvy Stations** next to Show Descriptions and then click **Go To Source**in the top right of the Output Source window that pops up to take you back to the input page and view the status of the Output’s creation. You may have to reset the Output at this point to get it started.

Congratulations! You have successfully integrated a source, applied a validation rule, constructed an enriched column, and built an output to display the results. The newly created output is available in the **output path** specified in Step \#5 of _Section 1.5.1 - Output Details_.

## 2. Source Configuration

The fields in the Source Configuration vary based on the selected Input and Source type. Some of the fields in the Source Configuration depend on the selected Acquisition Type, while others depend on the selected Source Type. This guide provides step-by-step instructions for each Input type, and then for each Source type. To configure a Source, both sections must be completed.

In the getting started tutorial, there were several areas marked with a _**Key Decision**_, indicating that a decision made at that particular step impacts the remainder of the configuration. This section provides a detailed look into how to respond to these decisions.

### 2.1 Input Types

This section details the source configuration with respect to the fields dependent on Input Type - the means by which RAP brings in a Source. RAP supports 3 Acquisition Types: File Pull, File Push, and Table Pull. In particular, Input Type selection dictates which fields are available in the Input Parameters section and Schedule of the configuration.

All steps not explicitly mentioned in this section should be completed according to the Getting Started Tutorial and/or configuration sections based on other Key Decisions.

#### **2.1.1 File Pull**

_When to Use:_ Select this Input Type if the Source file arrives on a regular, repeatable basis \(i.e. weekly sales report, daily stock market prices, etc.\). Use these configuration steps to bring a file into the platform on a specified interval, indicated by a Schedule.

**Steps:**

1. Ensure you’ve selected **File Pull** as the Input type![](/images/UM_input_type.png)

   Figure 2.1.1a

2. Complete the Schedule parameters:

   * **Seconds**: cron field
   * **Minutes**: cron field
   * **Hours**: cron field
   * **day\_of\_month**: cron field
   * **Month**: cron field
   * **day\_of\_week**: cron field
   * **error\_retry\_count**: how many times the input can be retried before marking as failure
   * **error\_retry\_wait**: amount of time to wait between error retries \(in seconds\)

   > **Important**: The cron fields make up a cron expression, which is an expression that tells the platform what time/day to run the input at. [This website](http://www.quartz-scheduler.org/documentation/quartz-2.x/tutorials/crontrigger.html) explains what cron expressions are and how the different fields work.

3. Input parameters:

   * **file\_mask** represents the file name,using the asterisk \(\*\) as a wildcard to identify all desired files. To find this, look at the name of the file and add an asterisk to any parts of the file name that may change over time.
   * **input\_path** is the initial location of the source file

   ![](/images/UM_input_parameters.png)

   Figure 2.1.1b

#### **2.1.2 File Push**

_When to Use:_ Use these configuration steps to bring a File into the platform without a Schedule. File Push is most applicable in a scenario where the file needs to be immediately available.

File Push is explained in the _Getting Started Tutorial – Section 1.1_.

#### **2.1.3 Table Pull**

_When to Use:_ Use these configuration steps to bring data from a Database table into the Platform on a specified interval, indicated by a Schedule.

1. Ensure you’ve selected **Table Pull** as the Acquisition Type.![](/images/UM_input_type_tablepull.png)

   Figure 2.1.3a

2. Complete the Schedule parameters:
   * **Seconds:** cron field
   * **Minutes:** cron field
   * **Hours:** cron field
   * **day\_of\_month:** cron field
   * **Month:** cron field
   * **day\_of\_week:** cron field
   * **error\_retry\_count:** how many times the input can be retried before marking as failure
   * **error\_retry\_wait:** amount of time to wait between error retries \(in seconds\)
3. Input Parameters:
   * **database\_type:** type of database that we’re pulling from \(SQL Server, Postgres, etc\)
   * **database\_name:** name of the database that the table we want to pull from is located in
   * **host\_name:** host address of the source database
   * **password:** password for the database user
   * **port:** port for the database server
   * **source\_query:** SQL query to capture data from the source table
   * **connection\_string:** Enter full connection string to source database
   * **pg\_fetch\_size:** Fetch size for JDBC connection
   * **user:** username for the database user

You can either enter in the parameters that make up the connection string or put in a full connection string in the connection\_string parameter. Either way works, but make sure to complete one or the other completely.

> Tip: With the source query, you can add in &lt;\extract\_datetime&gt; to grab the latest extract datetime for the source and parameterize it into the query.

![](/images/UM_input_parameters_tablepull.png)

Figure 2.1.3b

### 2.2 Source Types

This section details the source configuration with respect to the fields dependent on Source Type. Before starting these steps, ensure that all steps in “Source Configuration by Acquisition Type” have been completed for the given acquisition type.

All steps not explicitly mentioned in this section should be completed according to the _Getting Started Tutorial_ and/or configuration sections based on other Key Decisions.

#### **2.2.1 Key**

_When to Use:_ Key Sources represent any data source in which a primary or business key uniquely identifies each record. In the terminology of traditional star schema models, Key Sources are analogous to Dimensions.

Keyed source configurations are explained in the _Getting Started Tutorial – Section 1.1_.

#### **2.2.2 Time Series**

Use these configuration steps if the Source Type selected in the prior section is “Time Series.”

**Steps:**

1. The only section dependent on Source Type is the Staging Parameters. Definitions of the staging parameters for Time Series sources are below:

   * **file\_type:** Describes the strucutre of the file \(i.e. delimited\)
   * **batch \_size:** Limit of the number of records in a Landing table, which is RAP’s way to break up large files into smaller processing units.
   * **concurrency:** Number of supported threads for multi-threaded processing.
   * **datetime\_format:** Datetime format of the date\_column.
   * **date\_column:** The Source field that identifies the datetime in which the event described in a record occurred.
   * **rows\_to\_skip:** If the top row of the Source file is the column headers, enter 0. Otherwise, enter the number of rows there are above the column headers and the rest of the data.
   * **column\_headers:** Array of strings representing the column headers – used if file has no neaders.
   * **comment\_prefix:** Used to identify comments.
   * **text\_qualifer:** Text qualifier for delimited files.
   * **column\_delimiter:** The character\(s\) that mark the boundary between two columns in the Source data \(i.e. column, pipe, etc.\)
   * **obfuscated\_columns:** Array of sensitive columns that should be kept anonymous to the platform \(i.e. PII\).
   * **update\_landing\_timestamp:** Value \(0/1\) indicating if landing start and end timestamps should be updated from the date\_column.
   * **error\_threshold:** Indicates max number of file parsing errors, after which, staging will fail
   * **line\_terminator:** specifies what line endings are contained in the file, currently we either support \r\n or \n

   > Refer to documentation [here](https://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html) for accepted datetime\_format values.

2. Fill out the Staging parameters. Example values are in figure 2.2.2![](/images/UM_staging_parameters1.png)

   Figure 2.2.2a![](/images/UM_staging_parameters2.png)

   Figure 2.2.2b![](/images/UM_staging_parameters3.png)

   Figure 2.2.2c

## 3. Validation Rule Configuration

Validation rules enforce data quality constraints on the Source data and come in two varieties: Failure and Warning.

These rules are explained in the Getting Started Tutorial – Section 1.2.

## 4. Enrichment Rule Configuration

Enrichment rules provide the logic for adding new fields to the data. Enrichments can take two forms: formula and lookup. Formula-based enrichment rules populate a new field with the logic of a configured SQL SELECT clause. Lookup-based enrichment rules join disparate data sources on a specified criterion, and populate a new field with the logic of a configurable SQL SELECT clause.

> Note: All data field names must be enclosed with &lt;&gt; \(i.e. trip\_id field: &lt;\trip\_id&gt;\)

### 4.1 Formula Enrichment Rule

_When to Use:_ Use a formula enrichment rule when trying to add a column to the data based on some form of prescribed logic.

**Steps:**

1. Set Operation Type as **Lookup.**
2. Identify the source being brought in in the **Lookup Source** field.
3. Lookup Expression is the expression on which to join the source and the lookup source.
4. Return Expression is an expression from Lookup Source which populates the Enriched Column.![](/images/UM_enrichment_rule.png)

   Figure 4.2

This lookup joins two tables that have a **from\_station\_id** field in common and adds the **latitude** column from the lookup source to the data. Under the hood, the from\_station\_id isn’t the field name in both sources, but the ID field of the lookup source and from\_station\_id field from the source reference the same data.

## 5. Output Configuration

RAP natively supports two types of Outputs: Table and File. This section details the configuration of each type.

All steps not explicitly mentioned in this section should be completed according to the Getting Started Tutorial and/or configuration sections based on other Key Decisions.

### 5.1 Table

_When to Use:_ Use this Output configuration when the desired Output table is a database table.

**Steps:**

1. Give Output a name and a description
2. Select Output type as **Table**
3. Select your Driver \(the database type you're outputting to\)![](/images/UM_output_type_table.png)

   Figure 5.1a

4. Enter Output parameters for type table:

   * **Delete:** This parameter details what data will be deleted from the table prior to loading it.
     * **all** will result in a full truncate and reload
     * **none** will only insert the data
     * **key** will delete all data in the destination table that shares keys with the incoming data, and then insert the new data
     * **range** will delete all data in the destination table that is between the transaction start and end times for the incoming data, and then insert the new data
   * **Database\_name:** name of the database that the table we want to pull from is located in
   * **Host\_name:** host address of the source database
   * **Password:** password for the database user
   * **Port:** port for the database server
   * **User:** username for the database user
   * **Table\_name:** Creates the name of the table, or specifies an already existing table.
   * **Table\_schema:** Creates the schema of the table, or specifies an already existing schema
   * **Manage\_table:** If set to true, it gives permission to RAP to create and modify tables.
   * **Connection\_string:** Enter connection string of the output database.
   * **Batch\_size:** Batch size for inserts
   * **Bulk\_insert:** Only applicable if your database type is SQL Server. This option invokes the use of the SQLServerBulkCopy Object to insert data \(recommended\)
   * **Partition:** Indicates how to slice the Output. Options are Segment or Input. Segment will create an Output for every Landing table \(processing unit\) and Input will create an Output for every input.
   * **Delete\_batch\_size:** Batch size used when perfoming deletes. Applicable when using key or range delete types.
   * **Delete\_temp\_file:** Option to keep the temp file used when doing an output to Redshift. Only applicable to Redshift database type.
   * **Temp\_file\_output\_location:** Location to place temp file used when doing an output to Redshift. Only applicable to Redshift database type.

   ![](/images/UM_output_parameters.png)

   Figure 5.1b

5. Click over to the Mapping tab and create the columns with their data types for the output.![](/images/UM_output_mapping.png)

   Figure 5.1c![](/images/UM_output_newcolumn.png)

   Figure 5.1d

6. Finally, click over to the Sources tab and map all of the output columns you’ve just created in step \#4 to columns of the source data.![](/images/UM_output_sources_tab.png)

   Figure 5.1e

#### 5.2 File <a id="29"></a>

_When to Use:_ Use this Output configuration when the desired Output table is a flat file.

File Outputs are explained in the Getting Started Tutorial – Section 1.3.

## 6. Process Monitoring

### 6.1 Inputs Tab

An “Input” is RAP’s atomic unit of data processing. Conceptually, an Input corresponds to a single file or Table from a configured Source. The Inputs tab, located on the Sources page, provides insight into the processing of all stages for a given Source. The top of the page has a variety of filters, which allow filtering based on the status of all four processing stages, as well as the file path name. This page provides tools to monitor and interact with the data processing phases.

Each row represents a file for each source and each column value represents the status of a particular processing phase \(Input, Staging, Validation & Enrichment, Output\). A set of icons identifies the status of each phase:![](/images/UM_process_inputtab.png)

Figure 6.1

* **Pending:** Phase is waiting on a dependent source to complete processing.
* **Ready:** Landing is prepared for processing phase – if the next phase is configured, it will automatically change to In Progress.
* **In-Progress:** Processing phase currently executing.
* **Failed:** Processing phase failed. Check detail modal to discover reason of failure.
* **Completed:** Processing phase successfully completed. Check detail modal to discover phase metadata.

For more specific information on each of the phases, click on the Icon. The phase detail modals show the phase’s duration, as well as the number of records that successfully \(or unsuccessfully\) flow through each phase. You can also see logs and processing messages or errors when the Icon is clicked.

The ellipsis located on the far right of each row provides options for managing the processing phases per Landing. Reset Staging and Reset Output will start over the Staging and Output phases, respectively. Delete will delete an Input \(**Warning: This can’t be undone**\). View Data will take you to the Data View tab.

#### 6.2 Email Notifications

The platform provides the ability to toggle on email notifications for the various processing stages. This is configured in the Details tab for a Source, and configured on a per source basis. There are two options:

1. **Enable success:** Emails are sent whenever a processing stage for the source successfully completes. Enable this on a source if you are interested in the completion of stages, or would like to know when data has successfully processed and is ready.
2. **Enable failure:** Emails are sent whenever a processing stage for the source fails. An error message is also included in the email. Enable this if you are interested in the completion of stages, or would like to know when the data is not ready due to a failure in the pipeline.

If you want comprehensive notifications for a source, it is recommended to enable both of these. In the **distribution\_list**parameter, enter in any email addresses of those who wish to receive the notifications. The format should be a comma separated list.![](/images/UM_alerts.png)

Figure 6.2

## 7. RAP Cookbook

### 7.1 Overview

RAP’s Validation & Enrichment functionality provides users the opportunity to ensure data quality and make it more accessible for downstream users \(i.e. business analysts, data scientists\). We have found that there are several common tasks that are used in many different applications, and this Cookbook provides instructions on how to implement these tasks in the Rapid Analytics Platform.

### 7.2 Not Null Constraint

_Motivation:_ In some cases, null values should be excluded from the data set. This recipe uses a Validation Rule to filter out null values, so that they can be excluded from the Output.

_How to Implement:_ In Validation & Enrichment Templates, create:![](/images/UM_7.1a.png)

Figure 7.2a

Then, in the Source-specific Validation Rules, create this:![](/images/UM_7.1b.png)

Figure 7.2b

This will create a query identifying all values of the **id** column and mark them as Failures. When creating an Output, we can now filter by these flags to either display all the failed columns or exclude them from the Output.

### 7.3 Enforcing Values to Be in a Specified Range

_Motivation:_ Bad data can come in the form of nonsensical outlier values. For example, if a column describes exam scores \(1-100 scale\) and contains values outside of that range, we can deduce that those values are bad data. This recipe shows how to create a Validation Rule to identify those records.

_How to Implement:_ Create a Validation & Enrichment Rule template that identifies the records outside of the desired range of values. In the example below, the field contains latitude values for areas around Chicago. The limits of Chicago’s latitude are 41.7 on the south and 42.1 on the north, so any values outside of this range are bad data points.![](/images/UM_7.3a.png)

Figure 7.3a

Then in the Source-specific Validation rules, create this, with the desired field in Key 1.![](/images/UM_7.3b.png)

Figure 7.3b

This recipe can also be applied to enforcing values between certain dates, which we have found is very useful. Follow the same steps as above, just set the query to enforce values between two dates.

```text
 > **Best Practice:** RAP internally stores all data as text, so for calculations like this that require the data to be a numeric value. It is best practice to use the CAST function to transform the data into numeric.
```

### 7.4 Enforcing Values From a Specific List

_Motivation:_ Cookbook example 7.3 is great for filtering numeric fields on a certain range – but what if you need to filter a categorical field? Or filter a numeric field on a criteria other than a continuous range? By enforcing on the values from a list, we are able to filter data based on the values of a specified list.

_How to Implement:_ Similar to 7.3, we will be creating a validation rule capturing the logic of the desired values.

In this example, we have a FIFA dataset with data of every player in the FIFA league, including their skill level across several areas, their club, salary, etc. Let’s say we’d like to create an Output only containing the players of a few teams. To do so, we create the rule below and apply it to the Club field of the data.![](/images/UM_7.4a.png)

Figure 7.4a![](/images/UM_7.4b.png)

Figure 7.4b

A quick look at the Data Viewer reveals that all records that don’t have one of those values in the Club column have failed validation \(in Red\).![](/images/UM_7.4c.png)

Figure 7.4c

### 7.5 Transforming Categorical Values into Numeric Values

_Motivation:_ Downstream analysis often requires that categorical variables are expressed as numeric values. Consider a field describing “Gender”. A statistical model may not accept the values “Male” and “Female”, but it does accept 0’s and 1’s. This recipe shows how to create an Enrichment Rule that makes this transformation.

_How to Implement:_ We are going to take the values of a gender field and create a new field \(an “Enriched” field\) that contains a 0 for male and 1 for female. In RAP, this would take the form of an Enrichment rule with operation type Formula.![](/images/UM_7.5a.png)

Figure 7.5a

We are using the ALL Records – Complete – true rule type because we want all values to flow through, then apply the Return Expression logic to create the Enriched Column. This CASE…WHEN…THEN pattern is extremely powerful, as it can be used any time to apply conditional logic in creating an Enriched column.

Looking at the Data Viewer, we can see that our new column, Gender\_Numeric, is there.![](/images/UM_7.5b.png)

Figure 7.5b

### 7.6 Create an Output for Failed Records

Motivation: Using the Inputs page, we can monitor the status of an Input’s validation and enrichment phase by seeing how many records pass or fail. However, often times we would like to see which records fail. To achieve this, we can create a special Output containing the values that have failed validation, providing an extra layer of visibility into the data processing.

How to Implement: Create an Output that filters by the validation\_status\_code. Validation\_status\_code has one of three values: ‘P’ for passed, ‘F’ for failed, and ‘W’ for warn.

Create Output as usual for Details and Columns tabs. Add an Output Source, and then click on the name of the newly added Output Source. Enter **validation\_status\_code** = ‘F’ in the filter.![](/images/UM_7.6a.png)

Figure 7.6a

It is important that this pattern is paired a validation rule that fails some of the records, otherwise this Output will be blank. To see if the Input fails any records, check the Validation detail on the _Landings_ tab:![](/images/UM_7.6b.png)

Figure 7.6b

### 7.7 Capture a Segment of a String Field

_Motivation:_ Sometimes, a string field contains extra data that we are not concerned about. RAP supports use of a substring function to omit the undesired characters.

_How to Implement:_ We will create an Enriched column and set the _Return Expression_ to a field containing SQL Substring logic.

The last example \(Create output for failed records\) failed all records for a particular field that have four characters, as the data should only have two values. In particular, some integer values have addition/subtraction symbols in them \(i.e. 46+1, 78-3, etc.\), which is invalid for an integer. To correct this, we would like to create a column that contains only the first two characters of that field and then cast it to an Integer, using Substring functions.

Validation/Enrichment template:![](/images/UM_7.7a.png)

Figure 7.7a

This rule captures any value that has four characters. The Return Expression below outlines the formula to apply to the records captured by the above rule.![](/images/UM_7.7b.png)

Figure 7.7b

The substring function may take many different forms \(see [this link](https://www.postgresql.org/docs/9.1/static/functions-string.html)\), but in this case, we just wanted to capture the first two characters of the contents of the “Agility” field. This field is now available \(as displayed by the Data Viewer\) and can be built into any downstream Outputs.

Using a Validation rule \(_Validation Rule Type_ field\) in conjunction with a Formula \(the Return Expression field\) is a powerful pattern for making corrections to the data.

### 7.8 Change the Grain of Data and Process it Again Using a Platform Source

A Platform Source refers to a data source that has been previously processed by RAP. In the event that a change in the grain of the data becomes necessary, RAP accommodates a reprocessing at the desired grain through the use of a Platform Source. To configure a Platform Source, create a Source, Validation/Enrichment Rule, and an Output just like with any other source type. The key difference is that the Output configuration for Platform Source details should output a file into the location where another sources expects an input. The file will then be picked up and processed by the second input.  


