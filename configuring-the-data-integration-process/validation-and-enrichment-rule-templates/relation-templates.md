---
description: Relation template management scerens
---

# Relation Templates

### Managing Templates

Selecting Templates-&gt;Relation Templates from main menu opens Relation Templates list view :

![Relation Template List](../../.gitbook/assets/image%20%28267%29.png)

### Creating new Relation Template

Clicking NEW + button opens new template Setting tab:

![New relation template](../../.gitbook/assets/image%20%28301%29.png)

Select the test source to configure and validate template. Test source you selected will be used as \[This\] source in relation expression.

{% hint style="info" %}
Note: source you selected will be used for template validation only and will not be saved
{% endhint %}

Once test source is selected, system will prompt you to select Related source:

![Related Source selection](../../.gitbook/assets/image%20%28252%29.png)

Related source name defines \[Related\] source container for the relation expression.

{% hint style="info" %}
Note: Related source name is a text string and it allows tokenization. When template is linked to source, token value will be substituted and Related source matched by it's unique name. 
{% endhint %}

This mechanism enables pattern for defining parallel relation paths, e.g. :

Source \[Transactions A\] relates to source \[Customers A\] using Relation Template with these settings:

| Attribute | Value |
| :--- | :--- |
| Related Source Name | Customers ${System} |
| Relation Expression | \[This\].CustomerID = \[Related\].CustomerID |

Here is an example of Rule Template expression using the Relation Template above :

_\[Customers ${System}\].LastName_

### Validating and Saving New Relation Template

Once Test source and Related Source parameters have been filled, system displays Relation Template settings page:

![](../../.gitbook/assets/image%20%28314%29.png)

The attributes of Relation Template map directly into attributes of [Relation](../source-configuration/relations-1.md)

{% hint style="info" %}
Test source and Token Values listed at the bottom of Relation Template settings tab on grey background are used only for the template validation and are not saved with the template.
{% endhint %}

Once you've updated Template attributes and filled in Test Source and Token Values, click TEST button to validate the Template. Any validation errors will be displayed in red below Relation Expression. Once Template is validated, SAVE button is enabled and the Template can now be saved.

### Linking \(applying\) the template to sources

Once Relation Template is saved, it can now be applied to sources using Linked Sources tab:

![Relation Template Linked Sources Tab](../../.gitbook/assets/image%20%28315%29.png)

To link Template to source\(s\), click NEW + button and select one ore more sources in the dialog and click VALIDATE:

![](../../.gitbook/assets/image%20%28320%29.png)

System applies token values configured for the selected sources, creates Relation objects based on the Template and validates them for each of the selected sources. Results of the validation are summarized on the next dialog:

![Relation Template Validation Dialog](../../.gitbook/assets/image%20%28317%29.png)

Sources with green status successfully passed validation. Sources red status icon have failed and require corrections before template can be applied. Clicking on red status icon displays validation error details.

Once errors are corrected, click RE-VALIDATE. Once validation has passed, SAVE button is enabled. Saved linked sources appear on Linked Sources tab:

![](../../.gitbook/assets/image%20%28319%29.png)

