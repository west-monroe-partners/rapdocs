# Overview

## Use Case

When configuring multiple Sources within IDO, it is common for repeated patterns of Relation logic to be created across multiple Sources. For example, imagine a "Master Product" source that contains information about ever product an organization sells. The "Master Product" source has Relations to multiple sales Sources, all with the Relation Expression "\[This].productID = \[Related].ProductID"

![Master Products](<../../../.gitbook/assets/image (400).png>)

In the event that the user needs to change the Relation expression, they unfortunately will need to edit the expression in three separate places, one for each Source. This process is time consuming and prone to user error. The user needs the ability to manage the expression for all three Sales Sources in one place. In IDO, this functionality exists as a Relation Template.

Relation Templates also have an additional usage when using the Clone functionality. Check out the examples [here ](../../cloning/cloning-with-relations/)to see how they can be used to control logic in the Cloning process.
