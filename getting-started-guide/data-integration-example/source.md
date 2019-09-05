---
description: >-
  This section describes the process of setting up a Source, including the
  Source Details, Input Parameters, and Staging Parameters.
---

# Source

## Step 1: Create a Source

Navigate to the Sources screen through the Menu, then create and name a new Source in the same fashion as [input and output connections](connection.md). Be sure to again follow [Naming Conventions](). In this case, it is  recommended to use `Divvy - Stations 2017 Q1Q2`.

{% hint style="info" %}
The below image shows some available controls that we will not use in this basic example. Because these are more advanced features, this section only provides a brief description of each.
{% endhint %}

![Extra Options; Leave as-is](../../.gitbook/assets/screenshot_5%20%283%29.png)

{% tabs %}
{% tab title="Hide Advanced Parameters" %}
**Hide Advanced Parameters** reveals advanced source parameters when turned off.
{% endtab %}

{% tab title="Active" %}
**Active** is used to deactivate or reactivate a source. By default, all sources are active when created. Deactivating a source removes it from the active sources list, and stops any schedules, file watches, and processes from running on the source. Once this source is reactivated, it can begin processing again.
{% endtab %}

{% tab title="Agent" %}
The RAP **Agent** establishes a secure connection between data sources and the RAP instance. It uploads them securely and quickly into RAP.
{% endtab %}

{% tab title="Default" %}
**Default** sets the current Agent to be the default Agent when configuring future Sources. It is common to create many Sources that all use the same Agent.
{% endtab %}
{% endtabs %}

## Step 2: Configure Source Details

There are three configuration categories which define how the system should Input and Stage data:

![Source Detail Options](../../.gitbook/assets/screenshot_4.png)

### Source Type

Select `Key`. The Divvy Stations file is a Keyed source. Each record represents a station.

{% tabs %}
{% tab title="Keyed" %}
**Keyed** sources are data sets containing a unique identifier or key tied to a logical entity. These can be used as lookups from other sources. Time Series sources cannot be used as lookups.
{% endtab %}

{% tab title="Time Series" %}
**Time Series** sources represent transactional sources. Rather than a unique identifier per entity \(such as a Divvy Station\), they have a identifier to track individual _events,_ typically a timestamp.
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
Be sure to use `Divvy_Stations_2017_Q1Q2.csv -`the other files packaged in the original zip file are Time Series.
{% endhint %}

### Input Type

Select `File Push`

{% tabs %}
{% tab title="File Pull" %}
A flat file to be ingested at a scheduled time and cadence.
{% endtab %}

{% tab title="File Push" %}
A file watcher which monitors a folder path to ingest files as soon as they become available.
{% endtab %}

{% tab title="Table Pull" %}
A database table to be ingested at a scheduled time and cadence.
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
When selecting an Input Type, the screen fields will dynamically update in the Schedule and Input Parameters sections of the Source configuration. Reference the [Configuration Guide](../../configuration-guide/) for more details on input type attributes.
{% endhint %}

### **File Type**

Select `Delimited`. Divvy Stations has a delimiter that separates data fields.

{% tabs %}
{% tab title="Delimited" %}
A **Delimited** file is a file where each line has fields separated by a delimiter, representing a new record. The most common format is a CSV, where each each record is separated by a comma. 
{% endtab %}

{% tab title="Fixed Width" %}
A **fixed width** file consists of records with specified length per row, and specified length per column, rather than any delimiters. While less human-readable, they are much more compact, and thus useful for large data volumes.
{% endtab %}
{% endtabs %}

## **Step 3: Specify Input Parameters**

Input parameters specify where within the connection the data exists, and how to retrieve it.

**file\_mask:** Enter `Divvy_Stations*.csv`. The **file\_mask** represents the _name_ of the file. Using a mask allows the selection of multiple files that fit a specific pattern.

{% hint style="info" %}
In RAP, a mask uses \* for representing a variable block of text. For example, `TestData*.csv` matches both `TestData04.09.2018.csv` and `TestData04.10.2018.csv`, as well as just `TestData.csv`.
{% endhint %}

{% hint style="info" %}
File masks typically help with automated daily loads from external exported data. In this scenario, the exporting system typically post-pends the file name with the timestamp of extract. By using the file mask, RAP can identify these files as generated from the same system, and process them within one source.
{% endhint %}

**connection\_name:** Select the input connection configured earlier: `Divvy - Input Path` . For help configuring the connection, see [Connection Configuration](connection.md).

## **Step 4: Specify Staging Parameters**

_The Staging Phase details how RAP reads and stores the Source._

**key\_columns:** Enter `id` . These are the primary keys for the input CSV: Divvy Stations uses `id` as its primary key.

## Step 5: Save

Click the **Save** button to save the Source; all parameters should be configured. Upon saving the Source, users will be redirected to the Source details view.

![Source Details](../../.gitbook/assets/image%20%28146%29.png)

_RAP now has all the information it needs to complete the Input & Staging phases, allowing the source data to be ingested, read, and written into the RAP internal storage database._

## Step 6: Validate File Input

Check the Inputs tab at the top of the page to verify that the file has been successfully pushed to the system.

![Input Found &amp; Ingested](../../.gitbook/assets/screenshot_9%20%281%29.png)

{% hint style="info" %}
RAP will automatically begin the Input phase when the input files appear in the Connection specified earlier. These files will disappear once RAP ingests them.
{% endhint %}

