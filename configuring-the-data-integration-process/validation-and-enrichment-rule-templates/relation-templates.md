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

* 
