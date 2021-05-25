---
description: Token management screens overview
---

# Tokens

Selecting Templates-&gt;Tokens from main menu opens token list view :

![Token list](../../../.gitbook/assets/image%20%2896%29.png)

### Creating a New Token

Clicking NEW + button on the token list screen opens up new Token Settings tab:

![Token Settings](../../../.gitbook/assets/image%20%2850%29.png)

Enter unique token name \(case sensitive\), detailed description and default value for the token \(used for initial validation of the Template\). Once new token is saved, Values tab becomes accessible. It allows to assign values for the token for one ore more sources. To add a value click NEW + button

![Token Values tab](../../../.gitbook/assets/image%20%28179%29.png)

### Updating Token Value

Token values are managed by clicking on Token Value column:

![Token Value dialog](../../../.gitbook/assets/image%20%28157%29.png)

Token values can also be managed from the Source Settings Tab:

![](../../../.gitbook/assets/image%20%28168%29.png)

Clicking on Token Name hyperlink opens up Token Value dialog.

{% hint style="info" %}
Note: when token value is changed for the source, system re-evaluates and validates all Rule and Relation Templates using this token that are applied to the Source. If validation is successful, all Relations and Rules for the Source using token are updated, otherwise system will display error message with validation failure details
{% endhint %}

