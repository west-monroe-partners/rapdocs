# Resource Sizing

TODO: Write an intro.  Document default deployment size and how changing Postgres / ETL box sizing affects processing performance.  Also throw in a link to latest AWS pricing for all components

### Scaling Data Processing Resources

Depending on the amount of data being processed and the complexity of the logic in use, scaling up the data processing resources for RAP may be required to meet data loading SLAs.

Data processing resources for RAP are primarily affected by 3 components, all of which should be aligned to maximize use of all components:

* ETL machine size
* Database size \(RDS capacity units\)
* Number of available connections in the connection pool, which is controlled by the maxPoolSize system parameter

In order to optimize processing power with the allocated resources, general guidelines for sizing these 3 components are the following:

* The ETL sizing uses the c5 series of EC2 instances, all of which have \# of virtual CPUs documented.
* 4 ECUs for RDS roughly corresponds to 1 vCPU in EC2 instance sizing.
* The connection pool should contain 1 connection per vCPU + 2 additional connections for utility processes \(so for 4 vCPU configurations, this should be set to 6\).  This will allow for one processing thread to be available to RAP per each available vCPU.

By following these 3 guidelines, the sizing of all 3 components can be aligned to maximize utilization based on data processing needs.  The following table shows some common configurations.  The approximate additional horsepower over the default deployment is also specified.

TODO - add table here

