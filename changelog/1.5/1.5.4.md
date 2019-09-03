# 1.5.4

### Bugfixes

| Jira ID | Description/Problem | Impact | Risks |
| :--- | :--- | :--- | :--- |
| PROD-123 | Effective ranges were not being refreshed when validation for multiple landings was run in parallel, causing output to not run | Effective range refresh will occur later in the workflow and execute correctly | Low - Will make effective range refresh more reliable |
| PROD-96 | Outputs to Postgres with too high a batch size caused Orchestrator to crash | Orchestrator will limit itself to batch sizes that do not cause crashes | Low - May have small impact on processing time for existing Postgres outputs |
| PROD-97 | Validation failures were not properly kicking off waiting processes | Validation failures will correctly kick off dependent processes | Low - Only affects process workflow logic |
| PROD-156 | Obfuscated columns were not being calculated correctly | Obfuscated columns with the same input value will now receive the same hash value | Medium - Previously obfuscated columns should be restaged in order to ensure accuracy |

### Legend

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

