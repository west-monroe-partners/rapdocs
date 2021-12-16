# Quality of Life Features in 2.4.0

## Intellio DataOps 2.4.0 Release&#x20;

The Intellio 2.4.0 release contains a number of major quality of life features as well as exciting new functionality. We are excited to share with you a brief overview of these upcoming features!&#x20;

* Centralized schedule management&#x20;
* Post-Output SDK + Custom Loopback&#x20;
* Meta Monitor Refresh process&#x20;
* Auto-managed views for each hub table&#x20;

### Centralized Schedule Management&#x20;

In the past, schedules were created for each source individually. As a result, doing large scale updates to schedules was a tedious and error prone process that involved editing multiple sources or performing risky backend updates. In 2.4.0, we have centralized all schedule management into a single Schedule object that can be applied across sources. &#x20;

![The new schedule listing page](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-LhufZT729fit8K2vT1H-3930866850%2Fuploads%2FkHAZSQAxzvHC2U5OYfJ5%2Ffile.png?alt=media)



![The new schedule settings page ](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-LhufZT729fit8K2vT1H-3930866850%2Fuploads%2F2vZ9vSFDjFhEto2ZvvUr%2Ffile.png?alt=media)

As part of the 2.4.0 deploy, all scheduled sources will automatically be converted to use the new schedule objects. &#x20;

### **Post-Output SDK + Custom Loopback**&#x20;

We have often received requests to run a code snippet after the output process completes. With the new extensions to our DataOps SDK, custom post output, users will now be able to specify a Databricks notebook to run commands after out has completed. This opens up a large new set of capabilities. From triggering a custom ingestion to creating a summary file on the data anything that can be written in a Databricks notebook can be executed. Find full documentation [here](https://intellio.gitbook.io/dataops/v/master/configuring-the-data-integration-process/custom-post-output).&#x20;

### Meta Monitor Refresh process&#x20;

DataOps tracks every process as it moves through the platform in the Postgres database. With the meta monitor refresh process, we are giving users the ability to access these tables for querying, reporting, and analytics. When IDO has downtime, it will automatically kick off a process to export Postgres data into Databricks. Data is stored in its own database and can be accessed as any other Hive table would be.&#x20;

![The new DataBricks Meta Database](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-LhufZT729fit8K2vT1H-3930866850%2Fuploads%2Fgoq3nx3Hg9tScSya3svy%2Ffile.png?alt=media)

### Auto-managed views for each hub table&#x20;

While Prod.hub\_123 might be a fine name for backend processing, it is not great for human readability or data exploration. In release 2.4.0, we have added the ability to specify a Hub View Name for each source. Based on this name, a more human readable view will be created in the Databricks Hive metastore. With this, users will no longer need to reference back to the DataOps UI in order to do data exploration in DataBricks.&#x20;

![The new Hub View Settings Field ](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-LhufZT729fit8K2vT1H-3930866850%2Fuploads%2FaCK0uGfW0HK5glRvx9s8%2Ffile.png?alt=media)

