# Metadata monitoring dataset

### Overview

Metadata monitoring dataset exists in databricks meta.process table. It replicates data from postgres meta.process\_history table, adding additional Source and Output table columns. The dataset is refreshed automatically, with configurable cadence \(default is 15 min\). The purpose of this dataset is to provide data for customized IDO operations monitoring reports and dashboards and avoiding additional load and contention on postgres metadata database.

### Configuration

<table>
  <thead>
    <tr>
      <th style="text-align:left">Location</th>
      <th style="text-align:left">Parameter</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>meta.</p>
        <p>system_configuration</p>
      </td>
      <td style="text-align:left">meta-monitor-refresh-interval</td>
      <td style="text-align:left">Interval in seconds to refresh postgres metadata monitoring data tables
        in databricks</td>
      <td style="text-align:left">900</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>meta.</p>
        <p>system_configuration</p>
      </td>
      <td style="text-align:left">meta-monitor-refresh-query</td>
      <td style="text-align:left">Query generating meta.process operational monitoring dataset.</td>
      <td
      style="text-align:left">SELECT p.*, from_json(record_counts, &apos;old int, warned int, passed
        int, total int, hub_rows int, failed int, changed int, latest_timestamp_update
        int, unchanged int, new int&apos;) as record_counts_struct, s.source_name,
        o.output_name FROM meta.process_history p LEFT JOIN meta.pg_source s ON
        p.source_id = s.source_id LEFT JOIN meta.pg_output o ON o.output_id = p.output_id</td>
    </tr>
  </tbody>
</table>

### External tables

Following external tables are created in databricks catalog to support metadata monitoring dataset:



| Databricks table | Source Postgres Table | Persisted in Databricks |
| :--- | :--- | :--- |
| meta.pg\_source | meta.source | No |
| meta.pg\_output | meta.output | No |
| meta.pg\_input | meta.input | No |
| meta.pg\__process\__history | meta.process\_history | No |
| meta.process\_history | meta.process\_history | Yes |
| meta.process | see meta-monitor-refresh-query above | Yes |

### Customization

To customize meta.process dataset, modify query stored in meta-monitor-refresh-query parameter and reset meta.system\_status.last\_meta\_monitor\_refresh value to null. This will update meta.process table structure and trigger full dataset refresh. You can leverage any tables listed above in the modified query. 

