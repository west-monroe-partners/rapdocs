# Common Errors

This section lists some of the common errors for each phase of RAP that developers encounter. This is not an exhaustive list of errors, but it should provide some guidance to help with troubleshooting.

## Input

Cause: A File Watcher Source is active or A File Source attempted to ingest a file on a schedule, but no files have been found that match the File Mask parameter in the specified Connection  File Path.

Possible Solutions:   

1. Ensure that the file exists in the Connection path before ingesting the file.
2. Check the Connection's File Path parameter and correct any spelling/typing mistakes.

Cause: A Table Source fails to ingest data.

Possible Solutions:

1. The Source query is incorrect. Fix any syntax errors and ensure the column names are referenced correctly in the query.
2. Check the Connection Settings and verify that all parameters are correct.

Cause: An S3 connectivity issue has occurred.

Possible Solution: The connectivity to S3 may be inconsistent. To recover, delete the failed inputs and attempt to ingest the data again.



