# Metadata monitoring dataset

### Overview

Metadata monitoring dataset exists in databricks meta.process view. It replicates data from postgres meta.process_history table, adding additional attributes from Source and Output tables \(curently source\__name and output\_name\). The dataset provides real-time view of the data in underlying postgres database. The purpose of this dataset is to provide data for customized IDO operations monitoring reports and dashboards and avoiding additional load and contention on postgres metadata database.

Diagram below illustrates how data is flowing from postgres to the meta.process view:

![Data flow and refresh diagram](../../.gitbook/assets/image%20%28293%29.png)

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
      <td style="text-align:left">Interval in seconds to data in meta.process_history data table in databricks.
        Setting this value to 0 will disable metadata refresh process</td>
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
      style="text-align:left">SELECT p.*, s.source_name, o.output_name FROM meta.process_history p LEFT
        JOIN meta.pg_source s ON p.source_id = s.source_id LEFT JOIN meta.pg_output
        o ON o.output_id = p.output_id</td>
    </tr>
  </tbody>
</table>

### External tables

Following external tables are created in databricks catalog to support metadata monitoring dataset:



| Databricks table | Source Postgres Table | Persisted in Databricks |
| :--- | :--- | :--- |
| meta.pg\_input | meta.input \(currently not used, added for future extensibility\) | No |
| meta.process\_history | configured by meta-monitor-refresh-query \(see above\) | Yes |
| meta.process | configured by meta-monitor-refresh-query \(see above\) | Yes |

### Customization

To customize meta.process dataset, modify query stored in meta-monitor-refresh-query parameter and reset meta.system\_status.last\_meta\_monitor_refresh value to null. This will update meta.process view and underlying tables structure and trigger full dataset refresh. You can leverage any postgres metadata tables in the modified query as long as they are joined to core meta.process\_history table and retain its grain_. 

