# Configuring a Master Data Relation Template

In the Company 1 Example, the Relations between the Company 1 Sources and the Global Sources are all Master Data Relations. This means that upon cloning, the user expects newly created Company 2 Sources to have a relation directly to the existing Global Sources, NOT to create new Global Sources. This section will detail how to configure IDO for this behavior.

## Converting Existing Relations into Templates

For this example, we will be converting the existing Relations between the Company 1 Sources and the Global Sources into Relation Templates. The beginning setup is below.

![Beginning Setup](<../../../.gitbook/assets/image (399) (1).png>)

The first step to creating Relation Templates is to Remove the Primary Flag from any Relation that will be templated. Relations CANNOT be Primary if they will be templated. To do this, we simply open each relation, flip the Primary toggle, and hit save.&#x20;

![De-primary the Relation](<../../../.gitbook/assets/image (398) (1).png>)

After hitting Save, reopen the Relation and hit the Convert To Template button. This will create a Relation Template that matches the current Relation EXACTLY. It will also apply that Template to the currently open Relation. After applying the Relation Template, the Relation is now locked. It can only be edited from the Relation Template screen.

![A locked Relation associated with a Template](<../../../.gitbook/assets/image (380).png>)

We wil repeat these steps for each of the three Relations between Company 1 Sources and Global Sources.&#x20;

## The Relation Template - Master Data Style

Let's click the "View Relation Template" button in the top right corner to take a look at the Template itself. Below is an image of the Relation Template.&#x20;

On the Template, there is one button that controls whether this Relation will be treated as a Master Data Relation or a Group Relation when Cloning, the "Related Source Type" toggle. For a Master Data Relation, we want this toggle to be set to "Source". This tells IDO that any source using this Template should be related to the EXACT Source listed in the Related Source box. For a Source using this Template, it does not matter which Group it is a member of, it will always relate to Global Product.

![A Master Data Style Relation Template](<../../../.gitbook/assets/image (396) (1).png>)

In the next section, we will create a Relation Template for a Group Relation using the "Source Template" button. Seeing both examples should help clarify why the "Source" button allows the Master Data Relation to behave properly when being cloned.
