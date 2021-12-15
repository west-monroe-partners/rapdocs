# Global System Configuration Parameters

Global system configuration parameters are stored in `meta.system_configuration` table within Postgres. The objective of the table is to store global configuration settings, providing key database, environmental, and application information.

## How To Use

In order to change defaults, change system behavior, or otherwise, users can update rows in this table to set a new "value" entry. After modifying any of these values, the associated system component often needs to be restarted to take effect. The affected components are stored in the array field "application".

## Table Data

| name                             | description                                                                                                                                               | application                | datatype |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------- | -------- |
| cleanup-cron                     | Cron expression for executing cleanup job                                                                                                                 | \["core"]                  | text     |
| database-timeout                 | Timeout of Postgres database                                                                                                                              | \["core"]                  | int      |
| default-timeout                  | A timeout interface (default) for non-specified timeouts                                                                                                  | \["core"]                  | int      |
| file-write-timeout               | Timeout when attempting to write to file                                                                                                                  | \["core"]                  | int      |
| log-character-limit              | Max length of log messages                                                                                                                                | \["core"]                  | int      |
| log-timeout                      | Timeout of Postgres logs                                                                                                                                  | \["core"]                  | int      |
| alert-email                      | Email list for alerts                                                                                                                                     | \["core"]                  | text     |
| ses-user                         | Amazon SES (Simple Email Service) username                                                                                                                | \["core"]                  | text     |
| ses-password                     | Amazon SES (Simple Email Service) password                                                                                                                | \["core"]                  | text     |
| smtp-port                        | Default SMTP port                                                                                                                                         | \["core"]                  | int      |
| smtp-server                      | URL of SMTP server for generating alerts                                                                                                                  | \["core"]                  | text     |
| max-uri-length                   | Maximum URI length for Play API                                                                                                                           | \["api"]                   | text     |
| fixed-connection-pool            | API connection pool size                                                                                                                                  | \["api"]                   | int      |
| max-memory-buffer                | Max size of POST payload for API                                                                                                                          | \["api"]                   | text     |
| databricks-db-name               | Databricks hive database name                                                                                                                             | \["api","sparky"]          | text     |
| environment                      | Name of environment                                                                                                                                       | \["api","sparky"]          | text     |
| emr-mini-sparky-cluster-size     | Cluster size for Mini Sparky in EMR                                                                                                                       | \["api"]                   | int      |
| emr-ec2-instance-type            | EC2 Instance size in EMR                                                                                                                                  | \["api","core"]            | text     |
| emr-release-label                | Version of EMR                                                                                                                                            | \["api","core"]            | text     |
| ec2-spot-price                   | Percentage of on-demand price for spot request                                                                                                            | \["core"]                  | int      |
| emr-certificate-script-path      | Path in S3 to certificate installation script (used for Bootstrap addition)                                                                               | \["core"]                  | text     |
| log-retention-interval           | Time period to keep old log records                                                                                                                       | \["core"]                  | text     |
| data-lake-path                   | Data lake path                                                                                                                                            | \["core","sparky","agent"] | text     |
| spark-provider                   | Databricks or EMR                                                                                                                                         | \[]                        | text     |
| sparky-lifetime                  | Duration Sparky will wait for a new process when there are none in the queue                                                                              | \[]                        | text     |
| bricks-job-history-retention-sec | Amount of time Databricks jobs will be retained in seconds                                                                                                | \["core"]                  | int      |
| cloud                            | Cloud service running DataOps, AWS or Azure                                                                                                               | \["db"]                    | text     |
| spark-node-cost                  | Hourly cost of running single Spark node, including cloud hosting, Databricks, EMR, etc                                                                   | \["db"]                    | numeric  |
| meta-monitor-refresh-interval    | Interval in seconds to refresh postgres operational data snapshots                                                                                        | \["db"]                    | int      |
| spark-config                     | Extra spark configs for Sparky jobs                                                                                                                       | \["core"]                  | json     |
| meta-monitor-refresh-query       | Query generating meta.process operational monitoring dataset. After modifying query, reset meta.system\_status.last\_meta\_monitor\_refresh value to null | \["db","sparky"]           | text     |

