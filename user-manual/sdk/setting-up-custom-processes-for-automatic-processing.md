# Setting Up Custom Processes for Automatic Processing

Custom Ingest, Parse, and Post Output can all be configured to run on the user's Custom created notebooks by using the Cluster Configuration page. In the Hello World examples, we simply clicked all of the default buttons and saved the configuration. In this example, we will create a new Cluster Config pointing at our Custom Code and update a Custom Ingest Source to use it.

### Finding the Notebook Name

Before we create any Cluster Configurations, it is important to first find the name of the Notebook that is running the Custom code. In the Databricks UI, open the Notebook with the Custom code and hover over the Notebook name in the top left corner. A display will show the full name of the Notebook. In the case of this notebook, it is "Workspace/Shared/CustomIngest". Record this value, removing the "Workspace" portion. In this case we want "/Shared/CustomIngest", and don't forget the leading slash!

![Displaying the Notebook Name](<../../.gitbook/assets/image (382).png>)

### Creating the Cluster Configuration

Navigate to the Cluster Configuration page in IDO and Click the New Cluster button. A Cluster Settings Page will open. Locate the Job Task Type button and click "Custom Notebook". This will cause a Notebook Path control to appear. The user should put the value recorded in the step above into the Notebook Path control. See the image below for an example.

![The Cluster Configuration points toward the Custom Notebook](<../../.gitbook/assets/image (396).png>)

### Applying the Cluster Configuration to a Custom Ingestion Source

Now navigate to the Source Settings page for the Custom Ingest Source associated with the Notebook. Locate the "Custom Ingest Cluster Configuration" dropdown and select your newly created Cluster Configuration. Hit save. The Source is now configured to use the specified Notebook when runnning ingestion processes! Try clicking the Pull Now button or setting a schedule on the Source to pull in data.

![The Custom Ingest Cluster Config applied to a Source](<../../.gitbook/assets/image (378).png>)

That's it. The Source is now fully confgured to use the Custom Notebook when pulling in data. The process is similar for Custom Parsing, with the user editing the "Custom Parsing Cluster Configuration" control on the Source Settings page. For Custom Post Output, the user will edit the "Custom Post Output Cluster Config" control on the Output Settings page.
