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
3. The number of data fields parsed at a certain line in the data does not match the number of column headers. This is likely caused by a text qualifier mismatch. The text qualifier for CSV files is the double-quote character \("\) by default. Make sure the text qualifier parameter in RAP matches the text qualifiers in the source data.

## Enrichment

Issue: An Enrichment fails to convert a data column to the numeric data type.

Possible Solution: A record in the original column may have non-numerical data that cannot be converted to numeric data. Either fix the errors in the data source or delete the problematic data.

Issue: One or more Key Inputs are stuck in the "waiting" status.

Possible Solutions:

1. If previous Inputs have failed the Input, Staging, or Enrichment phases, resolve the errors on those Inputs. For example, an Input that is waiting on the Enrichment phase will release as soon as all errors on previous Inputs are resolved \(and no other dependencies exist for that Source\).
2. The Source that contains the waiting input may depend on other Sources to finish processing. In this case it is best to check the Workflow Queue. The Workflow Queue can be accessed through the Processing page from the hamburger menu at the top-left of the screen.

## Output

Issue: The Output phase fails on a Source.

Possible Solutions:

1. There may be a Output mapping type mismatch \(available for Table and Virtual Outputs only\). For example, mapping a text field to the numeric type will fail. If the requirements call for the Output column to be a numeric type and the text can be converted to numeric successfully, convert the text in an enrichment rule. Otherwise, fix the issue at the source.
2. The destination database for the Output is incorrect. Ensure that the Database Name parameter in the Output Details is correct, then reset Output.   

