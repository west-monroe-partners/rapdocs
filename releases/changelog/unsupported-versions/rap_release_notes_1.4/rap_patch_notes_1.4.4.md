# 1.4.4

### Hotfixes

| VSTS Bug ID | Description | Impact | Risks |
| :--- | :--- | :--- | :--- |
| 41313 | Work tables were not being cleaned up properly in the Cleanup process, leading to long and poorly performing Cleanup runs. | This update will make the Cleanup process less demanding on the platform. | Low - Only work tables that need to be cleaned up will be affected |

### Legend

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

