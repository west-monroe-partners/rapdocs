# 1.5.2

### Hotfixes

| VSTS Bug ID | Description | Problem | Impact | Risks |
| :--- | :--- | :--- | :--- | :--- |
| 43482 | Output Source delete type not overriding Output delete type for table outputs | The output query generator in Release 1.5 was not taking the Output Source delete type into consideration when choosing the delete type. This was causing Output to fail in certain scenarios. | Key Output Sources that feed into a single Output will fail if the delete type on the Output is Range. After this fix, the Key Output Sources need to be set to "Key" delete type in the Output Source parameter screen. | Low - This is a change to the output query generator to pick the correct parameter on Output run |

### Legend

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

