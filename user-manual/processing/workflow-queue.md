# !! Workflow Queue

![Sample Workflow Queue](../../.gitbook/assets/image%20%28364%29.png)

The Workflow Queue tab provides a simple view of incoming processes. It provides information regarding sources such as their queue id, when the workflow process was created, name of the source, input id, type of operation, wait type and wait details. 

Detailed breakdown of each header:

* **Queue Id**: A globally unique key identifier for the queued item.
* **Time Created**: When the workflow item was initialized.
* **Source**: Name of source.
* **Input Id**: A unique key identifier for input.
* **Operation Type**: Each of the following operation types represent a different stage of the data processing workflow. **◦ Attribute Recalculation** _**◦**_ **Capture Data Changes** - Comparison between data state to reflect latest changes. **◦ Cleanup** - Teardown after processing. **◦ Custom Post Output** **◦ Data Profile** - View of data. **◦ Enrichment** - Applies conditions to ingested data. **◦ Hub Table Check** - Verifies schema displayed by hub table. **◦ Import** - ****Pulls in item specified by connection type.  **◦ Ingestion** - Gathering item specified by connection type. **◦ Input Delete** - Deletion of input. **◦ Manual Attribute Recalculation** **◦ Manual Output** **◦ Manual Reset All Capture Data Changes** **◦ Manual Reset All Enrichment** **◦ Manual Reset All Output** **◦ Manual Reset Capture Data Changes** **◦ Manual Reset Enrichment** **◦ Manual Reset Output** **◦ Manual Reset Parse** **◦ Manual Reset Sparky Parse** **◦ Meta Monitor Refresh** **◦ Output** - Finalized view of data. **◦ Output Send Generation** **◦ Parse**: Iteration through ingested item. **◦ Refresh**: Check to determine if there were data changes. **◦ Reprocess Check** **◦ Rollback**: Revert to previous item state. **◦ Sparky Ingestion**: Sparky ingests item based on connection type. **◦ Sparky Parse**: Sparky iteration through item.
* **Wait Type \*\***

  **◦ Upstream Lookup Refresh**  
  **◦ Source In Refresh Or Recalc**

  **◦ Refresh Waiting**  
  **◦ Source Refresh**

  **◦ Manual Reset All Output**  
  **◦ Manual Reset Capture Data Changes**

 ****

* **Wait Details**

