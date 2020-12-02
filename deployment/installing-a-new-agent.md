---
description: >-
  Details requirements and configurations to installing an Agent on a server
  that can access an on-premise data source.
---

# Installing a New Agent

DataOps Agents are a means of transporting data from a source data source to a cloud storage location DataOps can manage \(either in AWS or Microsoft Azure\). Agents are managed through the DataOps interface once installed. This section walks through the process of installing a new DataOps Agent on a Windows server. Currently, the Agent can only be installed on Windows.

## Prerequisites \(Requirements for Installation\)

Prior to installing the DataOps Agent, the following requirements must be met.  Please ensure that the machine hosting the DataOps Agent meets the requirements prior to installation.

* The machine or VM must be running a Windows operating system:
  * Windows 7 / Server 2008 R2 or later
* The latest version of the Amazon Corretto JDK 8 should preferably be installed on the destination machine. Oracle JDK 8 is also sufficient. If neither is installed on the destination machine, navigate to the site linked [here](https://docs.aws.amazon.com/corretto/latest/corretto-8-ug/downloads-list.html) and download and install the Corretto JDK that is compatible with the target architecture and OS of the machine that the Agent will be installed on.
* Since the Agent initiates all its own connections, outbound connections to various cloud resources on the public Internet are required.  If a firewall is limiting outbound internet access, the following resources should be allowed through the firewall \(exact domain names will vary by environment\).  Note that these are all secure endpoints, so if SSL inspection is enabled at the firewall, special consideration is needed to ensure that the certificate presented to the machine hosting the Agent is trusted.
  * AWS S3 / Azure Data Lake Storage via HTTPS \(port 443\)
  * DataOps API endpoint via HTTPS \(port 443\)
  * Auth0 via HTTPS \(port 443\)
* File and database sources that will be accessed through the DataOps Agent must be accessible from the machine the Agent is being installed on.  In the case of database sources, many customers install the Agent software directly on the database server - network connectivity from the machine that the Agent software is being installed on is all that is required.  Note that the Agent will be performing data pulls and uploading to AWS / Azure, so the recommendation is to not segment off the Agent machine from the sources being accessed in a way that traffic needs to cross a limited capacity network segment to reach those sources.
* The user account intended for the Agent to use to access source databases needs to be set up with native database engine authentication.  In the case of SQL Server, Windows / Azure AD authentication is not supported for the Agent user and only SQL Server authentication can be used.
  * Mixed-mode authentication can still be enabled to allow for integrated authentication for non-DataOps related loads.

## Setting up a New Agent Configuration

![New Agent Creation.](../.gitbook/assets/rap-agent-select-new.png)

In the DataOps UI, from the left hand menu navigate to Agents, and click New. From this settings interface input the required inputs as seen below. Additional parameters exist for more detailed installations. Note that you will need information about the machine the agent will be hosted on.

| Agent Inputs | Detail |
| :--- | :--- |
| Name | Agent code on the backend, and name reference for sources and other DataOps elements within the UI. |
| Description | Necessary clarification and detail. |
| Region | AWS region, or Microsoft Azure region that the DataOps is stored and being processed in \(e.g. "us-west-2", "East US 1"\). |
| Machine Guid | A specific key to identify the machine the agent is running on, see below for the terminal command to obtain. |
| s3LandingPath | Path to Datalake bucket \(e.g. "s3://dev-datalake-intellio"\) |

#### Machine Guid Command

To get the Machine Guid, run the following command in the terminal on the server that the Agent will be installed on.

```text
reg query HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Cryptography /v MachineGuid
```

## Installing the DataOps Agent \(Windows\)

Navigate to the AWS S3 console for the specific environment on which DataOps will be operating. Navigate to the bucket: &lt;environment&gt;-agentjar-&lt;client&gt;. Within this bucket there is a path called "msi-install". Install the appropriate version based on the version of DataOps that is live in the environment.

Identify the appropriate version of the .msi \(installer\) by hovering over the navigation menu in the upper left hand corner of the DataOps UI.

![Location of DataOps version to identify installation version.](../.gitbook/assets/rap-agent-version-finding.png)

Download the appropriate version of the MSI installer. Run the installer and fill in the appropriate prompts.

![Selection of DataOps Agent installation screens.](../.gitbook/assets/rap-agent-installer-screens.png)

A specific **RAP Agent Code** will be required, and this is the **Agent Name** that was configured in the DataOps UI. Do not alter the configuration key unless directed to by a DataOps team member. 

The final step of the installation pertains to providing the appropriate credential configuration to the application. Navigate to the DataOps UI and Agents screen and click on the cloud icon under the Config column \(and for the appropriate Agent row\). Clicking on this icon will download the configuration document.

![Config location](../.gitbook/assets/rap-agent-configuration.png)

Locate the file. The downloaded file should be named "agent-config.bin". Move this file to the bin folder within the Agent: C:/Program Files \(x86\)/Rapid Analytics Platform/bin

![How the bin folder should look on client machine.](../.gitbook/assets/rap-agent-how-the-bin-files-should-look.png)

Once the file has been placed, navigate to the "Services" explorer on the server and restart the service named "RAPAgentBat".

Once the service is restarted, navigate to the DataOps UI and check the Agent logs and status to make sure it is online and heartbeating.

This should complete the setup of the Agent on the client machine. All further configuration should be able to be completed via the DataOps UI, such as locating file paths or database/data warehouse credentials.

## Installing the DataOps Agent \(Linux\)

{% hint style="info" %}
Historically, DataOps also provided a native Agent for Linux \(Centos/Redhat\), however, due to lack of demand the Linux DataOps Agent has been deprecated. Historical Linux RAP Installation can be found [here](https://westmonroepartners1.sharepoint.com/sites/DDPA/0063900000stpZHAAY/Docs/Forms/AllItems.aspx?FolderCTID=0x0120001A877AC2A8D0754C894745F7F2227E37&id=%2Fsites%2FDDPA%2F0063900000stpZHAAY%2FDocs%2FImplementation%2FTechnical%20Documentation%2F3%20-%20RAP%2FRAP%20Agent%20Installation%2FRAP%20Agent%20Install%20Guide%20for%20Red%20Hat%206%2E10%2Epdf&parent=%2Fsites%2FDDPA%2F0063900000stpZHAAY%2FDocs%2FImplementation%2FTechnical%20Documentation%2F3%20-%20RAP%2FRAP%20Agent%20Installation).

Linux agents are now supported via Docker containers hosted by AWS ECS or Azure Container Instances
{% endhint %}

