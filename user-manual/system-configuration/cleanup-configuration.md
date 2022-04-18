# Cleanup Configuration

Cleanup Configuration defines retention settings for data lake objects and metadata. It is accessible via main menu under System Configuration -> Cleanup Configurations.&#x20;

![](<../../.gitbook/assets/image (386).png>)

Default configuration object is created automatically and is assigned to all existing and new sources.

### Cleanup Parameters

| Parameter                        | Description                                                                                                                                                                          |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Hub delete type                  | <p>"Keep latest version only" setting will remove all non-current files and folders with hub table data from data lake <br>"Keep all versions" disables hub data objects cleanup</p> |
| Inputs No Effective Range Period | Retention period for batches (inputs) of data with no effective range (data). Applies to sources with key, timestamp and sequence refresh types                                      |
| Full Refresh Inputs Period       | Retention period for not current/latest batches (inputs) of data. Applies to sources with full refresh type                                                                          |
| Zero Record Inputs Period        | Retention period for inputs (batches) containing zero records. Applies to all source refresh types                                                                                   |

Cleanup process will delete all inputs from metadata store according to these settings. It will then remove any orphaned objects from the data lake, including deleted inputs and sources.

### Configuring Cleanup for the source

When new source is created, it is automatically assigned default cleanup configuration. To change it, open source settings and select desired cleanup configuration:

&#x20;

![](<../../.gitbook/assets/image (390).png>)
