# Overview

When cloning Sources, ALL of their Enrichments will be cloned with them. See the Enrichments page for Company 1 Sales Order Header and Company 2 Sales Order Header as an example below. Notice that they have almost identical Enrichment Rules. This was done automatically by the Clone operation. Below we will analyze each individual rule and categorize them into 3 "types" of rules that will help define their behavior during a clone.

![Company 1 Sales Order Detail Enrichments](<../../../.gitbook/assets/image (396).png>)



![Company 2 Sales Order Detail Enrichment created by the Clone operation](<../../../.gitbook/assets/image (401).png>)

## \[This] Source Rules

The first rule to look at is "Double order quantity". This is a simple rule that takes a field from the Sales Order Detail Source and double it. It uses no Relations and relies only on fields available in the Sales Order Detail Source

![The Double order quantity Rule](<../../../.gitbook/assets/image (398).png>)

When cloning, rules like this that use no Related Sources or fields have very simple logic. They will simply be created on the new version of the Source with the exact same expression. No changes and no interesting logic is needed.

## Master Data Relation Rules

The second rule to look at is "Product Class". Notice that this Rules does utilize a Relation and pulls data from another Source, specifically the "Class" field from the Global Product Source.&#x20;

![The Product Class Rule](<../../../.gitbook/assets/image (389).png>)

In the [Cloning with Relations](../cloning-with-relations/) section, we analyzed the difference between Master Data Relations and Group Relations. This Rule utilizes a Master Data Relation, so when the Source is cloned, the new version will still be related to the exact same Global Product Source. Because of this, we again want to keep the exact same Relation expression for both Company 1 and Company 2, as they both are related to and pulling data from the same Global Product Source.

## Group Relation Rules

The third rule is unique compared to the previous two. "Header Account Number" utilizes a Relation to a Source in the same Group as our Sales Order Detail Source, specifically "Account Number" field from the Sales Order Header Source. Below is the Rule definition for the Company 1 Sales Order Detail Source. Notice that it specifically mentions the Company 1 Sales Order Header Source in the Rule expression.

![The Header Account Number Rule for Company 1](<../../../.gitbook/assets/image (399).png>)

This Rule utilizes a Group Relation, so when the Source is cloned, we will not want a lingering reference to the original Sources. i.e. We do not want Company 2 to still have an expression that tries to get data from a Company 1 Source. In this case, IDO will automatically update the Rule expression to preserve the intra-Group logic of Company 1 <-> Company 1 and Company 2 <-> Company 2. Observe below how the expression for Company 2 Sales Order Detail has only references to Company 2 in its expression.

![The Header Account Number Rule for Company 2](<../../../.gitbook/assets/image (400).png>)

## Best Practices

While IDO will do all of this Rule management automatically during clones, we HIGHLY recommend utilizing [Rule Templates](../../validation-and-enrichment-rule-templates/rule-templates/overview.md) to manage all of these Rules. This will allow users to manage all of these Rules from a single location so that they can be changed and updated easily. In the next pages, we will go over how to turn Rules into Rule Templates before the Clone process for each of the three Rule types explained above.

