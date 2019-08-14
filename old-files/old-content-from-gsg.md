---
description: not sure where to put this
---

# Old Content from GSG

| Input Type | Explanation |
| :--- | :--- |
| file\_type | Our file is a comma-separated values file, which is a delimited file type. |
| cdc\_process | Array of CDC statuses to process and store. All available statuses are new \(N\), changed \(C\), unchanged \(U\), and old \(O\). |
| date\_column | The Source field that identifies the last datetime a record was updated. This field is critical to the Change Data Capture \(CDC\) process, so if the Source doesn’t contain this field, add it in. This field is mandatory. |
| key\_columns | The business key of the Source data; must be enclosed in brackets and quotes. Can be a single key or a composite key. This field is mandatory. |
| rows\_to\_skip | The amount of rows there are above the column headers in the Source file. |
| compression | Specifies if the file is compressed or not, currently the only compression type supported is gzip. In that case, enter gzip. |
| error\_threshold | This parameter specifies the amount of rows that can fail the parsing step before the entire input is declared failed. If the threshold is set to 100 for example, 50 rows could be bad and the input will still pass. |
| column\_headers | Only use this field if the source doesn’t natively have column headers. |
| comment\_prefix | If source file has a special character that is used for comments, enter it here. |
| text\_qualifier | If a delimiter is present in the field, the qualifier tells the platform to ignore that delimiter. |
| datetime\_format | Represents the date time format of the date-related source field\(s\). For valid datetime\_format values, refer to [Java documentation](https://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html). |
| column\_delimiter | The character\(s\) that mark the boundary between two columns in the Source data \(i.e. column, pipe, etc.\) |
| exclude\_from\_CDC | Enter any field names that CDC should ignore. If a field does not associate with how updated a particular file is, exclude it from CDC. |
| cdc\_store\_history | Option indicating whether to store history of changes for a given Input. |
| obfuscated\_columns | Enter all columns from the Source data that contain any sensitive data. RAP hashes these values, protecting the sensitive data. |
| update\_landing\_timestamp | Option indicating if landing start and end timestamps should be updated from the date\_column. |
| line\_terminator | specifies what line endings are contained in the file, currently we either support \r\n or \n |

1. The second section of Source configuration details how RAP is to read and store a data source. The selection of the Source Type drives the specific parameters of this section. To view all of the parameters, go to the bottom of the Sources page and unselect "Hide Advanced Parameters".

   Reference Figure 1.1h for an example.

2. Staging Parameters give the platform information about the file and how to ingest/process it.
3. Add in screenshot of all staging parameters that we're to use for the Getting Started example. Figure 1.1h
4. Press Save.
5. Source configuration is complete! RAP has all the information it needs to complete the Input & Staging phases, allowing the source data to be ingested, read, and written into the RAP internal storage database.
6. To kick off the Input phase, move Divvy\_Stations\_2017\_Q1\_Q2.csv into the input\_path from step \#2 to kick off the processing.
   * This file will immediately disappear from this folder, officially kicking off the input phase.
7. Scroll up to top of page, and navigate to the Inputs tab. Confirm that Input and Staging phases have succeeded.

Now that RAP can ingest a data source, we enter the Validation & Enrichment Phase to apply data validation & business logic.





## 4. Output Configuration

### 4.1 Before Output Configuration

Output configuration consists of two parts, each with a separate tab: **Details** and **Mappings**. **Details** provides gen **Mappings** define the logic for source-to-target mappings.

1. On Menu in the top left, navigate to Outputs.![](/images/menuoutput.png)

   Figure 1.3a

2. In top right corner, click Create New Output.![](/images/newoutput.png)

   Figure 1.3b

### **4.2 Output Details**

1. Enter Output name as Getting Started - Output and provide a brief description.
2. Select Output Type: File![](/images/outputtype.png)

   Figure 1.3c

   * File: Use to create a flat file Output.
   * Table: Use to create a database table Output.

   **Key Decision:** Selection made in this step changes the fields available in the Output parameters section. For configuration details, reference the Output Types configuration section. Use cases for each option are detailed below:

3. Enter Output parameters for File Output Type \(Uncheck “Hide Advanced Parameters” to see all of these\)

   | Output Parameter | Explanation |
   | :--- | :--- |
   | file\_mask | The TSHH12MISS is a token that will be replaced with the time the file was generated.\) |
   | file\_type | Structure of the file. |
   | partition | Indicates how to slice the Output. Options are Segment or Input. Segment will create an Output for every Landing table \(processing unit\) and Input will create an Output for every input. |
   | output\_path | Location of output. |
   | text\_qualifer | If a delimiter is present in the field, the qualifier tells the platform to ignore that delimiter. |
   | column\_delimiter | Character that separates columns in a delimited file. |

4. Add Output Retention Parameters:

   | Output Parameter | Explanation |
   | :--- | :--- |
   | buffer files | How long to keep files in the buffer folder |
   | archive files | How long to keep files in the archive folder |
   | temporary files | How long to keep temporary files |

   **Add in screenshots of both sets of parameters**

5. Click Save.
6. Scroll up and click on Mappings tab.![](/images/outputmappingnavigation.png)

   Figure 1.3

### **4.3 Output Mappings**

1. For each desired column of the Output, click Add Column.
2. Make sure the Show Types and Show Descriptions toggles are on.
3. Enter the Name and Data type of each desired field \(Description is optional\) and Click Save. For this example, we would like ID, Station Name, Latitude, Longitude, dpcapacity \(number of bikes available at a station\) isChicago. Here’s an example:![](/images/firstoutputcolumn.png)

   Figure 1.3

4. For the remaining fields, repeat step 11 with the information below:![](/images/fulloutputmapping.png)

   Figure 1.3

5. Scroll up and click on Add Source. Select A Source window
6. Click New Output Source.
7. In Select A Source, select our Source, Getting Started – Divvy Stations.
8. We’re only concerned with Stations located in Chicago, so we’ll filter it based on the results of the isChicago field. Enter CAST\( as int\) = 1 into the Filter Expression.
9. Operation Type: We wouldn’t like to transform the data in any other way, so keep this as N/A. Click Save and the Mappings page will automatically appear.
10. The file\_mask and output\_path parameters in this section are overrides, and we will leave them blank for now.![](/images/outputsourcemapping.png)

    Figure 1.3

11. In Mappings section, you will now be able to map each of the columns for the new Output Source that we just created. Use the drop down next to each column to align our new Output Source columns with what they should align to in the Output. Click Save.![](/images/fulloutputmappingcomplete.png)

    Figure 1.3

12. The output is configured! Click Getting Started – Divvy Stations next to Show Descriptions and then click Go To Source in the top right of the Output Source window that pops up to take you back to the input page and view the status of the Output’s creation. You may have to reset the Output at this point to get it started.
13. Congratulations! You have successfully integrated a source, applied a validation rule, constructed an enriched column, and built an output to display the results.
14. The newly created output is available in the path specified in \#5.

