# 1.4.2

### Hotfixes

| VSTS Bug ID | Description | Impact | Risks |
| :--- | :--- | :--- | :--- |
| 42138 | Record transaction start and end times for Key source on input table | This information is necessary for dependencies to work properly with Key sources. | Low – Impacts dependency processing |
| 42139 | Duplicate parameters in source tab | Original 1.4 deploy script caused duplication of some parameters. This is now removed. | Low - Updating parameters |
| 42140 | Port being saved as String datatype on connections page during initial deploy | Intial deploy will save port numbers as integers. | Low - Fixes a small bug in deployment script |
| 42141 | Single quote value in s\_key breaks output deletes | Users will now be able to include fields with single quotes in their s\_key fields. | Medium - Impacts output delete logic |

### Legend

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

