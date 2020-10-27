---
description: The UX of the Intellio DataOps (RAP) experience.
---

# User Interface

## Overview

![Left hand main menu of the RAP UI.](../.gitbook/assets/rap-ui-menu.png)

The user interface is the front end to Intellio DataOps \(RAP\) that developers will interact with the most.

Users must login to the RAP user interface with a RAP account. Authentication is handled with Auth0, so clients need to be provided with an account in order to login to and use the user interface RAP configurators may sign up with their e-mail address.

Within the RAP user interface, the user is able to set up data sources and outputs, enrich existing data, and even perform troubleshooting. Everything can be reached from the hamburger menu in the top-left corner of the screen.

Sources, Outputs, and Connections can be viewed and filtered from their respective pages. Users will be using these pages the most when configuring data in RAP since Sources, Outputs, and Connections are all required components of RAP's data ingestion and output processes.

Note that clicking on a specific Source on the Sources page will show all of the details and options related to that Source. The same is true for the Outputs and Connections pages.

{% hint style="info" %}
Provided in this section is a brief overview of the Intellio DataOps \(RAP\) UI, if you are looking for a more in depth explanation of the parameters within each section see the [Configuration Guide](../configuring-the-data-integration-process/).
{% endhint %}

## Sources and Processing

The Source Dashboard \(the UI shown when 'Sources' is selected from the left-hand menu\) shows the current status of all Sources as well as the status of the individual phases of the Source processes. The user can also view the status history of Sources and keep track of activity trends. When a Source is clicked on the user is brought to the tab navigation of that Source.

![Tabs within a Source.](../.gitbook/assets/rap-ui-sources-tabs.png)

Sources are the main containers of the Intellio DataOps \(RAP\) interface. There is one logical data flow per source. Connections, Outputs, and Agents are parts of Sources, and these other aspects of RAP enable Sources to work through the Logical Architecture. 

Within a Source the tab structure along the top provides a framework of organizing a monitoring different aspects of the logical data flow. Enumerated below are the general summary of what each tab represents.

### Sources Tabs

| Tab | Summary |
| :--- | :--- |
| Settings | The main interface for set up and configuration of a Source. Set the Connection and Agent.  |
| Dependencies | Sources may be dependent upon other Sources. This tab displays additional data flows the Source may be dependent upon. |
| Relations | Additional data may be joined to a source and this tab displays the details of these joins. |
| Enrichments | Documentation of row level manipulations of the data, inclusive of both enrichments and validation rules. Adding new enrichments allows the user to apply business logic to the data. |
| Inputs | View of the associated raw data that is ingested through the Connection settings. |
| Process | Overview of the logical data flow and the calculations enacted. This view checks the status of the logical data flow. |
| Data View | Provide a view of the first so many rows in the Source Hub Table. |

### Processing

If errors occur during configuration, the RAP user interface has some troubleshooting functionality that allows both clients and configurators to report and handle issues effectively.

The Processing page and Source Dashboard allows users to monitor Sources and Processes at a global level. From the Processing page, users can view and search all current and upcoming RAP processes as well as any process dependencies.

![Process Tab and status of logical data flow ](../.gitbook/assets/rap-ui-process.png)

Within the Process Tab, logs are kept on how data flows through the logical data flow. As seen in the above image, Process IDs 2143-2150 at the bottom of the image report on this particular source going through Ingestion, Capture Data Change \(CDC\), Enrich, Refresh, Attribution Recalculation \(Recalculate\), and Output. When errors occur the Process Tab will held identify at which stage the error occurs.

## Connections and Outputs

Connections and Outputs are the means in which data is moved into and out of Intellio DataOps \(RAP\). Connections can be thought of as folder routing, and their settings determine where data is ingested from or sent out to. Outputs rely on connections to dictate where the data is sent to, but Outputs also do types of data manipulation relevant to business logic.

### Connections

![Connections Settings.](../.gitbook/assets/rap-ui-connections.png)

Connections contain the necessary information to locate data that will be ingested or outputted. Based on connection type and storage type different parameters will appear that need to be populated. These parameters could be just the file path \(inclusive of S3\), credentials to access a database, or additional information to support the RAP Agent.

### Outputs

![The Outputs Mapping Interface.](../.gitbook/assets/rap-ui-outputs.png)

The settings of Outputs are similar to that of Connections but are distinctly different in that the Outputs settings utilize a Connection for the file path and the parameters are associated with end object name and or credentials of the end database.

The additional business logic that may be applied to data during the Output stage occurs in the Mapping tab. Here specific columns can be chosen and additional customization can occur. Even multiple sources can be utilized in the output mapping.

## Other Screens

### Agents

The Agents UI manages parameters related to how an Agent will ingest appropriate data. Additional data on the Intellio DataOps \(RAP\) agent can be found in the [Agent Installation Guide](../deployment/installing-a-new-rap-agent/) and its subsequent [Source Connection Guide](../deployment/installing-a-new-rap-agent/connecting-a-rap-agent-to-a-source.md).

### Documentation

The Documentation link in the main left-hand menu brings you to the document you are currently reading. This Gitbook will be updated as additional information and future releases occur.

### Contact Support

If you require assistance with Intellio DataOps, the Contact Support option exists to assist with troubleshooting and errors. Do note that depending on your relationship and contractural agreements with West Monroe Partners the level of assistance the Intellio DataOps team can provide may be limited.

## Sign Up and Log In

When you first encounter the Intellio DataOps environment you will be prompted to either sign up or to log in. 

![Intellio DataOps \(RAP\) login screen.](../.gitbook/assets/rap-ui-login-signup.png)

In the sign up case you will be prompted to enter a username and a password. The admin of the Intellio DataOps environment should white list emails of the client domain and implementation team. If you email is appropriately whitelisted, then a confirmation email will be sent to the email and next steps will be prompted for the user to login.

In the login case, enter your username and password. Upon logging in you will land on the Sources UI. 

{% hint style="warning" %}
Portions of this page are under development. Additional information may be added in the future pertaining to Cost, Queue times, and additional detail on functionality.
{% endhint %}

