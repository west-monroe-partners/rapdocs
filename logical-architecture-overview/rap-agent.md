# RAP Agent

## Overview

The purpose of the RAP Agent is to acquire files from local file storage or an Amazon S3 bucket / Azure Storage account \(depending on the platform where RAP is deployed\) and ingest them into the RAP application. The Agent is installed on local Windows or Linux machines at client sites for the security of the client. Since the Agent handles all file ingestion operations at the client site, West Monroe does not need to access the client's machine directly.

The RAP Agent was created to solve the common problems related to data ingestion and data connection from source data locations.

## Process

&lt;Diagram&gt;

The RAP Agent is a small piece of software that requires read only access to the source data and a 443 outbound internet connection. The setup allows for the data upload to bypass client intake firewalls and establishes a secure, encrypted connection between the uploaded source and the upload location.

The RAP Agent works as a light weight service. On the client side the RAP Agent turns on when the service is active and shuts down when not active. The service runs the appropriate queries against the source database 



 The agent reads the necessary data, 

The heartbeat and logs

At the beginning of a project an installation guide will be provided as well as documentation as how to download, install, and set up the specifics of the RAP Agent.



TODO - add a diagram

TODO - step-by-step on what Agent does, from pulling request from RAP database, thorough performing data acquisition, all the way through to landing data in Data Lake

