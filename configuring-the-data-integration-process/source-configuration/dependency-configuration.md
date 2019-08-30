---
description: >-
  Dependencies allow Sources to wait until other Sources process as a strategy
  to maintain data integrity downstream.
---

# Dependencies

## How Dependencies Work

{% hint style="danger" %}
WARNING - CURRENTLY OUT OF DATE - EDITS PENDING ON DRAFT
{% endhint %}

Dependencies are managed from the Source screen. Dependencies force Sources to wait for other Sources to finish Validation and Enrichment first. The most common use for Dependencies includes Lookups. When performing a Lookup from one source \(Source A\) to another \(Source B\), users often want to wait for Source B to process first before populating the lookup fields in Source A.

RAP provides configurable delays between Sources that are dependent on each other to ensure accurate dependency management in imperfect production scenarios.

### Example Scenario: Source A Time Series, Source B Keyed

When the source you are looking up from \(Source A\) is of type Timeseries - Timestamp, RAP will use the time range from the file to assist with dependency management.

In the example below, Source A has a date\_column spanning 24 hours from 3PM on 01/03/2020 to 3PM on 01/04/2020. To ensure the keyed lookup source \(Source B\) has up to date information for all times within the range in Source A, RAP take the latest time in Source A - 3PM on 01/04/2020, and compares it to the most recent time Source B was extracted from the source system.

If Source B's most recent extract\_datetime occurs after the latest date time in Source A's data, Source A can run immediately. However, if Source B's extract date occurs before Source A's latest date time, then Source A will wait to execute any processing steps. Once Source B's schedule or file push creates a new Input at a date time after Source A's latest date time - 3PM on 01/04/2020, Source A will then execute.

![One Timeseries Timestamp source, with a lookup Keyed Source dependency](../../.gitbook/assets/image%20%2871%29.png)

![Two Keyed Sources, with one acting as a lookup Keyed Source dependency](../../.gitbook/assets/image%20%2840%29.png)

### The Interval Parameter

The interval parameter gives users the ability to skew the dependency time logic in a given direction. 

#### Negative Intervals

Configuring a negative interval allows some leniency in the dependency when comparing Source A's latest date time to Source B's latest extract time.

An example use case is if both Source A and Source B use a file upload process to push data into RAP. If both files begin the upload process at the same time, but Source B's data is much smaller than Source A's, Source B may arrive much earlier than Source A - even though they both apply to the same snapshot time from the source system. In this case, we know we do not want to wait to process Source A, so we can apply a negative interval band to account for this difference in upload time.

#### Positive Intervals

Configuring a positive interval forces Source A to wait, even if Source B's extract date occurs after Source A's latest date time. This is useful if the information in Source B typically lags behind information in Source A. If Source A's data applies to data through 3PM 01/04/2020, and an extract of Source B arrives within RAP on 3:30PM 01/04/2020, but the information contained in Source B actually applies to the previous day's information 01/03/2020, a RAP user would consider configuring a -1 day interval to ensure all records in Source A have valid matches in Source B.

## Dependencies Tab

The Dependency tab allows users to see all previously created Dependencies, as well as search, edit and filter them. By default, only Active Dependencies are listed. The **Active Only** toggle changes this setting.

![Source Dependencies - Active Only](../../.gitbook/assets/image%20%28185%29.png)

To edit a Dependency, select the Dependency directly. This opens the Edit Dependency modal.

![Select a Dependency to Edit](../../.gitbook/assets/image%20%28117%29.png)

To add a Dependency, select **New Dependency**. This opens the Edit Dependency modal for a new Dependency.

![Source Dependencies - New Dependency](../../.gitbook/assets/image%20%2834%29.png)

## Edit Dependency Modal

On the Edit Dependency modal, users can modify a specific Dependency's details.

![Edit Dependency](../../.gitbook/assets/image%20%2874%29.png)

#### Fields Available:

| Parameter | Default Value | Description |
| :--- | :--- | :--- |
| **Source\*** |  | This is the Source that this Source depends on finishing first. |
| **HH** |  | Hours |
| **MM** |  | Minutes |
| **SS** |  | Seconds |
| **Hard Dependency** | FALSE | If activated, this forces the Source to wait for the other Source to finish running. If this is not activated, the Dependency does not wait and does not limit the Source. |
| **Active** | TRUE | If not activated, this Dependency does not affect a Source and the Dependency will be automatically deleted. |



