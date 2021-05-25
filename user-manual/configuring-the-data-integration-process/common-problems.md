# !! Common Problems

This section is dedicated to common issues that arise and are solved by tuning configuration parameters.

My file did not read into Intellio DataOps \(RAP\)

My file read incorrectly into Intellio DataOps \(RAP\) 



Switching from Timestamp to Key refresh type throws the following error when pulling a new input:

`Capture Data Changes failed with error: Range column defined but null data present in the entire column. Please check source data & make sure the datetime_format parameter on your Source Settings page is correct in order to parse the timestamp data. More info on datetime_format parameter can be found here:` [`https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html`](https://docs.oracle.com/javase/7/docs/api/java/text/SimpleDateFormat.html)\`\`

Solution:  Date column is from the Timestamp refresh is retained, and some data has NULLs for that column in the source.  If that field no longer needs to be used for any change tracking purposes, if should be removed as a date column.  This can be done by switching back to Timestamp refresh, clearing out the Date column, switching back to Key refresh then re-saving the source.

