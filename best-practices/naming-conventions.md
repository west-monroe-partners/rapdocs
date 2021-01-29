---
description: >-
  Suggested standards for naming objects both within DataOps and at handoff
  points.
---

# !! Naming Conventions

A consistent naming convention for source names and enrichment names can make the difference between an easily maintainable solution and one that is difficult to understand.  This section suggests a naming convention that project teams can leverage when starting a new implementation.

{% hint style="info" %}
Note that implementation teams may leverage a different standard that that proposed here, whether that is due to customer naming requirements or specialized business rules where a different naming convention is preferable.  On existing implementations, if a naming convention is already in place, follow the existing naming convention over the convention proposed here.
{% endhint %}

### Source Names

#### Table-based Sources

Table-based sources should be named according to the following convention:

**`<Source System> - <Table / Entity Name> - <Refresh Type>`**

**&lt;Source System&gt;** is the name of the source system where data is being pulled \(ERP, CRM, HR, etc\). In the scenario where the source is a loopback may be needed, the loopback source should be denoted by adding “Loopback - ” as a prefix to the originating source system name, and the table / entity name should be a very short description of the grain change occurring \(e.g., Sales Order to Customer\).

On multi-tenant implementations, the source system name should also take into account the division / company name that is being ingested.  One situation where this is needed is when each division / company uses their own separate instance of the same ERP system.  In this case, prefix the source system name with the division or company name \(ex: instead of just "ERP", use "Company 1 ERP" as the source system name\).

**&lt;Table / Entity Name&gt;** is the exact name of the table or entity being pulled.

**&lt;Refresh Type&gt;** is the refresh type of the source. This should match with the refresh type that is set up for the source, which can be one of the following values:

* Full
* Key
* None
* Sequence
* Timestamp

As an example, to pull the Customer table from ERP as a Key refresh source, the source should be named “ERP – Customer – Key”.

### Validation and Enrichment Names

Validation and Enrichment rules \(both within sources and templates\) should be named according to the following convention:

**`<Operation Type> - <Source System> - <Rule Description>`**

**&lt;Operation Type&gt;** can be one of the following values:

* **Validation**: For validation rules
* **Conversion**: For rules performing strictly datatype conversions
* **Enrichment**: For rules adding an enriched field performing any type of calculation / derivation / hard coding

**&lt;Source System&gt;** is the name of the source system where data is being pulled \(ERP, CRM, etc\). This is done to facilitate the ease of converting rules over to templates as needed. In the scenario where a rule is intended to be used globally across different source systems, the value “Global” should be used.

As an example, to add a field called “customer\_full\_name” to an ERP source, the enrichment rule should be named “Enrichment – ERP – Customer Full Name”.

### Relation Names

Relations should be named according to the following convention:

**`<Source 1 Name> (<Cardinality 1>) to <Source 2 Name> (<Cardinality 2>)`**

**&lt;Source 1 Name&gt;** and **&lt;Source 2 Name&gt;** are the names of the two sources on either side of the Relation.

**&lt;Cardinality 1&gt;** and **&lt;Cardinality 2&gt;** are the cardinalities of the respective sources on either side of the Relation.  Values here can be "ONE" or "MANY".

### Output Names

Outputs should be named according to the following convention:

**`<Output System> - <Table / Entity Name>`**

**&lt;Output System&gt;** is the name of the system where data is being outputted to \(DW, Data Lake, etc\). In the scenario where the output is being written out to the Data Lake to facilitate a loopback source, the value should be denoted as “Loopback”.

As an example, to output to the f\_sales table in the Data Warehouse, the output should be named “DW – f\_sales”.

### Channel \(Output Source\) Names

Channels are given the same name as the Source name by default by DataOps.  However, this can be confusing for new developers / configurators in understanding what that output grain means or what it is used for, since the source name is more focused on describing the source and not the destination.  Therefore, the recommended convention is to use the name of the output grain instead \(ex: Sales Order, Purchase Order, Inventory Snapshot, etc\).  Since DataOps shows the Source name next to the Channel name in the Output Mappings, having those 2 values be different is helpful to give a full picture of what the source data is and the grain that plays in the output.

### File / Folder Names

#### Inbox Folder Structure

The simplest scenario where it is an option would be to drop files in DataOps' internal "inbox".  This will be a special account or container that exists in AWS / Azure for the purpose of ingesting file-based data into DataOps.  The recommendation is to create a top-level Inbox folder on the storage account or bucket, then point the connection in DataOps to that Inbox folder.

In some cases, both the Development and Production environments may leverage the same storage account / container for the DataOps inbox.  In this case, the recommendation is to create a DEV and PROD folder as the top level folders and to have the Inbox folder exist as the next level down.  All files ingested into DataOps from the Inbox should come from the folder structure for the DEV or PROD environment respectively.  When the two environments use separate storage buckets / containers, environment folders can be omitted.

Within the root of the Inbox folder, a sub-folder should be created for each input system.  As upstream file names are frequently out of the control of the implementation team, the separate folders will help keep files organized and avoid naming conflicts.

#### On-Premise Folder Sources

For on-premise source folders \(Windows file shares or local directories\), many times those structures are controlled and standardized based on customer standards or prior server usage.  However, as much as possible, leveraging the same guidelines as the Inbox folder structure defined in the previous section would still be recommended to minimize file naming / search pattern collisions.

#### Output Files for Downstream Consumers

Output file names are frequently driven by the naming convention required by the downstream system ingesting those files.  In order to prevent naming collisions / confusion, each downstream system should have its own folder in the Output container coming out of DataOps.

#### Loopback Sources

{% hint style="info" %}
Before implementing a loopback source, consider if there are any alternative approaches that can be used instead of loopbacks.  Loopbacks add extra complexity and I/O and should only be leveraged when no other \(more performant\) options exist.
{% endhint %}

Loopbacks are a special use case for leveraging virtual outputs.  DataOps is able to automatically re-ingest virtual outputs to a Loopback source as soon as the virtual output is updated.  The virtual output can also be leveraged directly through the Hive metastore as a normal virtual output if desired.

Specific guidelines for Loopback naming are the following:

1. Loopback sources should be named according to the following convention:
   1. **`Loopback - <Ultimate_Source_System_Name> - <Original_Grain> to <New_Grain> - <Refresh Type>`**
      1. **&lt;Ultimate\_Source\_System\_Name&gt;** is the original source system where the loopback originates \(see Source System naming convention earlier in this document for the suggested convention\).
      2. **&lt;Original\_Grain&gt;** should be descriptive of the grain where the loopback originates from.
      3. **&lt;New\_Grain&gt;** should be descriptive of the new grain of data that is being created as part of this loopback.
      4. **&lt;Refresh Type&gt;** is the refresh type of the loopback source.  Refer to the Source naming convention earlier in this document for the suggested values.
2. The virtual output \(and associated channel\) should be named according to the output naming convention earlier in this document.  If the virtual output is being used strictly for the purpose of a loopback, the suggested name of the **&lt;Output System&gt;** value in the convention is "Loopback".

