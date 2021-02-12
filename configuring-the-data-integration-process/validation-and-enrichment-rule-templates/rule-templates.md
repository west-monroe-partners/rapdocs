---
description: Rule template management screens
---

# Rule Templates

Selecting Templates-&gt;Rule Templates from main menu opens Rule Templates list view :

![](../../.gitbook/assets/image%20%28109%29.png)

Creating new Rule Template

Clicking NEW + button opens new template Setting tab:

![New Rule Template Settings](../../.gitbook/assets/image%20%2832%29.png)

You need to select Test Source before you can start configuring template attributes. Test source is required for enabling auto-complete attribute dropdown for \[This\] source container and related sources. Token values configured for the selected Test Source will be used for validation of the Template. These values can be updated by the user.

![New Rule Template Settings Tab](../../.gitbook/assets/image%20%28102%29.png)

Rule Template is configured identically to [Rule](../source-configuration/enrichment-rule-configuration.md). Following attributes of the Rule Template can be tokenized using _${token\_name}_ syntax:

* Template Name
* Attribute Name
* Expression

Typing $ will display searchable list of configured tokens:

![Using Token in Rule Template attributes](../../.gitbook/assets/image%20%2879%29.png)

Common examples of token use

| Example | Description |
| :--- | :--- |
| \[This\].${token}\_id | Parametrize attribute used in the expression |
| \[${token}\].id | Parameterize related source name |
| \[This\]~ {${token}}  \[Source name\].attribute | Parametrize relation name |
| attribute\_${token} | Parametrize attribute name |
| Validate customer ${token} | Parametrize rule name |

