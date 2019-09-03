# 1.6

### Enhancements <a id="Enhancements"></a>

| Jira ID | Description | Problem | Impact | Risks |
| :--- | :--- | :--- | :--- | :--- |
| PROD-21 | Import a data source through the UI | Importing a data source for Source Promotion was a manual step that required direct Postgres database access. | Users can now use a button on the UI to import the JSON file that was provided to them from the Export process. Users will be shown a list of changes to the environment before hitting accept on the import. | Medium - Source promotion could affect existing sources |
| PROD-8 | Export a data source through the UI | Exporting a data source for Source Promotion was a manual step that required direct Postgres database access. | Users can now use a button on the UI to choose sources to export. When exported, a JSON file will be downloaded, that can then be used in the Import step. | Medium - Source promotion could affect existing sources |
| PROD-20 | File paths for Input and Output, as well as database\_name parameter added to Connections tab | The Connections tab was added to help keep manual updates out of the Source Promotion process. | The File type is now added as a Connection type. This connection is now used in place of input\_path and output\_path in Staging and Output. Database\_name has been moved to the Database connection type. | Medium - This change requires a migration of existing parameters |
| PROD-12 | Remove template association from changed rules | When updating rules in V&E templates, the user would expect associations to be removed in the UI. | This causes confusion for frontend users and and incorrect view of the state of their templates. | Low - UI enhancement that is purely cosmetic. |
| PROD-13 | Manage sources associated with a template | The user experience around applying templates to sources was tedious and not user friendly. | Users can now apply multiple sources to a template simultaneously through a modal window. | Low - User experience update, functionality remains the same |
| PROD-14 | Stage and parse Fixed Width files | RAP did not support ingesting Fixed Width files. | The user now has an option to choose a File Type of Delimited \(current state for csv parsing\) or Fixed Width. If Fixed Width is chosen, the schema parameter must be filled out to parse the file correctly. | Limited/None - Net new functionality in Staging |
| PROD-15 | Delete unused dependencies during source promotion | When importing a source, unused dependencies weren’t cleared out, causing dependency issues. | Unused \(inactive\) dependencies will always be cleaned up on import. The promotion process will no longer require deactivated dependencies to work properly. | Limited/None - This removes a manual process during the source promotion step for users |
| PROD-16 | Postgres Virtual Output | Users wanted to be able to “virtualize” an Output in RAP, without writing to an outside database. | Virtual outputs are views that are stored in the Postgres RDS, and function similar to normal table outputs. | Limited/None - Net new functionality in Output |
| PROD-18 | Agent logging updated to show logs specific to inputs | When opening the logs for the input step on the UI, all of the Agent logs were contained in the window, making it difficult to comprehend and troubleshoot. | Logs for the input step are now specific to the input that was ran. | Limited/None - Cosmetic update |
| PROD-69 | Button added to Inputs tab to run input pull adhoc | Users wanted the ability to schedule an adhoc pull for a Table or File Pull source. | The Pull Now button has been added to the Inputs tab. When clicked, an input will be queued for the source, and ran when the next Agent heartbeat rolls around. | Limited/None - Button will pull a brand new input, and not affect existing inputs |
| PROD-119 | Better management of backend parameter table | The parameter table was tough for developers to update, and also retain client specific default parameters. | We now capture the defaults from an environment before updating the table, then apply the defaults back on the table. | Limited/None - Parameter defaults could potentially be changed |

### Bugfixes <a id="Bugfixes"></a>

| Jira ID | Description | Problem | Impact | Risks |
| :--- | :--- | :--- | :--- | :--- |
| PROD-149 | Enrichment Rule Templates: Rule type won’t save | Server errors are not shown on screen when a validation rule type fails to be created | Server error messages will now be displayed on screen when a rule type fails to save | Limited/None – Cosmetic |

### Legend <a id="Legend"></a>

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

