---
description: Suggested standards for naming objects in RAP.
---

# Naming Conventions

A consistent naming convention for source names and enrichment names can make the difference between an easily maintainable solution and one that is difficult to understand.  This section suggests a naming convention that project teams can leverage when starting a new implementation.

### Source Names

#### Table-based Sources

Table-based sources should be named according to the following convention:

`<Source System> - <Table / Entity Name> - <Refresh Type>`

**&lt;Source System&gt;** is the name of the source system where data is being pulled \(ERP, CRM, HR, etc\). In the scenario where a loopback may be needed, the loopback source should be denoted by adding “ – Loopback” to the originating source system name, and the table / entity name should be a very short description of the grain change occurring \(e.g., Sales Order to Customer\).

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

`<Operation Type> - <Source System> - <Rule Description>`

**&lt;Operation Type&gt;** can be one of the following values:

* **Validation**: For validation rules
* **Conversion**: For rules performing strictly datatype conversions
* **Enrichment**: For rules adding an enriched field performing any type of calculation / derivation / hard coding

**&lt;Source System&gt;** is the name of the source system where data is being pulled \(ERP, CRM, etc\). This is done to facilitate the ease of converting rules over to templates as needed. In the scenario where a rule is intended to be used globally across different source systems, the value “Global” should be used.

As an example, to add a field called “customer\_full\_name” to an ERP source, the enrichment rule should be named “Enrichment – ERP – Customer Full Name”.

### Output Names

Outputs should be named according to the following convention:

`<Output System> - <Table / Entity Name>`

&lt;Output System&gt; is the name of the system where data is being outputted to \(DW, Data Lake, etc\). In the scenario where the output is being written out to the Data Lake to facilitate a loopback source, the value should be denoted as “Loopback”.

As an example, to output to the f\_sales table in the Data Warehouse, the output should be named “DW – f\_sales”.

### File Names and Bucket Usage

TODO - loopback naming, file naming, S3 bucket / ADLS container usage

#### Input Folder Structure

TODO - local agent vs. RAP inbox in AWS / Azure

The simplest scenario where it is an option would be to drop files in RAP's internal "inbox".  This will be a special account or container that exists in AWS / Azure for the purpose of ingesting file-based data into RAP.  In most cases, both the Development and Production environments will leverage the same storage account / container for the RAP inbox.  Therefore, the recommendation is to create a DEV and PROD folder as the top level folders in the inbox.  All files ingested into RAP from the Inbox should come from the folder structure for the DEV or PROD environment respectively.

Within those those environment folders, a sub-folder should be created for each input system.  As upstream file names are frequently out of the control of the implementation team, the separate folders will help keep files organized and avoid naming conflicts.

#### Output Files for Downstream Consumers

Output file names are frequently driven by the naming convention required by the downstream system ingesting those files.  In order to prevent naming collisions / confusion, each downstream system should have its own folder in the Output container coming out of RAP.

#### Loopback Files

Loopback files are a special case of file outputs.  As loopbacks are only created to the specific use case of re-ingestion back into RAP, those files are not intended to be used for consumption by any users and are transient in that they exist only until they are re-ingested.  This will be the only scenario where output files should be written to the Inbox container.

Specific guidelines for loopback file naming and placement are the following:

1. Create a folder called "Loopback" in RAP's inbox.  This folder should only ever be used to output loopback files and should not be used to expose data to external systems.

