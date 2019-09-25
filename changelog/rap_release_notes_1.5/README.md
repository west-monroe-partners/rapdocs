# 1.5

### Enhancements

| VSTS ID | Description | Impact | Risks |
| :--- | :--- | :--- | :--- |
| 36587 | Added Mass Output functionality | Output will now wait for processing to finish within the Source before running. All inputs that need to be Output will be combined into one Output run when processing finishes. Effective ranges are now calculuated for Time Series outputs to help reduce range delete errors and to eliminate redundant processing. Key Deletes are have been optimized. | High - Major change to Output processing and key and range calculations. Will need to be smoke tested heavily in Dev client environments |
| 41152 | Delete Input is a now a process and uses the process queue accordingly | Delete Input was causing some trouble by running ad hoc on the platform. Now, deleting an input is a process that gets queued up and allocated through the connection manager to run more smoothly with the other processes that may be running on the same source. | Medium - Deleting inputs is always a dangerous task, but should prevent collision of processes moving forward |
| 40175 | Partitioning is now available for use in the Staging process | A need for partitioned landing tables for inputs arose when window functions started popping up in Enrichment rules. Users can now specify a partition key \(can be a composite key\), as well as an estimated table batch size. During Staging, the data will be partitioned on the key. | Medium - This is a large addition to Staging, but is also net new functionality. No existing sources will be affected |
| 42231 | Input delete type added to Output | Users will now be allowed to delete on the input grain. Instead of using keys or ranges, the data will be deleted based on which Input Id's are included in the Output run. | Low - New delete type that previously didn't exist. Users should be wary of what will happen if existing Outputs are changed to this type |
| 42053 | Clean up orphaned input files in staging folder and input archive folder | When inputs are deleted from the Inputs tab, the underlying source files still remain in the staging folder, or the input archive folder if they've been archived. Now, the Cleanup process will remove these files. | Low - The files being removed are already unrecoverable into the platform |
| 41722 | Ability to override Agent jar location for auto updates | The location that the auto update functionality looks to was an API level parameter. Now, an override location can be specified in the Agent parameters to allow. | Low - If the incorrect location is entered, could cause the Agent to get out of date |
| 39191 | Trim header and/or data columns in the Staging process | Two parameters are added to the Staging parameters to control header and data whitespace trimming. The options are none, leading spaces, trailing spaces, or both. | Low - Trimming the data can cause existing lookup rules to fail matches if they were working with the whitespaces |
| 38322 | Automatically generate Clustered Columnstore Indexes for SQL Server Time Series Outputs | Output will now auto-generate a Clustered Columnstore Index for these Outputs as well as a SQL Agent job to rebuild the index automatically on a schedule. This will only work with on-premise SQL Server. | Low - Failure to create the index/agent job can cause Table Output to fail processing |
| 42558 | Display a more informative error message to the user when a source file is missing during Staging | When the application looks to the staged file directory, it used to display a confusing message when it couldn't find the specified source file to stage. Now the user will get a more accurate description of the error. | Limited/None - This is an updated error message |
| 41998 | Added description field to Output Source | Users can now enter in a description field to help differentiate Output Sources. | Limited/None - Purely a cosmetic change |
| 42497 | Support for Agent to run on RedHat Linux 6.1 | We now can deploy the Agent to a Redhat Linux 6.1 server. | Limited/None - New environment, does not affect any other Agent installations |
| 42263 | Connection parameters added for new environment database deploy | These parameters need to be added to allow initial database creation and deployment for new environments | Limited/None - Doesn't affect existing environments, only initial deployments |
| 41687 | Platform Import/Export functionality - Dependencies | Added Dependency export capabilities based on source id as well as import capabilities. User will be warned if source doesn't exist | Limited/None - Affects import and export functions, which are not involved in processing |
| 41686 | Platform Import/Export functionality - Validation Rule Types | Added Validation Rule Type export capabilities as well as import capabilities | Limited/None - Affects import and export functions, which are not involved in processing |
| 39127 | Process tab added to UI to allow visualization of the Process Queue | With the addition of the process queue, users wanted the ability to see what was queued up for processing in the platform. This tab in the UI displayed queued, running, and historical processes | Limited/None - This is a cosmetic enhancement to the platform |

### Bugfixes

| VSTS ID | Description | Impact | Risks |
| :--- | :--- | :--- | :--- |
| 42422 | Range delete fails if transaction start or end times are null | This was generally an issue with 0 record Time Series inputs. Now, these inputs will be handled during the new Mass Output enhancement during effective range calculation | Medium - The bug is fixed in the Mass Output enhancement, which is a large processing change |
| 42281 | Source file size not being recorded during Table Input | We were failing to capture the source file size during the Table Input process. We need this information for the new Configurable Partitioning enhancement to dynamically calculate partitions | Low - Added an extra parameter to a Json object in the Agent |
| 42176 | Dependency queue passes source dependencies with blank transaction end datetime | Source dependencies that had at least one input with a NULL transaction end datetime were bypassing dependency logic and progressing to the next phases | Low - This is a processing logic change but only affects inputs that should have been held back in the first place |
| 41466 | Enrichment Rule Template changes not being enforced on Validation Rule Bridges | Changes made in template were not propagating down to the individual enrichment rules. | Low - UI fix that doesn't affect processing, just updating of existing rules |
| 42054 | Start datetime value is not updated in the process table for the dependency\_queue\_check operation | This is a minor bug that made debugging process runtimes difficult when they involved the dependency\_queue\_check operation | Limited/None - The start datetime value is just used for logging |

### Legend

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

