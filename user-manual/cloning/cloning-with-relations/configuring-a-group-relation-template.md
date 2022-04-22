# Configuring a Group Relation Template

In the Company 1 Example, the Relation between Company 1 Sales Order Header and Company 1 Sales Order Detail is an example of a Group Relation. When the user Clones the Company 1 Sources to make Company 2, they will want a new Relation to be created between the Company 2 Sales Order Header and the Company 2 Sales Order Detail Sources. See below for a visual example.&#x20;

![Desired Group Relation behavior when Cloning](<../../../.gitbook/assets/image (393) (1) (1).png>)

## Converting Existing Relations into Templates

For this example, we will be converting the existing Relations between the Company 1 Sales Order Header and Company 1 Sales Order Detail Sources. The beginning setup is below.

![Beginning Setup](<../../../.gitbook/assets/image (388) (1).png>)

The first step to creating Relation Templates is to Remove the Primary Flag from any Relation that will be templated. Relations CANNOT be Primary if they will be templated. To do this, we simply open each relation, flip the Primary toggle, and hit save.&#x20;

![De-primary the Relation](<../../../.gitbook/assets/image (402) (1).png>)

After hitting Save, reopen the Relation and hit the Convert To Template button. This will create a Relation Template that matches the current Relation EXACTLY. It will also apply that Template to the currently open Relation. After applying the Relation Template, the Relation is now locked. It can only be edited from the Relation Template screen.

## The Relation Template - Group Relation Style

Let's click the "View Relation Template" button in the top right corner to take a look at the Template itself. Below is an image of the Relation Template.&#x20;

![](<../../../.gitbook/assets/image (387) (1).png>)

On the Template, there is one button that controls whether this Relation will be treated as a Master Data Relation or a Group Relation when Cloning, the "Related Source Type" toggle. For a Group Relation Relation, we want this toggle to be set to "Source Template". This tells IDO that any source using this Template should be related to the Source within its Group that uses the Soruce Templated listed in the Related Source Template box. For example, Company 1 Sales Order Header will want to relate to Company 1 Sales Order Detail, while Company 2 Sales Order Header will want to relate to Company 2 Sales Order Header.&#x20;

With this Relation Template created using the "Source Template" toggle, we have successfully made a Relation that will follow the Group Relation style when cloned! In the next section, we will perform the Clone operation and observe the resulting behavior.

