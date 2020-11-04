---
description: >-
  Output Mapping controls the way data is sent to its final destination. It
  allows a user to rename columns, append multiple Sources, and filter data.
---

# !! Output Mapping

## Navigation

The Output Mapping tab can be split into three sections: [Column Configurations](output-mapping.md#column-configurations), [Sources Configurations](output-mapping.md#source-configurations), and [Output Source Details](output-mapping.md#output-source-details). 

* **Column Configurations** define the final schema of the Output data.
* **Source Configurations** allows selection of multiple sources and mappings to Output Columns.
* **Output Source Details** allows filtering and operations on Source data.

{% hint style="info" %}
Important note: no changes made to an output mapping will take effect unless the user presses the **Save** button at the bottom right hand corner of the output mapping screen.
{% endhint %}

## Adding a Source

To enable mappings between an output and a source, the first step is to add the source to the Output Mapping screen, thus generating a link between this Source and Output. Click **Add Source Mapping** at the top of the mapping table and underneath the "Mapping" tab, as seen below.

![](../../.gitbook/assets/addoutputsource%20%281%29.png)

This should bring up the Source Mapping configuration modal. To select the source to map, click on the **Select Source** search bar/drop down menu circled below, begin typing the name of the source that needs to be mapped, and once the desired source appears in the dropdown menu, it can be selected.

![](../../.gitbook/assets/selectmappingsource.png)

#### Options: 

* **Filter Expression:** Allows users to define a condition that will exclude rows of data from the output, for which the filter expression does not evaluate to **true**. More information on filter expressions can be found in this article below.
* **Operation Type:** Default is "N/A". Allows the user to mark an output source mapping as an aggregate. More information on aggregate source mappings can be found below.
* **Name:** The name of the Source Mapping defaults to name of the source itself. The user may want to set their own name for the source mapping, for instance to help distinguish between two Source Mappings that come from the same source.
* **Description:** Allows the user to briefly describe to other users the use case of the Source Mapping.
* **Auto Add Columns:** Default is ".\*". Regex pattern, all matching column headers will be automatically added to the output.
* **Key History:** Default is false. Output key history for Key Output Sources - ignore for Time Series sources.
* **Post Processing Command:** Default is true. SQL Command to run after output is complete WARNING: This will run live against your destination DB
* **Allow Output Regeneration:** Default is true. If set to false, the output will not be generated if triggered by an output reset or validation reset

## Adding Columns

![](../../.gitbook/assets/addoutputsource.png)

First, to add a single column, click on **Add Column** in the top middle of the screen, seen next to the **Remove All Columns** button in the image above.

Then, when the create column modal opens \(seen in the image below\), a column name must be added. The column name should start with a letter and may contain only letters, numbers, and underscores.

![](../../.gitbook/assets/image%20%28265%29.png)

Optionally, the user can add a description to the column, or explicitly set the datatype of the column. If no datatype is set, the datatype of the column will be automatically inferred based on what source attributes are mapped to the column.

## Aggregate Output Sources

Aggregate outputs allow users to output data at a grain higher than their actual data. A source mapping is set as an aggregate source mapping by checking the **Aggregate** option of the **Operation Type** field on the source mapping details modal. Aggregate outputs are denoted on screen by the icon circled in the picture below:

![](../../.gitbook/assets/aggregateicon.png)

 All columns of an aggregate source mapping fall into one of two categories:

* **GROUPS:** Static fields by which rows with matching values will be grouped together. Akin to columns in a SQL "GROUP BY" clause.
* **MEASURES:** Usually numeric fields that we will perform aggregate operations on.

Which category a column falls under is determined by the expression mapped to it for the aggregate source mapping.

![](../../.gitbook/assets/aggregateexpression.png)

If an aggregate function is selected, the column automatically becomes a **MEASURE** column. Conversely, if no aggregate function is selected, the column automatically becomes a **GROUP** column.

## Source Mapping Menu Controls

To open the Source Mapping control menu, click on the kebab button \(⋮\) at the far right of the **Source/Mapping Name** column of the output mapping

![Source Mapping Menu can be seen on the right](../../.gitbook/assets/image%20%28270%29.png)

* **View/Edit Details:**
* **Automap:**
* **Add&Map All Source Columns:**
* **Clear Mappings:** Clears all expressions mapped to columns from this source mapping. This does not take effect until the output mapping save button and the bottom right hand corner of the screen is pressed.
* **Remove:** Removes the source mapping in question from the output. This does not take effect until the output mapping save button and the bottom right hand corner of the screen is pressed.

{% hint style="warning" %}
Note: It is best practice to manually add all Output Columns when configuring an enterprise grade system to adhere to destination naming convention and semantics.
{% endhint %}

## Column Menu Controls

To add all of the source data columns and map their corresponding source attributes all at once, instead of one by one, click on the kebab button \(⋮\), and press the "Add&Map All Source Columns" button \(circled in the image below\)

![](../../.gitbook/assets/image%20%28266%29.png)

* **View/Edit Details:**
* **Move Left:**
* **Move Right:**
* **Automap:**
* **Clear Mappings:**
* **Hide:**
* **Remove:**

## Mapping Expressions

Once at least one source mapping and column have been created, the user can start mapping attributes by clicking on the empty cell that lies at the intersection of the target source mapping and the target column, example below:

![](../../.gitbook/assets/enteringexpression.png)

Once the user clicks the empty cell they would like to map a value to, the expression entry modal will appear:

![](../../.gitbook/assets/image%20%28264%29.png)

To map an attribute from the target source of the source mapping, type a period "." to reveal a drop down of all attributes of this source. Choose an attribute from the drop down to map it to the target field.  
  
To map an attribute from a source that is related to the target source of the source mapping, type an opening bracket "\[" to reveal a drop down of all sources that have an active primary relation chain to the target source of the source mapping. Once the user has selected a related source, a drop down of the attributes of that source can be revealed by typing a period ".". Then, choose an attribute from the drop down to map it to the target field.

## OLD STUFF IS BELOW THIS

## Column Configurations

_There can only be one column configuration in an Output. Columns represent the final schema of the Output data._

### 



## 

_Source Configurations in the Output Mapping tab determine which data ends up in the Output, allowing users to select multiple sources, filter them based on validation rules, and perform operations on the Source data._



![Remove Source](../../.gitbook/assets/image%20%2864%29.png)

### Source Mapping

Connecting Source data to an Output column requires a mapping. These mappings form the backbone of an Output. 

To create a single mapping, first [create a Column](output-mapping.md#column-configurations). Then, select the corresponding UI row under the desired Source in order to link the Source data to the Output column.

![Map Source data to an Output column](../../.gitbook/assets/image%20%2874%29.png)

To perform more complex mapping operations, click the kebab button \(⋮\) found on the top of the Source header to bring up a list that includes the following mapping options:

* **Automap:** For all existing Columns by the same name, map all data columns in Source
* **Add All Data Columns:** Create and map data columns in Source to existing Columns with the same name, or new ones if Column not yet created
* **Add All System Columns:** Create and map system columns in Source to existing Columns with the same name, or new ones if Column not yet created
* **Clear Mappings:** Clears all mappings from the Source. Does not delete Output Columns

![Source Mapping Options](../../.gitbook/assets/image%20%28163%29.png)

To map multiple Sources to a single Column, first ensure that both relevant Sources are visible via the [Show/Hide Sources](output-mapping.md#source-selection) button. Then, individually map each Source to the desired Column. In the Output, the data from each Source will be appended, **not Joined.** Use this functionality when mapping Columns from multiple Sources that represent the same Output Column. For example:

* Creating an output of Divvy Stations with a Source of Stations from Chicago and a Source of Stations from the suburbs.
* Creating an output of Vehicles that imports a Cars Source and a Trucks Source, both of which share a `License Plate` Column.

![Multiple Sources mapped to one Column](../../.gitbook/assets/image%20%28111%29.png)

## Output Source Details

To open the Output Source Details modal, select the Source header.

![Open Output Source Details](../../.gitbook/assets/image%20%28228%29.png)

![Output Source Details - File Output Type](../../.gitbook/assets/image%20%28230%29.png)

### Filter Expression

Output Source Details requires a Filter Expression. Data is included by the filter when the specified expression evaluates to `true`.

{% hint style="info" %}
Note: The supported syntax in the expression input is specific to PostgreSQL. Refer to PostgreSQL documentation: [https://www.postgresql.org/docs/10/functions.html](https://www.postgresql.org/docs/10/functions.html)
{% endhint %}

### Parameters

* **Include Rows:** Filters additionally based on flags set during the [Source Validation]() step
* **Name:** Names the Source with the applied logic. Name appears atop the Source UI column
* **Description:** Description of the Source
* **Active:** If set to Active, the filter will be applied

#### Operation Type

{% tabs %}
{% tab title="N/A" %}
**N/A** applies no operations on the Source data.
{% endtab %}

{% tab title="Distinct" %}
**Distinct** applies a `DISTINCT` filter on output columns to remove duplicates. An additional parameter,  **Column List** appears when Distinct is selected. 

* **Column List** - Select the list of columns that a `DISTINCT` statement will be applied to.
{% endtab %}

{% tab title="Unpivot" %}
**Unpivot** applies an `UNPIVOT` filter on output columns to remove duplicates. An additional parameter,  **Column List** appears when Unpivot is selected. 

* **Column List** - Select the list of columns that a `UNPIVOT` statement will be applied to.
{% endtab %}
{% endtabs %}

### Additional Parameters

These parameters appear depending on the user's Output Type selection.

| Filter Appears Under | Parameter | Default Value | Description | Advanced |
| :--- | :--- | :--- | :--- | :--- |
| Virtual, Table, SFTP | key\_history | FALSE | Output key history for Key Output Sources - ignore for Time Series sources | N |
| Virtual, Table | manage\_table | TRUE | True or false, decides if you want table to be altered if there are missing columns | Y |
| Virtual, Table | table\_name\* |  | Table name for destination table | N |
| Virtual, Table | table\_schema\* |  | Schema name for destination table | N |
| Virtual, Table | delete | none | Choice of how we handle the output data into the destination - Options: none, all, input, key, range | N |
| SFTP | file\_mask | FileName&lt;TSHH12MISS&gt;.csv | File mask for output file | N |
| SFTP | connection\_name\* |  | Connection name for the destination | N |
| SFTP | partition | segment | Partitioning strategy for files {input, segment} | N |

