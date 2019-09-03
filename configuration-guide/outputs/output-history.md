---
description: >-
  Output History shows the status of recent Output processes. Users can diagnose
  and understand the reasons for failure and see Output logs. This includes both
  automatic and manual outputs.
---

# Output History

## Output History Tab <a id="validations-screen"></a>

The Output History tab allows a user to monitor the status of RAP Outputs. The top of the page allows a user to filter Outputs by searching by their name, id, status, or by choosing a date range the Output ran.

![Output History Tab](../../.gitbook/assets/image%20%2878%29.png)

The Output History tab displays the Output logs at the bottom of the screen and the following columns:

* **id:** Unique identifier used on the backend
* **Output Run Name:** Name of the Output. Generated either in the [Output Details ](details.md)or [Manual Output](manual-outputs.md) tabs
* **Output Source Name:** Source name
* **Records Inserted:** Count of records inserted into the target
* **Records Deleted:** Count of records deleted from the target
* **Queued DateTime:** Datetime the output was queued in the process queue
* **Started DateTime:** Datetime the output started operating
* **Duration:** Duration the Output ran
* **User:** User that ran the Output
* **Type:** 
  * **A:** Automatic, as configured in [Output Details](details.md)
  * **M:** Manual, as configured in [Manual Output](manual-outputs.md) 
* **Status:** Icons showing the status of the Output

## Viewing Output Details

Clicking the Status icons opens up the Output History Details modal. This shows advanced logging information about Outputs.

![Status Icon](../../.gitbook/assets/image%20%289%29.png)

The Output History Details modal shows the Severity, Date, and Message for each step of the output. The [Checking Logs]() page gives more context on these messages.

![Output History Details Modal](../../.gitbook/assets/image%20%2813%29.png)



