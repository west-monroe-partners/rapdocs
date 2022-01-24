# Cluster and Process Configuration Overview

### Terms and definitions

| Object                              | Description                                                                                                                                                                                                                                                                            |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p>Cluster </p><p>Configuration</p> | <p>Stores all configuration settings required for the databricks job: cluster configuration, job configuration + few IDO-specific parameters used to control job execution. <br>Cluster configuration record is directly linked to the databricks job via unique job_id attribute.</p> |
| Process Configuration               | Comprised of one default cluster configuration and optional set of cluster configurations for each specific process type. Process configuration is attached to each Source in IDO.                                                                                                     |



Below is high level diagram representing relationship of cluster and process configurations to other IDO metadata tables and Databricks objects

![Diagram 1](<../../../.gitbook/assets/image (380) (1) (1) (1) (1) (1).png>)

