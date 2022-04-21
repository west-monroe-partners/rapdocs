# Overview

## Use Case

When configuring multiple Sources within IDO, it is common for repeated patterns of Enrichment Rule logic to be created across multiple Sources. For example, imagine an environment with 3 Sales Sources that contains information about Sales that an organization makes across multiple stores. Each Source needs a rule for Profit, defined as \[This].Revenue - \[This].Expenses.

![3 Sales Sources with a Profit Enrichment](<../../../.gitbook/assets/image (414).png>)

In the event that the user needs to change the Enrichment expression, they unfortunately will need to edit the expression in three separate places, one for each Source. This process is time consuming and prone to user error. The user needs the ability to manage the expression for all three Sales Sources in one place. In IDO, this functionality exists as a Rule Template.
