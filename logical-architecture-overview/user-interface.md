# User Interface

The user interface is the front end to RAP that developers will interact with the most.

Users must login to the RAP user interface with a RAP account. Authentication is handled with Auth0, so clients need to be provided with an account in order to login to and use the user interface RAP configurators may sign up with their e-mail address.

Within the RAP user interface, the user is able to set up data sources and outputs, enrich existing data, and even perform troubleshooting. Everything can be reached from the hamburger menu in the top-left corner of the screen.

![RAP Menu ](../.gitbook/assets/2.0-menu.jpg)

###  Sources, Outputs, and Connections

Sources, Outputs, and Connections can be viewed and filtered from their respective pages. Users will be using these pages the most when configuring data in RAP since Sources, Outputs, and Connections are all required components of RAP's data ingestion and output processes.

![Example - Sources Page](../.gitbook/assets/2.0-sources.jpg)

For example, clicking on a specific Source on the Sources page will show all of the details and options related to that Source. The same is true for the Outputs and Connections pages.

![Example - An overview of a single Source](../.gitbook/assets/2.0-source.jpg)

### Processing and Source Dashboard

If errors occur during configuration, the RAP user interface has some troubleshooting functionality that allows both clients and configurators to report and handle issues effectively.

The Processing page and Source Dashboard allows users to monitor Sources and Processes at a global level. From the Processing page, users can view and search all current and upcoming RAP processes as well as any process dependencies.

![Processing Page](../.gitbook/assets/2.0-processing.jpg)

The Source Dashboard shows the current status of all Sources as well as the status of the individual phases of the Source processes. The user can also view the status history of Sources and keep track of activity trends

\(Source Dashboard Photo\)  

TODO - what can you do in UI?

TODO - login experience / Auth0

