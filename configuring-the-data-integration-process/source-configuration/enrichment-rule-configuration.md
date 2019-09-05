---
description: >-
  Enrichment Rules allow RAP to modify and transform data as it is brought in.
  Each enrichment rule creates a new column.
---

# Enrichment Rules

Enrichment Rules are managed from the Source screen. Enrichments provide the logic for adding new columns to the data. 

Enrichments can take two forms: **Formula** and **Lookup**. 

**Formula**-based enrichment rules populate a new field with the logic of a configured SQL expression. Example:

```sql
T.revenue - T.cost
```

**Lookup**-based enrichment rules join disparate data sources on a specified criterion, and populate a new field with the logic of a configurable SQL expression. Example:

```sql
/* LOOKUP EXPRESSION: */
L.id = T.customer_id

/* RETURN EXPRESSION: */
L.customer_name
```

Enrichments can be created as Enrichment Rules Templates to allow re-usability. More details about Enrichment Rule Templates can be found on the [Validation and Enrichment Rule Templates page](../validation-and-enrichment-rule-templates.md).

{% hint style="info" %}
Note: The supported syntax in the expression input is specific to PostgreSQL. Refer to PostgreSQL documentation: [https://www.postgresql.org/docs/10/functions.html](https://www.postgresql.org/docs/10/functions.html)
{% endhint %}

## Enrichments Tab

The Enrichments tab allows users to select, edit, remove, or add a Source's Validations. By default, only Active Enrichments are listed. The **Active Only** toggle changes this setting.

![Source Enrichments - Active Only](../../.gitbook/assets/image%20%28195%29.png)

To edit an Enrichment, select the Enrichment directly. This opens the Edit Enrichment modal.

![Source Enrichments - Select an Enrichment to Edit](../../.gitbook/assets/image%20%28228%29.png)

To add a Validation, select **New Validation**. This opens the Edit Validation modal for a new Dependency.

![Source Enrichments - New Enrichment Rule](../../.gitbook/assets/image%20%285%29.png)

## Enrichment Parameters

On the Edit Enrichment modal, users can modify Enrichment Rule parameters, or apply an existing [Template ](../validation-and-enrichment-rule-templates.md)using the **Enrichment Rule Type** dropdown. Selecting **Enforce** ensures that a Template cannot be modified and is only configurable through the [Validation and Enrichment Rule Templates](../validation-and-enrichment-rule-templates.md) screen, while leaving **Enforce** unchecked copies the Template into a rule specific to the Source.

![Edit Enrichment Modal](../../.gitbook/assets/image%20%28220%29.png)

{% hint style="info" %}
Note: **Save as Rule Type** allows users to save Enrichment Rules as templates for later use. For more details, see [Validation and Enrichment Rule Templates](../validation-and-enrichment-rule-templates.md).
{% endhint %}

**Fields Available:**

| Parameter | Default Value | Description |
| :--- | :--- | :--- |
| **Name\*** |  | The user-defined name of the Enrichment Rule |
| **Description\*** |  | The user-defined description of the Enrichment Rule |
| **Enriched Column Name\*** |  | The name of the new column created by the Enrichment Rule |
| **Rule Type** |  | Configures this rule to be managed from an [Enrichment Template](../validation-and-enrichment-rule-templates.md). If chosen, all configuration is grayed out, and any modifications must be done in the parent template |
| **Enriched Column Data Type** | Text | This can be Text, Numeric, or Timestamp |
| **On conversion error set to** | Warn | These are the flags that will be set on records that fail to be converted to either Numeric or Timestamp. Warn, Fail, or Ignore are the possible options. |
| **Operation Type** | Formula | This can be either Formula or Lookup. For Lookups, see below. |
| **Return Expression** |  | Use SQL syntax to set the Enrichment Rule transformation logic. |
| **Active** | TRUE | Allows the user to set this Validation as Active or not. If Active, it affects the Source load. |

## Lookups

Lookups can be used to add data to a Source from a different Source. They work similarly to the Excel VLOOKUP formula. Lookups can only return one record per row. If RAP detects when a lookup may return more than one record per row, it will prompt a user to specify an `ORDER BY` statement in the parameters to sort the results and select only the `TOP 1` result.



![Lookup Configuration](../../.gitbook/assets/image%20%28171%29.png)

### Lookup-Specific Parameters

* **Lookup Source:** Separate Source that the Enrichment is pulling data from.
* **Lookup Expression:** The expression that specifies the `JOIN ON` condition between the Source and Lookup Source, such as `L.bike_id = T.bike_id`. The prefix L refers to the Lookup Source and the prefix T refers to the current Source. When possible, use RAP's `s_key` to join the Lookup Source: The `s_key` represents the Primary Key columns of the Source, pipe delimited. 
  * For example: `456|Chicago|15`
* **Automatic Reprocessing:** Specifies when the current Source records will be re-processed upon Lookup Source refresh.
  * **None**: The Enrichment is not automatically re-ran when the Lookup Source has updated data.
  * **New**: The Enrichment automatically re-runs only on the newly added rows that have been added to the Lookup Source.
  * **All**: The Enrichment automatically re-runs on all updated or new rows when the Lookup Source received a new Input of data.
* **Select Lookup Column\(s\) to Order Returned Results**: This specifies which columns the `ORDER BY` is run against. Comma-separated list of column names.

