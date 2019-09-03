# 1.2

## Enhancements

* New processing architecture - Postgres queueing will make for smoother, faster, more reliable processing 
* Autoreprocessing Logic \(Applicable to MDM use cases\) - We will be scheduling a KT session to explain this feature to users. 
* New Lookup Rule Parameter: Automatic Reprocessing - Use this to specify whether data is updated via autoreprocessing logic when new or changed keyed records are added. 
* New UI icons, Queued and Old - Queued indicates that the process is waiting in the system, but not actively executing, Old indicates that an input's data tables have fallen out of sync with Enrichment Rule Columns. Just revalidate to get rid of the "Old" status. 
* Improved Logging - Added processId and inputId to log messages -Improved performance of query joins for faster validation and enrichment

## Bug fixes

* Starting a large number of revalidations should no longer cause the system to crash.

## Known issues

* Lookup Rules with a newline character at the end of their lookup statement are at risk of failing with the following error: ERROR: table name "l\_276\_1" specified more than once. This can be fixed by removing the newline character from the rule in question.

