# Creation & Configuration

### Managing Templates

Selecting Templates->Rule Templates from main menu opens Rule Templates list view :

![](<../../../.gitbook/assets/image (109).png>)

### Creating new Rule Template

Clicking NEW + button opens new template Setting tab:

![New Rule Template Settings](<../../../.gitbook/assets/image (32).png>)

You need to select Test Source before you can start configuring template attributes. Test source is required for enabling auto-complete attribute dropdown for \[This] source container and related sources. Token values configured for the selected Test Source will be used for validation of the Template. These values can be updated by the user.

![New Rule Template Settings Tab](<../../../.gitbook/assets/image (102).png>)

Rule Template is configured identically to [Rule](../../source-configuration/enrichment-rule-configuration.md). Following attributes of the Rule Template can be tokenized using _${token\_name}_ syntax:

* Template Name
* Attribute Name
* Expression

Typing $ will display searchable list of configured tokens:

![Using Token in Rule Template attributes](<../../../.gitbook/assets/image (79).png>)

Common examples of token use

| Example                                         | Description                                  |
| ----------------------------------------------- | -------------------------------------------- |
| \[This].${token}\_id                            | Parametrize attribute used in the expression |
| \[${token}].id                                  | Parameterize related source name             |
| \[This]\~ {${token\}}  \[Source name].attribute | Parametrize relation name                    |
| attribute\_${token}                             | Parametrize attribute name                   |
| Validate customer ${token}                      | Parametrize rule name                        |

### Linking (applying) the template to sources

Once template has been validated and saved, it can now be linked (applied) to sources. When user links the template to sources, system substitutes token values used in template with values configured for each target source, generates and validates rules for each of the sources and saves them. To link the template to sources click on Linked Sources tab:

&#x20;

![Linked Sources Tab](<../../../.gitbook/assets/image (245).png>)

To link source(s), click NEW + button and select sources in searchable list.

{% hint style="info" %}
Note: sources already linked to the template are not shown&#x20;
{% endhint %}

![Select source dialog](<../../../.gitbook/assets/image (237).png>)

After selecting sources, click Validate button. System will evaluate each template and display dialog with validation status for each of the selected sources:

&#x20;

![Validation status dialog](<../../../.gitbook/assets/image (283).png>)

Sources marked green passed template validation and are ready for the template to be linked. Sources marked red didn't pass validation. Clicking on red status icon shows validation error details:

![Validation Error Details](<../../../.gitbook/assets/image (310).png>)

Clicking on source name opens new browser tab with source settings to help user correct validation errors (configure token values, check metadata, etc.). By clicking triple-dot action menu, user can remove source from the list to be linked and address issues at a later time. Once issues have been corrected, click RE-VALIDATE button to repeat checks:

&#x20;

![](<../../../.gitbook/assets/image (257).png>)

Once all validation checks have passed, SAVE button is enabled. Click it to link template to displayed sources. After saving, updated linked sources tab is displayed:

&#x20;

![Linked Sources](<../../../.gitbook/assets/image (268).png>)

Clicking on Source Name on the Linked Sources tab list opens up new browser tab with Linked source Rule dialog:

![Template-linked Rule Dialog](<../../../.gitbook/assets/image (320).png>)

Rules linked to Template cannot be edited directly. Actions available are:

| Action                  | Description                                                                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| View Rule Template Link | Opens Rule Template setting page                                                                                                      |
| Duplicate               | Clones Rule and opens it up in new dialog                                                                                             |
| Unlink                  | Removes link between Rule and Template and converts Template-linked rule to regular Rule                                              |
| Delete                  | Removes link between Rule and Template and physically deletes the rule. Equivalent to "Remove Linked Sources" action on Template page |

