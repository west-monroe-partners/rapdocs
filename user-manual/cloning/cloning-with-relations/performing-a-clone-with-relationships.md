# Performing a Clone with Relationships

In the prior sections, we setup both Group Relation and Master Data Relations to control the logic of our Clone operation for Company 1. As a reminder, the setup for Company 1 is below.&#x20;

![Company 1 Setup](<../../../.gitbook/assets/image (381) (1).png>)

## Performing the Clone

To perform the clone, follow the steps described [here](../very-basic-cloning-example/performing-a-basic-clone.md), this time updating the Group name to Company 2. Be sure to validate all of the screen to ensure no unexpected objects are exported or cloned!

## The Results

After Cloning, two new Company 2 Sources have been created for Sales Order Detail and Sales Order Header. See the image below.

![Company 2 Relations](<../../../.gitbook/assets/image (377).png>)

A few things to notice

* Company 2 Sales Order Detail is related to Company 2 Sales Order Header because they have a Group Relation between them.
* Company 2 Sales Order Detail is related to the SAME Global Product as Company 1 Sales Order Detail because it is a Master Data Relation.
* Company 2 Sales Order Header is related to the SAME Global Territory and Global Customer Sources as Company 1 Sales Order Header because they are Master Data Relations
* Company 1 Sources are not directly related to any Company 2 Sources because they are in different Groups.

Additionally, looking at the Linked Sources tab for any of the Relation Templates will show that they are now applied to Sources for both Company 1 and Company 2. See the image below.

![The Relation Template is linked to Sources in both Groups](<../../../.gitbook/assets/image (380).png>)



We have successfully cloned a Group with both Master Data and Group Relations! In the next section we will observe how Rule Templates can be leveraged to control the behavior of Enrichment and Validation Rules in the Cloning process.
