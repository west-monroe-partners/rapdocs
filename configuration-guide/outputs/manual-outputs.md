---
description: >-
  Manual Output creates a one-time Output with custom parameters. This is
  typically used for historical loads or re-loads after configuration changes.
---

# Manual Output

## Manual Output Tab

The Manual Output tab allows users to create a custom-defined output. There are filtering options available for users to define the output.

![Manual Output Tab](../../.gitbook/assets/image%20%28138%29.png)

The Manual Output tab displays the sources at the bottom of the screen and the following columns:

* **Output Source Name:** The Source Name.
* **Source Type:** The Source Type \(Timestamp or Key\)
* **Delete Type:** Allows a user to delete records from the destination of the Output.
  * **All:** All records in the destination are deleted and reloaded
  * **Range:** Records within the Range selected are deleted and reloaded. Only applies to Timestamp data.
* **Range Start:** The beginning of the Range. This filters the Source from the beginning of the Range, inclusively. Only applies to Timestamp data.
  * **Timestamp:** datetime value that the user can filter the **date\_column**.
  * **Range:** integer value that filters the **range\_column**.
* **Range End:** The end of the Range. This filters the Source to the end of the Range, inclusively. Only applies to Timestamp data.
  * **Timestamp:** datetime value that the user can filter the **date\_column**.
  * **Range:** integer value that filters the **range\_column**.
* **Status:** Icons showing the status of the Source after validation is ran.

{% hint style="info" %}
Ranges are defaulted to their min/max values for easy mass reload of all history
{% endhint %}

## Choosing Sources

The first step for creating a Manual Output is choosing Sources. Select the **Add Source** button to open the Add Source modal.

![Add Source Button](../../.gitbook/assets/image%20%28122%29.png)

The Add Source modal only displays the list of Sources that were selected in the [Output Mapping ](output-mapping.md)tab. The user can select a list of Sources and click **Apply** to add them to the Manual Output.

![Add Source Modal](../../.gitbook/assets/image%20%2885%29.png)

## Removing Sources

Clicking the Trash Icon from the Manual Output page removes the Source from the Manual Output.

![Trash Icon](../../.gitbook/assets/image%20%28105%29.png)

## Validating the Manual Output

Manual Outputs must be Validated before running. Because mass re-outputs have large impacts on the Data Warehouse layer, and represent one of the more risky production impact operations, RAP has built in  pre-check processes prior to a full execution.

Click the Validate button at the bottom of the screen to validate each source and get the status of each Source.

![Validate Button](../../.gitbook/assets/image%20%2897%29.png)

Once the Validate button is pressed, the Sources are all validated. The Status will update to reflect the validation of each Source.

![Status After Validation](../../.gitbook/assets/image%20%2899%29.png)

There are three possible Status values. 

\*\*\*\*![](../../.gitbook/assets/inprogress.png) **In-Progress**: Processing phase currently executing

\*\*\*\*![](../../.gitbook/assets/failed.png) **Failed**: Processing phase failed

\*\*\*\*![](../../.gitbook/assets/completed.png) **Completed**: Processing phase successfully completed

## Running the Manual Output

Click the Submit button to run the Manual Output. All Completed Sources will run, while Failed Sources are ignored.

![Submit Button](../../.gitbook/assets/image%20%2889%29.png)

