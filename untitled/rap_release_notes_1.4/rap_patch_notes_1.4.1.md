# 1.4.1

### Hotfixes

| VSTS Bug ID | Description | Impact | Risks |
| :--- | :--- | :--- | :--- |
| 42022 | Removed Order By attributes from lookup table key to improve performance | This fix reduces the size of lookup tables and removes a reduntant key. Since the Order By key is already calculated on lookup table creation, there is no need for it to be in the lookup table key. | Medium - Change to lookup table creation and population, but will ultimately improve performance |
| 42016 | Removed unnecessary parameters from Time Series and Key Sources | This is a legacy update to sources and outputs to bring their parameters in line with the parameter case class update in 1.4. This update will not affect any source/output parameters that are currently in use. | Low - Updating unused parameters |
| 42008 | Lookup table build broke when using non-standard order by expression | Trying to order the lookup with DESC would cause the lookup table build to fail. | Low - Enables different kinds of order by expression but does not remove existing functionality |
| 41989 | Cleanup actor threw error during lookup table cleanup | This error was being thrown when there was nothing to clean up in the lookup tables. | Limited/None - Error was only thrown when there was nothing to do, so Cleanup actor didn't miss anything |
| 41993 | Lookup on hard dependency was not waiting for lookup table build and reverting to join\_filter method instead | V&E now only waits if a lookup refresh is in progress for the lookup source. | Medium - Affects process workflow and dependency checks |
| 41970 | Join\_filter lookup override type was missing row\_number filter | The row\_number filter is what prevents the lookup from returning duplicates. | Low - Update to V&E query generator |

### Legend

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

