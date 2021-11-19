---
description: >-
  A schedule uses a CRON expression to determine how often source inputs are
  updated. Multiple sources can be assigned to a single schedule.
---

# Schedules

## Schedules

Schedules was previous found under the Source Settings parameter table (<2.4.0). It now has its own page which can be accessed from the main menu; simply hit Schedules to be taken to the list of Schedules.

{% hint style="info" %}
The schedule description is always the parsed CRON expression. Names can be anything, but best practice is to have a CRON expression present or use time specific statement.
{% endhint %}

![](../.gitbook/assets/schedules\_002.png)

## Schedule Settings

The schedule settings page allows users to create and update schedules. The example below will run at 41, 42, 43, and 45 minutes past the hour.

![](../.gitbook/assets/schedules\_003.png)

* **Name\*: **A unique name. Best practice has a CRON expression present.
* **Seconds: **Number of seconds ranging from 0-59 - Allowed Special Characters: , - \* /
* **Minutes: **Number of minutes ranging from 0-59 - Allowed Special Characters: , - \* /
* **Hours: **Number of hours ranging from 0-23 - Allowed Special Characters: , - \* /
* **Day of Month: **Specific days of the month as numerals ranging from 1-31 - Allowed Special Characters: , - \* ? / L W C
* **Month: **Specific months using 1-12 or JAN-DEC - Allowed Special Characters: , - \* /
* **Day of Week: **Specific days of the week as numerals ranging from 0-6 (0=Monday 6=Sunday) - Allowed Special Characters: , - \* ? / L C #
* **Error Retry Count\*: **Number of times the input will retry upon error (_default 3)_
* **Error Retry Wait\*: **Number of seconds the input will wait before retry _(default 60)_

## Add a Schedule to a Source

In order to add a schedule to a source, simply navigate to the source settings page of your desired source. There, select the desired schedule from the Schedules dropdown. The dropdown is located directly beneath the **Initiation Type**.

{% hint style="info" %}
The schedules dropdown will not appear for loopback connections nor watcher initiation types.
{% endhint %}

![](<../.gitbook/assets/schedules\_004 (1).png>)

## CRON Expressions

CRON (Command Run On) is used to create repetitive schedules. Use the following rules to create a CRON expression:

| **Field**    | **Allowed Values** | **Allowed Special Characters** |
| ------------ | ------------------ | ------------------------------ |
| Seconds      | 0-59               | , - \* /                       |
| Minutes      | 0-59               | , - \* /                       |
| Hours        | 0-23               | , - \* /                       |
| Day of month | 1-31               | , - \* ? / L W                 |
| Month        | 1-12 or JAN-DEC    | , - \* /                       |
| Day of week  | 0-6 or MON-SUN     | , - \* ? / L #                 |

{% hint style="info" %}
You must specify either day of month or day of week, but not both. Insert a question mark (?) as a placeholder for the one not specified.

The names of months and days of the week are not case sensitive. "MON" is the same as "mon".
{% endhint %}

### Using Special Characters

The following table describes the legal special characters and how you can use them in a CRON expression:

| **Special Character**                        | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <p><code>*</code><br>(all values)</p>        | <p>Selects all values within a field.</p><p>For example, * in the minute field selects "every minute".</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| <p><code>?</code><br>(no specific value)</p> | <p>Used to specify something in one of the two fields in which the character is allowed, but not the other.</p><p>For example, to make the trigger fire on a particular day of the month (say, the 10th), when it does not matter what day of the week that happens to be, put <code>10</code> in the day-of-month field, and <code>?</code> in the day-of-week field.</p>                                                                                                                                                                                                                                                                                                                                                                                                             |
| <p><code>-</code><br>(range)</p>             | <p>Used to specify ranges.</p><p>For example, <code>10-12</code> in the hour field selects the hours 10, 11 and 12.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| <p><code>,</code><br>(comma)</p>             | <p>Used to specify additional values.</p><p>For example, <code>MON,WED,FRI</code> in the day-of-week field means the days Monday, Wednesday, and Friday.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| <p><code>/</code><br>(forward slash)</p>     | <p>Used to specify increments.</p><p>For example, <code>0/14</code> in the seconds field means the seconds 0, 14, 28, and 42; and <code>2/14</code> in the seconds field means the seconds 2, 16, 30, and 44.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <p><code>L</code><br>(last)</p>              | <p>Used differently in each of the two fields in which it is allowed:</p><ul><li>In the day-of-month field, <code>L</code> selects the last day of the month, which is 31 for January and 29 for February on leap years.</li><li>When used in the day-of-week field by itself, it means Saturday. But if used in the day-of-week field after another value, L selects the last xx day of the month. For example, <code>6L</code> selects the last Friday of the month.</li></ul><p>When using the <code>L</code> special character, do not specify lists, or ranges of values, because this may give confusing results.</p>                                                                                                                                                            |
| <p><code>W</code><br>(weekday)</p>           | <p>Used to specify the weekday (Monday-Friday) nearest to the given day.</p><p>For example, if you specify <code>15W</code> as the value for the day-of-month field, the nearest weekday to the 15th of the month is selected. So if the 15th is a Saturday, Friday the 14th is selected. If the 15th is a Sunday, Monday the 16th is selected. If the 15th is a Tuesday, Tuesday the 15th is selected.</p><p>However if you specify <code>1W</code> as the value for day-of-month, and the 1st is a Saturday, Monday the 3rd is selected, as the selection rules do not allow for crossing over the boundary of a month's days to the previous or the subsequent month.</p><p>The <code>W</code> character can only be used to specify a single day, not a range or list of days.</p> |
| `C `(calendar)                               | This characters is short-hand for "calendar" and is only allowed for the day-of-month and day-of-week fields. This means values are calculated against the associated calendar, if any. If no calendar is associated, then it is equivalent to having an all-inclusive calendar. A value of "5C" in the day-of-month field means "the first day included by the calendar on or after the 5th." A value of "1C" in the day-of-week field means "the first day included by the calendar on or after Tuesday"                                                                                                                                                                                                                                                                             |
| `#`                                          | <p>Used to specify the nth XXX (or XX) day of the month.</p><p>For example, the value <code>FRI#3</code> or <code>6#3</code> in the day-of-week field means the third Friday of the month (<code>6</code> or <code>FRI</code> = Friday, and <code>#3</code> = the 3rd one in the month).</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |

{% hint style="info" %}
The `L` and `W` characters can also be combined in the day-of-month field to yield `LW`, which translates to "last weekday of the month".
{% endhint %}
