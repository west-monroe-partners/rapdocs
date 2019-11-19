# 1.7.1



## Features

### Source Type And Range Type Converted To Data Refresh Type

* **JIRA ID**: PROD-590
* **Problem**: We wanted to simplify Source creation choices - as well as have room to easily add new source types, so we've condensed the Source Type and Range Type radio buttons into Data Refresh Type
* **Impact**: Data Refresh Type will now be the options: Key, Timestamp, Sequence, Full, None. These correspond to the previous configurations with the only addition being Full. All sources will be migrated to the new types during the deployment of 1.7.1.0.
* **Risks**: Medium

### Input Type Converted To Connection And Initiation Types

* **JIRA ID**: PROD-591, PROD-592
* **Problem**: Simplifying and expanding design room also had the Input Type radio buttons split out into Connection Type and Initiation Type.
* **Impact**: Connection Type will be the system that you're acquiring the Input from: File, Table, SFTP. Initiation Type will be the how the Agent will kick off the Input creation: Scheduled, Watcher \(push\). All sources will be migrated to the new types during the deployment of 1.7.1.0.
* **Risks**: Medium

### Sources Page Updated

* **JIRA ID**: PROD-569, PROD-795
* **Problem**: The sources page needed an update to reflect the new Data Refresh, Connection, and Initiation Types. Agent Code is shown and searchable as well.
* **Impact**: Users will be able to see more information about their sources from the Sources screen.
* **Risks**: Limited/None

### Full Data Refresh Type Added

* **JIRA ID**: PROD-594, PROD-595,
* **Problem**: There was a need for a Data Refresh type that would handle pulling all the data from a source every day with the latest input being the only relevant input.
* **Impact**: Users will be able to select and use the Full Data Refresh type in the Source details tab
* **Risks**: Limited/None

### Agent Screen Added

* **JIRA ID**: PROD-743, PROD-915
* **Problem**: The only place to view Agent status and logs was on the backend via querying Postgres
* **Impact**: The new Agent screen will allow users to view the Agent health status, configured plugins, and Agent specific logs.
* **Risks**: Limited/None

### 

## Enhancements

### V&E Templates Tab Renamed To Templates

* **JIRA ID**: PROD-701
* **Problem**: The V&E Templates tab was a little too wordy.
* **Impact**: Users will now just click Templates in the Side menu
* **Risks**: Limited/None

### Reset Buttons Disabled When Inputs Are In Progress

* **JIRA ID**: PROD-724
* **Problem**: Users were able to reset inputs when they were still processing
* **Impact**: Resetting an input during processing causes issues with locking and inaccurate results on the backend
* **Risks**: Limited/None

### Reset Staging Disabled When Input Source File Is Cleaned Up

* **JIRA ID**: PROD-724, PROD-889
* **Problem**: When the Cleanup process deleted an Input's source file, the user would still be able to reset staging on the Input
* **Impact**: Users will no longer be able to reset staging in this scenario, saving the user from seeing a difficult to troubleshoot error
* **Risks**: Limited/None

### Connection Refactoring

* **JIRA ID**: PROD-816
* **Problem**: Connections were hard to understand when viewing a source from the Source Details tab
* **Impact**: Connection has been moved up to a searchable dropdown in the Source Details tab and will display where the connection points to in the dropdown.
* **Risks**: Low

### Process Screen Updates

* **JIRA ID**: PROD-836
* **Problem**: Process Screen needed some Quality Of Life updates
* **Impact**: Total Duration replaced with Execution Duration, Batch ID now searchable, pagination replaced with infinite scrolling, added process end time
* **Risks**: Limited/None

### Orchestrator Logging Updates

* **JIRA ID**: PROD-890, PROD-893, PROD-947, PROD-962
* **Problem**: Orchestrator database and file logging needed some Quality Of Life updates in order to better troubleshoot processes and errors as they move through the platform
* **Impact**: Actor names have been added to the log.actor\_log table, timestamps are now shown in file logs, file logs have been reformatted to be easier to read, database logging is more informative when errors are thrown
* **Risks**: Limited/None

### Agent Logging Updates

* **JIRA ID**: PROD-891, PROD-904, PROD-908, PROD-956
* **Problem**: Agent database and file logging needed some Quality Of Life updates in order to better troubleshoot processes and errors as they move through the platform
* **Impact**: Agent logs have been segmented into the log.agent table, Actor names are now logged in the log.agent table, Agent Log screen shows heartbeat interruption logs, Server name and IP are logged when the Agent starts up, timestamps are now shown in file logs, file logs have been reformatted to be easier to read, file logs are more informative when API errors are thrown
* **Risks**: Limited/None

### Connection Screen Updates

* **JIRA ID**: PROD-905
* **Problem**: Connection Screen needed to display more configuration information to users
* **Impact**: Connection Screen now shows the Connection type and Connection path as well as the Connection name
* **Risks**: Limited/None

### Processed Record Count Correctly Updated When Reset All V&E Is Used On Keyed Source

* **JIRA ID**: PROD-1012
* **Problem**: When resetting all V&E for a Keyed source, the processed record counts do not show accurately
* **Impact**: Processed record counts will now be shown at the most recent input for Keyed source
* **Risks**: Limited/None

### Archiving And Deletion Of Files In S3 Removed From Cleanup

* **JIRA ID**: PROD-941
* **Problem**: Archiving files by moving them to another S3 bucket turned out to be useless and caused more harm than good. Low costs of storage in S3 makes it unnecessary to delete files aggressively.
* **Impact**: Source files will now be touched much less by the Cleanup process
* **Risks**: Limited/None

### Orchestrator Handling Manual Reset Processes Better Upon Restart

* **JIRA ID**: PROD-551
* **Problem**: The Orchestrator would often times queue up multiple processes for an input when restarting while inputs were processing in the platform
* **Impact**: The Orchestrator will now be able to handle these situations much better upon restart of the application
* **Risks**: Limited/None

## Bugfixes

### Unable To Read Log In Output History

* **JIRA ID:** PROD-793
* **Problem**: When clicking into an Output History status, users were unable to read the log messages
* **Impact**: Users will now be able to see the Output History log messages
* **Risks**: Limited/None

### Trim Whitespaces On Output Table And Schema Name On Save

* **JIRA ID**: PROD-824
* **Problem**: Leaving whitespace at the end of table and schema names during Output config would cause errors during the Output process
* **Impact**: Saving the Output Config will now trim the whitespace from these fields.
* **Risks**: Limited/None

### Dependency Update Not Releasing Downstream Process

* **JIRA ID**: PROD-949
* **Problem**: When a Dependency was changed from Hard to Soft, it would not release any inputs that were waiting due to the hard dependency.
* **Impact**: Downstream inputs should now be released when updating the Dependency.
* **Risks**: Limited/None

### Search Filter Ignored On Source Dashboard When Paging Through Results

* **JIRA ID**: PROD-843
* **Problem**: Search filter was not working properly when going to pages past the first on the Source Dashboard screen
* **Impact**: Search filter should now be working on all pages on the Source Dashboard
* **Risks**: Limited/None

### Snowflake Output Rename Column Issue

* **JIRA ID**: PROD-902
* **Problem**: Snowflake Output was failing to rename columns that were already in the table
* **Impact**: Snowflake Output will now be able to handle this case
* **Risks**: Limited/None

