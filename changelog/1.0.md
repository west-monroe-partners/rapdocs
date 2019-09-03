# 1.0

## Enhancements

* Display warning messages when users try to map datatypes incorrectly
* When an enrichment rule is added or removed that affects the priority groups of the source, the priority groups are now recalcuated automatically
* Changing name in output mapping does not remove destination column
* Changing data types in output mapping will now display a warning message
* Output has been refactored to use Akka Streams for SQL Server insert and file output, new parameters added

    postgres\_concurrency: thread count coming from Postgres source data

    sql\_server\_concurrency: thread count for insert into SQL Server destination 

* SFTP Output is now supported
* UI updates made to Source Dashboard
* Added support for complex lookups in Enrichment phase
* Cron field descriptions and instructions updated
* Staging streams data from S3 instead of moving file to ETL box, source parameter added

    Staging Path: this is the path that the source will stage from, it will be defaulted to an S3 path

* Lookup source search added and lookup sources are listed alphabetically in Enrichment rule creation
* Delete all output column functionality added
* Inputs sorted in descending order, with most recent at the top

## Bug fixes

* Output column names with invalid characters no longer permitted
* Added error message during creation of lookup rules when data table is empty
* Can now view more than 100 inputs for any one source
* Made cleanup process handle missing output files correctly
* UI routes to sources/\[new source\] after saving the new source instead of sources/\[new\]
* Output source filter condition fixed and displays warning messages now
* Process time now accurately represented on inputs page
* Lookup grain preservation functionality now properly applies lookup expression
* Change events when updating rules and sources fixed

## Known issues \(Expected hotfix by 7/5/2018\)

* Old outputs will need to be resaved before being ran/reset due to parameter changes/additions
* Any lookup expression that does not use L.s\_key as its lookup field will need a lookup order by applied
* Output SFTP will fail if postgres concurrency is set higher than 1 \(Resolution TBD\)

