# On-Premise Agent

RAP leverages an agent architecture in order to ingest data into RAP from various on-premise networks.

### Supported Source Data

The RAP Agent currently supports ingestion of the following source data types out of the box.

* Flat files \(CSV, pipe-delimited, etc\) via file share, local drive or SFTP
* SQL Server
* PostgreSQL
* Snowflake

RAP also supports the following proprietary source systems.  However, since those drivers are under proprietary licenses and may require a licensing fee from the associated vendor, support is not provided directly out of the box.  Instead, the client requiring support for one of the following source systems will need to acquire the appropriate JDBC driver \(and license file if appropriate\) from the appropriate vendor and provide a copy to the RAP development team so the appropriate driver can be built into the RAP Agent.

* Oracle
* Quickbooks
* SAP HANA
* Pervasive SQL

### Data Flow into AWS

The RAP Agent pulls data from the source system and generates a CSV.  That CSV is then uploaded to the landing bucket in S3.

TODO: arch diagram

