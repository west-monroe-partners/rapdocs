# 1.3.7

### Bugfixes

| VSTS Bug ID | Description | Impact | Risks |
| :--- | :--- | :--- | :--- |
| 41272 | Output status code now correctly shows success code when output completes | Users will now see an accurate Output processing status in the UI | Low - will only affect visual status codes in the platform |
| 40150 | Resolved deadlocking issues when running range deletes that overlap for the same output source | The system will now recover properly during the Output phase in the event of a deadlock | Limited – Changes involve error handling and recovery. No changes made to core processing |
| 41273 | Validation and Enrichment phase now shows the Wait status code while waiting on a dependency | Users will now see an accurate processing status for the Validation and Enrichment phase | Low - will only affect visual status codes in the platform |
| 41205 | Validation and Enrichment now starts correctly after Staging completes | Daily loads will no longer be hampered by processes not handing off properly - less time spent monitoring | Low - affects enqueuing of processes but no changes made to core processing |
| 41274 | Data profiling is now kicked off correctly for Time Series sources | Data profiling is not used actively in Production but will help Dev users | Low - Data profiling process is sparsely used and is a low priority process |
| 39479 | Source dashboard no longer limited to 5 sources | Users can now view all their sources in source dashboard page | Low - Source dashboard is a UI component and is currently not monitored in Production |
| 41253 | Concurrently running Validations no longer cause system lockout due to Postgres lock waits | Platform stability improved | Low - affects how processes are grabbed from process queue |

### Legend

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

