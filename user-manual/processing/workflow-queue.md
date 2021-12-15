# Workflow Queue

![Sample Workflow Queue](<../../.gitbook/assets/image (364).png>)

The Workflow Queue tab provides a view of processes that have upstream dependencies they are waiting on. All processes in this list require another process to finish successfully, or the blocker manually removed by a user to be released into the Processing Queue. It provides information regarding sources such as their queue id, when the workflow process was created, name of the source, input id, type of operation, wait type and wait details.&#x20;

Detailed breakdown of each header:

* **Queue Id**: A globally unique key identifier for the queued item.
* **Time Created**: When the workflow item was initialized.
* **Source**: Name of source.
* **Input Id**: A unique key identifier for input.
*   **Operation Type**: Each of the following operation types represent a different stage of the data processing workflow. To see a full list of all available operation types - please execute the _following_ query.

    ```
    --Query to get all operation type names
    select process_name from meta.process_type;
    ```
*   **Wait Type**: Each wait type represents the item state in the workflow queue. To see a full list of all available wait types - please execute the _following_ query.

    ```
    --Query to get all wait type names
    SELECT wait_type FROM meta.wait_type;

    --Query to get all wait type names & their descriptions
    SELECT wait_type, description from meta.wait_type;
    ```
* **Wait Details**: Provides additional information about item state through unique identifier.
