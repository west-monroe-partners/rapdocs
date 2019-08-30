---
description: >-
  Validation Rules enable RAP to identify data quality issues or improper logic
  and flag these records for downstream handling.
---

# Validation Rules

Validation Rules are managed from the Source screen. 

{% hint style="info" %}
Note: Validation Rules only set a Pass/Warn/Fail flag. Further behavior based on these flag values can be specified in [Outputs](../output-configuration/).
{% endhint %}

Validation Rules can be created as Validation Rule Templates to allow re-usability. More details about Validation Rule Templates can be found on the [Validation and Enrichment Rule Templates page](../validation-and-enrichment-rule-templates.md).

{% hint style="info" %}
Note: The supported syntax in the expression input is specific to PostgreSQL. Refer to PostgreSQL documentation: [https://www.postgresql.org/docs/10/functions.html](https://www.postgresql.org/docs/10/functions.html)
{% endhint %}

## Validations Tab

The Validation tab allows users to select, edit, remove, or add a Source's Validations. By default, only Active Validation Rules are listed. The **Active Only** toggle changes this setting.

![Source Validations - Active Only](../../.gitbook/assets/image%20%28141%29.png)

To edit a Validation Rule, select the Validation Rule directly. This opens the Edit Validation Rule modal.

![Source Validations - Select a Validation to Edit ](../../.gitbook/assets/image%20%28178%29.png)

To add a Validation Rule, select **New Validation** **Rule**. This opens the Edit Validation Rule modal for a new Validation Rule.

![Source Validations - New Validation Rule](../../.gitbook/assets/image%20%2873%29.png)

## Validation Rule Parameters

On the Edit Validation Rule modal, users can modify Validation Rule parameters, or apply an existing [Template ](../validation-and-enrichment-rule-templates.md)using the **Validation Rule Type** drop down.

![Edit Validation Rule](../../.gitbook/assets/image%20%2849%29.png)

{% hint style="info" %}
Note: **Save as Rule Type** allows users to save Validation Rules as templates for later use. For more details, see [Validation and Enrichment Rule Templates](../validation-and-enrichment-rule-templates.md).
{% endhint %}

**Fields Available:**

| Parameter | Default Value | Description |
| :--- | :--- | :--- |
| **Name\*** |  | The user-defined name of the Validation Rule. Must be unique for this Source. |
| **Description\*** |  | The user-defined description of the Validation Rule. |
| **Rule Type** |  | Configures this rule to be managed from a [Validation Template](../validation-and-enrichment-rule-templates.md). If chosen, all configuration is grayed out, and any modifications must be done in the parent template |
| **When expression is true, set to** | Warn | The user can set this to be Warn or Fail. Each of those will set up a flag that can be used later in the Output Configuration to filter out Warned or Failed rows. |
| **Expression\*** |  | Expression to determine if the record is valid or not. Must evaluate to a boolean. If true, Validation Rule will warn/fail. If false, Validation Rule passes. Any SQL WHERE clause statement is valid. |
| **Active** | TRUE | Allows the user to set this Validation Rule as Active or not. If Active, it affects the Source processing. |
| **Fail Input** | FALSE | If the Validation Rule is set to Fail and a record fails, the Source does not load. |

