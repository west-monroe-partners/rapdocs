# 1.5.3

### Hotfixes

| Description | Problem | Impact | Risks |
| :--- | :--- | :--- | :--- |
| Wait/Dependency logic regarding the lookup refresh process caused extra processes to be enqueued | If a Key source that a Time Series source looks up on finishes processing, it calls a Lookup Refresh process. If that Time Series source had Outputs waiting, the Lookup Refresh would mistakenly think it needed to release Validations in the Time Series source. | A processing edge case, but would cause extra Validation runs to be processed, which slowed down morning loads and source processing. | Low - This is a processing logic change to fix an edge case |
| Extract Datetime and Received Datetime system fields added to Output mappings | Users wanted the ability to map these two fields to provide more information about the input in their Output destinations | Extract datetime is the last modified time of the file if it is input through file pull or file push. If you use a table pull to get the input data, then it is the time the source query was ran. Received datetime is the time the file is brought into the platform. | Low - These are columns that already exist on the input table, we're just exposing them to the user now |
| Key History now available to Output | Previously, there was no way for a user to Output the data that is stored in the Key History tables. Key History data is useful when creating Type 2 dimensions with the platform. | This data is stored when the user enables "cdc\_store\_history" in the Source details parameters. The Key History table for a source tracks updates to the keys in the source. To use this feature, enable the 'key\_history' parameter on the Output | Low - This data is currently stored in the platform, just an output level decision needs to made to push the key or history data |

### Legend

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

