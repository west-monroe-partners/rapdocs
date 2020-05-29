# Storage Data Model

TODO - write an intro

TODO - RAP uses Avro / Parquet files and Hive tables now, sections below get replaced

### Working Data \(work\)

The **work** schema is used for intermediate processing by RAP.  All data in this schema is intended to be used internally for RAP processing only and cannot be directly exposed via an output.

The different types of tables in this schema are the following:

TODO:  list them out \(lookup and intermediate tables\), what do \#'s in table names mean

### Processed Data \(data\)

The **data** schema contains all processed data that is ready to be used for lookups or output to an external destination.  This can be thought of as the data hub layer, as data cleansing and enrichments are already complete, and the last step is to write that data out to a reporting warehouse or flat files to an external system.

The different types of tables in this schema are the following:

TODO:  list out abbreviations, what do \#'s in table names mean

