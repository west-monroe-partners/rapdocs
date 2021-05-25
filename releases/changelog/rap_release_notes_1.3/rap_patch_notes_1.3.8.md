# 1.3.8

## Rapid Analytics Platform Patch Notes

## Hotfix 1.3.8

### Hotfixes

| VSTS Bug ID | Description | Impact | Risks |
| :--- | :--- | :--- | :--- |
| 41649 | Output Screen is reaching limit of output sources, receiving 413 error | Users will be able to create more Output Sources | Limited/None - will affect users that have hit an output source limit |
| 41506 | Fix Migration Function handling of output columns | Users currently unable to migrate sources between environments. After fix, migration functions will no longer fail when output column order has changed | High - No effect on core process but cannot be tested in a singluar dev environment, must be deployed to both dev and prod |
| 41648 | Source Promotion Scripts: Doesn't migrate lookup\_order\_by field | Migration scripts will now move all metadata configuraiton fields | High - No effect on core processing but cannot be tested in a singluar dev environment, must be deployed to both dev and prod |

### Legend

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

