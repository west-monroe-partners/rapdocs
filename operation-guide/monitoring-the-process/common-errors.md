# Common Errors

This section lists some of the common errors for each phase of RAP that developers encounter. This is not an exhaustive list of errors, but it should provide some guidance to help with troubleshooting.

## Input

Issue: A File Watcher Source is active or A File Source attempted to ingest a file on a schedule, but no files have been found that match the File Mask parameter in the specified Connection  File Path.

Possible Solutions:   

1. Ensure that the file exists in the Connection path before ingesting the file.
2. Check the Connection's File Path parameter and correct any spelling/typing mistakes.

Issue: A Table Source fails to ingest data.

Possible Solutions:

1. The Source query is incorrect. Fix any syntax errors and ensure the column names are referenced correctly in the query.
2. Check the Connection Settings and verify that all parameters are correct.

Issue: An S3 connectivity issue has occurred.

Possible Solution: The connectivity to S3 may be inconsistent. To recover, delete the failed inputs and attempt to ingest the data again.

## Staging

Issue: The Staging phase failed for the ingested Input data.

Possible Solutions:

1. The line terminators \(line ending characters\) in the source data and the Source parameters in RAP do not match. Ensure that the line terminators match by either changing them in the source data or changing the Source parameters in RAP so that they match. Line terminators are normally not visible, so certain text editors such as Notepad++ are useful for this, as they allow users to view line terminators.
2. There is at least one line terminator in the source data that is placed incorrectly. An example of this is when the line terminator is incorrectly inside a data value in a CSV file. This issue must be fixed in the source data. Ensure that no line terminators are placed within data values.



