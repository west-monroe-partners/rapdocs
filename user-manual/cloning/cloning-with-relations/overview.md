# Overview

## Summary and Use Case

As seen in the Very Basic Cloning example, single Sources can be copied into new Groups easily. However, real configuration setups are always substantially more complex, particularly because they contain logic around Relations. Let's look at the Company 1 configuration as an example. In the below image there are 4 relations. 2 of the Relations are between Company 1 Sales Order Header and Sources that exist outside of the Company 1 Group. 1 Relation is between Company 1 Sales Order Detail and a Source that exists outside of the Company 1 Group. The other Relation is a Relation between two members of the same Group, Company 1 Sales Order Header and Company 1 Sales Order Detail. The sections below will break down the desired Cloning behavior for each.

![Company 1 Relations](<../../../.gitbook/assets/image (394) (1).png>)

## Master Data Relations

The 3 Relations between Company 1 Sources and the Sources that exist outside of the Company 1 Group (Product, Territory, and Customer) are unique because there are not separate Product, Territory, or Customer Sources for each Group. When configuring Sources for Company 2, the user will want Company 2 Sources to be related to the EXACT same global Product, Territory and Customer. We call these "Master Data Relations". When the user performs a Clone operation, the desired behavior resembled the image below. Notice that after cloning, there is still a single Master Data Source, and it has been related to the newly created Source.

![A Master Data Relation Clone](<../../../.gitbook/assets/image (391) (1) (1).png>)





## Group Relations

The other Relation in the Company 1 diagram, between Company 1 Sales Order Header and Company 1 Sales Order Detail, is different from the above Master Data Relation because it exists between two sources in the same Group. When thinking of the Company 2 Sources that a Clone operation would create, the desired behavior is to create two brand new Sources and relate them to each other, not to relate the new Company 2 Sources to the Company 1 Sources. We call these "Group Relations". See the two images below for examples of desired and undesired behavior. Notice that in the desired behavior the newly created Group is completely independent from the Original Group.

![Desired Behavior for Group Relations](<../../../.gitbook/assets/image (383).png>)

![Undesired Behavior](<../../../.gitbook/assets/image (401) (1).png>)

## Example

Below is an example of the desired behavior when performing a Clone operation for Company 1. Notice the difference in behavior between the Master Data Relations and the Group Relations.&#x20;

![Desired Cloning behaviro with Master Data and Group Relations](<../../../.gitbook/assets/image (395) (1).png>)

The next sections will cover the creation of Relation Templates to control this Group vs Master Data logic when configuring objects in IDO. Note that it is recommended to use Relation template for ALL relations of a Source that will be cloned. For more information on Relation Templates and why they exist, check out the page [here](../../validation-and-enrichment-rule-templates/relation-templates/overview.md).
