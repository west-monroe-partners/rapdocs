# !! Example Daily Routine

In this section, we will detail a suggested routine for maintaining the application on a daily basis.

## Alerts

The primary mechanism to stay informed on the activities of the platform is RAP Alerts. Alerts are configurable to inform users when any of their Sources succeeds or fails a processing phase. If the daily data load is set to run overnight, the first activity in the morning is to look over the alerts and address any failures. Each alert message contains the following details to assist the user in troubleshooting:

* Input Id
* Log Id
* Source Name
* Link to processing page
* Error Message

To add alerts, navigate to the Source Detail page, Show Advanced Parameters, and enter an email to the distribution list.

![Add an Email to the Distribution List](../../.gitbook/assets/image%20%28192%29.png)

## Summary View of Nightly Load

After addressing all alerts, users should check the general status of the previous night’s data loads. Do this by utilizing the Operational Reporting dashboard or the following query:

```sql
SELECT  i.input_id, i.source_id, s.source_name, s.staging_table_name, i.input_status_code, 
        i.staging_status_code, i.validation_status_code, i.output_status_code, *
FROM stage.input i
JOIN stage.source s ON i.source_id = s.source_id
WHERE received_datetime::date = now()::date
AND (input_status_code IS DISTINCT FROM 'P' 
    OR staging_status_code IS DISTINCT FROM 'P' 
    OR validation_status_code IS DISTINCT FROM 'P')
ORDER BY s.source_name
```

This provides the opportunity to identify any issues with the application and act upon them quickly. Example error use cases include:

* Error: Not all sources pulling from on Agent succeeded scheduled inputs.  Resolution Tactic: Finding the error message and determining the cause of the issue \(network partition, credential issues, etc.\).
* Error: A Keyed source succeeds Input and Staging, but is waiting to run Validation & Enrichment. Resolution: After tracing Validation and Enrichment dependencies for a source, it is revealed that prior input has failed V&E, causing current input to wait before the preceding V&E rule completes processing.

{% hint style="danger" %}
If an issue like this goes unresolved, the system will continue to follow the scheduling rules which will result in Keyed source data awaiting to process for an extended period of time. Pay close attention to Keyed sources every day.
{% endhint %}

{% hint style="info" %}
For information about frequently encountered errors during monitoring as well as steps to fix them, see the [RAP Agent](rap-agent.md) page.
{% endhint %}

## Logs

Check into the [Actor and Orchestrator Logs](checking-logs.md) in the morning to see if anything unusual happened. Digging into the Sources, as described in the last two steps, should expose an issue if one occurred, but it is a good practice to check both the Actor and Orchestrator Logs on a daily basis to identify any other issues.

## Instance Health

As part of the daily routine, it is a good habit to confirm that the relevant AWS instances are active and healthy. The [EC2 ](example-daily-routine.md)instances \(ETL & API boxes\) should have an Instance State of `running` and have passed their Status Checks.

![Healthy EC2 Instances](../../.gitbook/assets/13.png)

The [RDS Postgres](example-daily-routine.md) instance should have a Status of `available` and should have CPU levels and Current Activity below the red line. If these levels are above the normal amount, check the logs and RDS Performance Insights to see what the cause may be.

![Healthy RDS Postgres Instance](../../.gitbook/assets/14%20%281%29.png)

{% hint style="info" %}
See [Maintaining the Infrastructure](example-daily-routine.md) for more detailed information on keeping healthy instances.
{% endhint %}

