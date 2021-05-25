# 1.4.3

### Hotfixes

| VSTS Bug ID | Description | Impact | Risks |
| :--- | :--- | :--- | :--- |
| 42206 | Source dependencies should use extract\_datetime on keyed source | Dependency datetimes will be properly based on when keyed source files are received, not the data they contain | Medium - Modification to wait logic |
| 42063 | Lookup performance enhancements | Tweak to enrichment lookup logic will make queries much more performant | Medium - Small change affecting core processing logic |
| 42205 | V&E not marking failures properly | If a single landing ID fails validation & enrichment, the entire input will be correctly marked as failed | Low - Visual change only |
| 42221 | Quickbooks option for connections | Users will be able to select quickbooks for table source connections | Low - Change to parameters, no underlying logic change |
| 42209 | V&E reprocess inserting too much data to history table | Reprocessing a keyed source with history enabled will not only move records from the specified input to the history table | Low - Only affects history tables |
| 42193 | Not staging gZip files | GZip compression will be functional again | Low - Adding back previously lost capability |

### Legend

| Severity | Description |
| :--- | :--- |
| High | Possible reporting outage, service disruption, or critical processing failure upon failed deployment or bug in the code |
| Medium | Change in methodology to a core processing algorithm, where performance may shift based off of pipeline logic |
| Low | Modification to any core processing logic to fix a known issue, but does not change performance or actual processing algorithm |
| Limited/None | Process tracking fixes, orchestration, or UI only bugs |

