---
description: >-
  The Source Inputs screen shows the status of an individual Source's processing
  and allows users to restart processing of input(s) from a specific processing
  step.
---

# Inputs

An "Input" is Intellio’s atomic unit of data processing. Conceptually, an Input corresponds to a single file or scheduled Table pull from a configured Source.&#x20;

## Source Inputs Tab <a href="#validations-screen" id="validations-screen"></a>

The Source Inputs screen allows users to monitor the status of their Sources' Inputs. The Inputs tab provides insight into the processing of all stages for a given Source. The top of the page has a variety of filters, which allow filtering based on the status of all four processing stages, as well as the file path name. The fields displayed are as follows:

### Columns and statuses

* **Id** - The input ID. Used to indicate this input in logs, processes, and the dependnecy queue
* **File** - The source file name, not present for table sources
* **Received DateTime** - The timestamp at which the input was pulled into Intellio
* **Size** - The total file size of the source file/database pull
* **Record Count** - The number of records appearing in the original input file/database pull
* **Effective Record Count** - The number of records appearing in the hub table after CDC.
*   **Status** - Indicates whether an input has successfully gone through all of its processing steps. Click this status to navigate to the Process page filtered for that input.

    * ![](../../.gitbook/assets/completed.png)  Everything has processed correctly
    * ![](../../.gitbook/assets/failed.png)  A failure has occurred for this input
    * ![](<../../.gitbook/assets/pending (1).png>)  The input is waiting in the dependency queue
    * ![](../../.gitbook/assets/inprogress.png)  The input is currently running a process
    * ![](<../../.gitbook/assets/image (291).png>)The input is launching a new cluster
    * ![](../../.gitbook/assets/delete.png)The input is queued for deletion
    * Grey Check Mark - The input has passed processing but contains 0 records.


* **Checkbox -** Used to select multiple inputs for reprocessing

![The Inputs Page](<../../.gitbook/assets/image (293) (1) (1).png>)

### Three Dot Menu&#x20;

Contains reprocessing options. Kicking off any of these reprocess options will lead to all downstream processes running as well. i.e. Reset Capture Data Changes will perform enrichment, refresh, and output after completing. If one of these options is greyed out, hover over the value to find out why it is not currently a valid choice.

* **Reset Parsing**
  * Only present for file sources
  * Rereads data from the source file
  * Use this when a file is not read into Intellio correctly after adjusting the parsing parameters
* **Reset Change Data Capture**
  * Recalculates all CDC values and rewrites CDC files for a specific input
  * Use this when changing the CDC tracking fields or source refresh type
* **Reset Enrichment**
  * Regenerate enrichment query and run it to rewrite enrichment file for a specific input
  * User this to test out newly created enrichments.
* **Reset Output**
  * Regenerate output query and output delete query for a specific input and run it.
  * Use this to repopulate outputs with newly mapped values
* **Delete Input**
  * Delete this input from Intellio and the hub table.
  * This process type can cause other inputs to process in order to fill in data gaps.
  * Use this to get rid of unwanted data

![Example Menu with Invalid Options](<../../.gitbook/assets/image (289).png>)

## Controlling All Inputs

Users can control all of the Inputs for a Source using the options below. Not all options will be available depending on the current state of the Source.

* **Reset All Output:** Reset the Output phase for all inputs
* **Reset All Parsing:** Reset the Parsing phase for all inputs
* **Reset All Enrichment:** Reset the Enrichment phase for all inputs
* **Reset All Capture Data Changes:** Reset the CDC phase for all inputs
* **Delete All Source Data:** Delete all stored data for the Source
* **View Data:** Navigate to the Data View tab
* **Pull Data Now:** Immediately generate a new Input for this Source (not available on watcher sources)
* **Recalculate**: Perform all net new and changed enrichments on the hub table to bring it up to date.

![Options for all inputs](<../../.gitbook/assets/image (292).png>)

