---
description: A high-level description of all the infrastructure components powering RAP.
---

# !! Infrastructure Components

## The Components

![Components and their interactions.](../../.gitbook/assets/rap-components.png)

RAP leverages various cloud components in AWS or Azure \(depending on the platform selected\). Regardless of the platform, the actions of the components remain the same. This section provides an overview of these different components and how these components interact with one another. Subsequent sections explore the details as they pertain to what specific services are leveraged in the AWS or Azure platform.

{% hint style="info" %}
For Authentication, RAP utilizes Auth0
{% endhint %}

### UI \(User Interface\)

The UI represents what you see when you log into the RAP platform. The UI is the front end view of RAP. The [UI](../../logical-architecture-overview/user-interface.md#overview) is characteristic of the left-hand menu and consists of screens including Sources, Connections, Agents, and Outputs. The UI is agnostic to the platform \(AWS or Azure\) as it is interacted with through the browser.

### API

RAP's API is a lightweight communicator between all of the components of the infrastructure. The API strictly communicates between the components and does not execute any business logic.

### Meta Storage

Meta Storage is the component that executes the vast majority of business logic and transformation. Meta Storage encompasses the databases and functions \(in Postgres SQL\) to execute the required business logic. 

### Core

Core is the component of orchestration. Core manages executions of beginnings, hand offs, restarts, queues, and makes sure whatever must occur next does in fact occur. Core works closely with the Meta Storage to execute the appropriate business logic in the appropriate order. Core also works with Sparky Job to reference when to start up and when to shut down Spark infrastructure.

### Agent

The [RAP Agent](../../logical-architecture-overview/rap-agent.md#overview) works to move data from the client infrastructure into the RAP Cloud infrastructure. The RAP Agent is only utilized during the Ingest stage of the Logical Data Flow.

### Ad Hoc Cluster

The Ad Hoc Cluster enables the UI. The primary purpose of the Ad Hoc Cluster is to execute the UI and ensure nothing breaks in the configuration of RAP. The most common usage of the Ad Hoc Cluster is the Data Viewer, such as viewing the first 200 rows of data.

To illustrate how the Ad Hoc Cluster works, when you click "save" on a rule, RAP will generate the query within the meta storage layer, process these rules with the source data, and return the results back through the API. The Ad Hoc Cluster will test this API result \(with a zero records call\), and will return back to the UI. 

### Sparky Job

When a job is ready for scale, the Sparky Job is enlisted to execute. Business logic that is validated in the meta storage layer and Ad Hoc Cluster is then executed, when dictated to by the Core, at scale. Sparky Job on its own is simple spark code. Sparky Job relies on the Core to indicate when to begin, run, or shut down based on appropriate orchestration.

### Data Storage

The data storage is the location of where the data is stored, and is dependent on the services used: AWS or Azure.

## Infrastructure Application

RAP 2.0 currently exists for AWS and Microsoft Azure systems. Depending on which the specific services are listed below at a high level with additional detail elaborated on in the Amazon Web Services and Microsoft Azure sections. !! Add links to the AWS and Microsoft sections.

### Amazon Web Services Implementation

![Amazon Web Services Infrastructure](../../.gitbook/assets/rap-components-aws.png)

### Microsoft Azure Services Implementation

![Microsoft Azure Infrastructure](../../.gitbook/assets/rap-architecture-agent.png)

### Services Summary

| Component | AWS | Azure |
| :--- | :--- | :--- |
| Data Lake |  |  |
|  |  |  |
|  |  |  |
|  |  |  |

{% hint style="info" %}
!! Resource Costs. Add here some links to pricing for the different resources.
{% endhint %}





!! Add image of the physical architecture, what software used, etc.

RAP leverages various cloud components in AWS or Azure \(depending on the platform selected\).  This section provides an overview of the components leveraged by RAP, how they are leverages as part of the RAP processing engine, and how sizing affects processing performance.

This section covers the following components of the RAP infrastructure stack, as well as how sizing affects performance when applicable:

* On-Premise Agent
* Data Lake Storage
* Virtual Machines
* Database Layer

