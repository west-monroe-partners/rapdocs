# !! Managing System Configuration

Last Update: 6/9/2021 3 PM EST

## Objective

System configuration metadata is available within the `meta.system_configuration` table in `reportingprod`. The objective of the table is to collect global configuration settings, providing key database, environmental and application information.

## Table Schema

| Columns | Purpose | Example\(s\) |
| :--- | :--- | :--- |
| name | Name of the configuration parameter | 'password', 'timeout', 'alert' |
| description | What, where & why of the configuration | 'The cost of running a single node' |
| value | Measurable attribute | '20 days', 'Azure', 'AWS', '.052' |
| application | Which service\(s\) use the parameter | '\["core"\], \["api", "sparky"\]' |
| datatype | Type of data of the value | text, int, numeric, json |

## Table Data

| name | description | value | application | datatype |
| :--- | :--- | :--- | :--- | :--- |
| cleanup-cron | Cron expression for executing cleanup job | 0 0  _\*\*_ \* ? | \["core"\] | text |
| database-timeout | Timeout of Postgres database | 3600 | \["core"\] | int |
| default-timeout | A timeout interface \(default\) for non-specified timeouts | 3600 | \["core"\] | int |
| file-write-timeout | Timeout when attempting to write to file | 3600 | \["core"\] | int |
| log-character-limit | Max length of log messages | 1000 | \["core"\] | int |
| log-timeout | Timeout of Postgres logs | 3600 | \["core"\] | int |
| alert-email | Email list for alerts | update-post-deploy | \["core"\] | text |
| ses-user | Amazon SES \(Simple Email Service\) username | update-post-deploy | \["core"\] | text |
| ses-password | Amazon SES \(Simple Email Service\) password | update-post-deploy | \["core"\] | text |
| smtp-port | Default SMTP port | 587 | \["core"\] | int |
| smtp-server | URL of SMTP server for generating alerts | update-post-deploy | \["core"\] | text |
| max-uri-length | Maximum URI length for Play API | 16k | \["api"\] | text |
| fixed-connection-pool | API connection pool size | 2 | \["api"\] | int |
| max-memory-buffer | Max size of POST payload for API | 4096K | \["api"\] | text |
| databricks-db-name | Databricks hive database name | reportingprod | \["api","sparky"\] | text |
| environment | Name of environment | ReportingProd | \["api","sparky"\] | text |
| emr-mini-sparky-cluster-size | Cluster size for Mini Sparky in EMR | 2 | \["api"\] | int |
| emr-ec2-instance-type | EC2 Instance size in EMR | m5a.xlarge | \["api","core"\] | text |
| emr-release-label | Version of EMR | emr-5.30.1 | \["api","core"\] | text |
| ec2-spot-price | Percentage of on-demand price for spot request | 80 | \["core"\] | int |
| emr-certificate-script-path | Path in S3 to certificate installation script \(used for Bootstrap addition\) | - | \["core"\] | text |
| log-retention-interval | Time period to keep old log records | 30 days | \["core"\] | text |
| data-lake-path | Data lake path | update-post-deploy | \["core","sparky","agent"\] | text |
| spark-provider | Databricks or EMR | Databricks | \[\] | text |
| sparky-lifetime | Duration Sparky will wait for a new process when there are none in the queue | 10 minutes | \[\] | text |
| bricks-job-history-retention-sec | Amount of time Databricks jobs will be retained in seconds | 86400 | \["core"\] | int |
| cloud | Cloud service running DataOps, AWS or Azure | AWS | \["db"\] | text |
| spark-node-cost | Hourly cost of running single Spark node, including cloud hosting, Databricks, EMR, etc | 0.0788 | \["db"\] | numeric |
| meta-monitor-refresh-interval | Interval in seconds to refresh postgres operational data snapshots | 900 | \["db"\] | int |
| spark-config | Extra spark configs for Sparky jobs | { "spark.sql.legacy.avro.datetimeRebaseModeInWrite": "CORRECTED", "spark.sql.legacy.avro.datetimeRebaseModeInRead": "CORRECTED" , "spark.hadoop.fs.s3a.experimental.input.fadvise": "sequential" } | \["core"\] | json |
| meta-monitor-refresh-query | Query generating meta.process operational monitoring dataset. After modifying query, reset meta.system\_status.last\_meta\_monitor\_refresh value to null | SELECT p.\*, s.source\_name, o.output\_name FROM meta.process\_history p LEFT JOIN meta.source s ON p.source\_id = s.source\_id LEFT JOIN meta.output o ON o.output\_id = p.output\_id | \["db","sparky"\] | text |



