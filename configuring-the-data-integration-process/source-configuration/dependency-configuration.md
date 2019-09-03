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

### Example Scenario: Source A Keyed, Source B Keyed

In the following example, we have two sources scheduled to pull data using two different schedules. Source A pulls data every 12 hours at 3AM and 3PM, Source B pulls data every 24 hours at 3PM. 

Setting a hard dependency on Source A to Source B - something that would automatically occur if a user configures a lookup from Source A to Source B - indicates that Source A should wait until there exists guaranteed time-valid data in Source B before performing the Enrichment and execute lookups and formulas.

Let's take a look at the first pull of data at 3PM:

![Example K1](../../.gitbook/assets/image%20%28103%29.png)

In this example, RAP will generate the input 1A four Source A, then identify all dependencies. In this case it's identified Source B as a hard dependency. Before running Validation and Enrichment, RAP will identify the maximum extract date time four all dependent sources and compare it to the current Input's extract date time. If the greatest extract date time across all inputs for Source B **is greater than or equal to** the current input's extract date for Source A, then Source A will run Validation and Enrichment.

For this case, because the greatest extract date time for Source B is equal to Input 1A's extract date time, Input 1A will not wait and will immediately begin executing Validation and Enrichment.

![](../../.gitbook/assets/image%20%28193%29.png)

Now let's take a look at what will happen when Source A pulls data again 12 hours later:

![](../../.gitbook/assets/image%20%283%29.png)

Source A has now pulled data on its 12 hour schedule, and will again trigger a dependency check. RAP will compare the current input's \(2A\) date time \(3AM on 01/04/2020\) to the greatest date time  across all inputs for Source B \(3PM on 01/03/2020\) and detect that there are no inputs for Source B with a date time greater than the current extract time of Source A. This time, Input 2A will **not** move to Validation and Enrichment, and will instead move to a "pending" status:

![](../../.gitbook/assets/pending%20%281%29.png) **Pending**: Phase is waiting on a dependent source to complete processing.

It will remain in this state until an Input from Source B satisfies the condition of the dependency.

![](../../.gitbook/assets/image%20%28113%29.png)

Finally, let's see what happens at 3PM later that day when the next Input for Source B is pulled:

![](../../.gitbook/assets/image%20%28207%29.png)

Now that RAP has generated a new Input for Source B, it immediately checks if there are any Inputs waiting on upstream dependencies. In this case, Input 2A is pending, and so RAP compares the date times between 2A and the greatest input extract time for Source B, which now exceeds the threshold for Source A's dependency, allowing Source A to begin Validation and Enrichment.

![](../../.gitbook/assets/image%20%28105%29.png)

Finally, RAP looks at the new Input for Source A, Input 3A, and compares the maximum date time from all extract date times for Source B, finds that the time is equal to Input 3A's extract time, and thus allows Input 3A to execute.

![](../../.gitbook/assets/image%20%28108%29.png)

## Dependencies Tab

The Dependency tab allows users to see all previously created Dependencies, as well as search, edit and filter them. By default, only Active Dependencies are listed. The **Active Only** toggle changes this setting.

![Source Dependencies - Active Only](../../.gitbook/assets/image%20%28216%29.png)

To edit a Dependency, select the Dependency directly. This opens the Edit Dependency modal.

![Select a Dependency to Edit](../../.gitbook/assets/image%20%28139%29.png)

To add a Dependency, select **New Dependency**. This opens the Edit Dependency modal for a new Dependency.

![Source Dependencies - New Dependency](../../.gitbook/assets/image%20%2841%29.png)

## Edit Dependency Modal

On the Edit Dependency modal, users can modify a specific Dependency's details.

![Edit Dependency](../../.gitbook/assets/image%20%2883%29.png)

#### Fields Available:

| Parameter | Default Value | Description |
| :--- | :--- | :--- |
| **Source\*** |  | This is the Source that this Source depends on finishing first. |
| **HH** |  | Hours |
| **MM** |  | Minutes |
| **SS** |  | Seconds |
| **Hard Dependency** | FALSE | If activated, this forces the Source to wait for the other Source to finish running. If this is not activated, the Dependency does not wait and does not limit the Source. |
| **Active** | TRUE | If not activated, this Dependency does not affect a Source and the Dependency will be automatically deleted. |



