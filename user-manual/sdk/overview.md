# Overview

## The Use Case

The DataOps SDK (Standard Development Kit) was created to enable users who needed IDO to integrate with new technologies or services. Prior to the development of the SDK, all IDO integrations for Ingestion, Parsing, and Output required native development by the Intellio team. As a result, a user who wanted to integrate to a new service, say Salesforce, would be required to wait for their request to be prioritized, developed, tested, and released by the Intellio team before they were able to progress in their project. With tight project timelines and endless technologies to integrate, the Intellio team decided instead to develop functionality to let users attach their own code into the IDO processing workflow.

The SDK can help users solve three specific problems, each with a unique custom process type:

### "I need to ingest data from X, but X is not an option in IDO."

Custom Ingestion will allow users to connect to new datasources and pull in raw data. This enables ingestion from locations such as Salesforce, JDBC databases, custom APIs, etc.\


### "I have a file that hsa format Y, but IDO does not have an option for Y"

Custom Parsing will allow users to send a file to IDO and provide instructions for how to parse it. This enables IDO acess data stored in Excel files, Zips, custom formatted flat files, etc.

### &#x20;"I want to write data to location Z, but IDO does not support it."

Custom Post Output will allow users to run any logic after the completion of IDO processing. This enables users to refresh PowerBI reports, update view definitions, push data to a queue, etc.



## How It Works

The mechanism for creating this custom logic is a [Databricks Notebook](https://docs.databricks.com/notebooks/index.html). For all Custom process types, users will write their code into a Databricks Notebook. See the next section "Setting up Databricks to run a Custom Process" for more info on how to set one up.
