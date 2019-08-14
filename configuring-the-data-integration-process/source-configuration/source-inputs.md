---
description: >-
  The Source Inputs screen shows the status of an individual Source's processing
  and allows users to restart processing of input(s) from a specific processing
  step.
---

# Inputs

An "Input" is RAP’s atomic unit of data processing. Conceptually, an Input corresponds to a single file or scheduled Table pull from a configured Source. 

## Source Inputs Tab <a id="validations-screen"></a>

The Source Inputs screen allows users to monitor the status of their Sources' Inputs. If an Input has failed any steps, users can see the logs and investigate the failure.

![Inputs Tab](../../.gitbook/assets/image%20%2831%29.png)

Selecting any Input's field displays a modal with further details.

### Process Time Modal

Selecting an Input's **Process Time** value displays a modal with sub-times of each of the Input's different phases.

![Process Times Modal](../../.gitbook/assets/image%20%2822%29.png)

### Records Staged / Processed Modal

Selecting an Input's **Records Staged** or **Records Process** displays a modal with record counts at each of the Input's different phases.

![Record Count Modal](../../.gitbook/assets/image%20%2881%29.png)

### Processing Log Modal

To view the Processing Logs for any specific phase of an Input, click the corresponding Input Status icon.

![Select a Processing Log to Display](../../.gitbook/assets/image%20%28151%29.png)

The processing Log contains detailed information useful during troubleshooting. For more information about Processing Logs, see the [Operation Guide](../../operation-guide/).

![Validation Processing Log](../../.gitbook/assets/image%20%281%29.png)

## Statuses and Inputs

The Inputs tab provides insight into the processing of all stages for a given Source. The top of the page has a variety of filters, which allow filtering based on the status of all four processing stages, as well as the file path name.

Each row represents a file for each source and each column value represents the status of a particular processing phase \(Input, Staging, Validation & Enrichment, Output\).

![](../../.gitbook/assets/pending1.png) **Pending**: Phase is waiting on a dependent source to complete processing. 

\*\*\*\*![](../../.gitbook/assets/ready1.png) **Ready**: Landing is prepared for processing phase – if the next phase is configured, it will automatically change to In Progress. 

\*\*\*\*![](../../.gitbook/assets/inprogress1.png)   **In-Progress**: Processing phase currently executing. 

\*\*\*\*![](../../.gitbook/assets/failed1.png) **Failed**: Processing phase failed. Check detail modal to discover reason of failure. 

\*\*\*\*![](../../.gitbook/assets/completed1.png) **Completed**: Processing phase successfully completed. Check detail modal to discover phase metadata.

ADD WARNING SYMBOL HERE

## Controlling All Inputs

Users can control all of the Inputs for a Source using the options below. Not all options will be available depending on the current state of the Source.

* **Reset All Output** - Reset the Output phase for all Sources.
* **Reset All Staging** - Reset the Staging phase for all Sources.
* **Reset All Validation & Enrichment** - Reset the Validation and Enrichment phases for all Sources.
* **Delete All Source Data** - Delete the internally stored data for each Source, forcing RAP to pull the latest data from the Inputs.
* **View Data** - Navigate to the Data View tab.
* **Pull Data Now** - Restart the Input phase for all Sources.

![Options for All Inputs](../../.gitbook/assets/image%20%28105%29.png)

## Controlling One Input

Each Input can be controlled using the options below, which are available depending on Source Type. Options can be selected depending on the current state of the Source.

{% tabs %}
{% tab title="Time Series" %}
* **Reset Staging:** Reset the Staging phase for an Input. 
* **Reset Output:** Reset the Output phase for an Input.
* **Reset Validation & Enrichment:** Resets the Validation & Enrichment phase for an Input.
* **Delete:** Delete the Input from the Source.
* **View Data:** Navigate to the Data View tab - filtering the data for only the selected input.

![Options for Time Series Inputs](../../.gitbook/assets/image%20%2883%29.png)
{% endtab %}

{% tab title="Keyed" %}
* **Reset Output:** Reset the Output phase for an Input.
* **Delete:** Delete the Input from the Source.
* **View Data:** Navigate to the Data View tab - filtering the data for only the selected input.

![Options for Keyed Inputs](../../.gitbook/assets/image%20%28172%29.png)
{% endtab %}
{% endtabs %}

