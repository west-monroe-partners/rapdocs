# Templatizing Group Relation Rules

Templatizing a Group Relation Rule is more difficult because of the references to Grouped Sources in the Rule expression. Let's take a look at the "Header Account Number" rule on the Company 1 Sales Order Detail Source.

![The Header Account Number Rule](<../../../.gitbook/assets/image (416).png>)

To begin, we will click the "Convert To Template" button and make a template based off of the existing Rule. Notice below that it was created with "Company 1" explicitly mentioned in the Rule expression.

![The Header Account Number Rule Template made based off of Company 1](<../../../.gitbook/assets/image (401) (1).png>)

If this Template were to be applied to the Company 2 Sales Order Detail Source, or any Sales Order Detail Source made by a subsequent Clone, the expression would be invalid due to its reference to "Company 1". In order to make this expression valid for usage across ANY Sales Order Detail Source, we must replace the "Company 1" with the ${GROUP} token as seen below. After replacing all mentions of the Group (there can be multiple in a single Rule), the update Template can be saved. It is now ready to be used when Cloning.

&#x20;

![The Header Account Number Rule Template updated to use the GROUP Token](<../../../.gitbook/assets/image (388).png>)

With these two styles of Rule Templates, users can create Rule Templates that will allow them to Clone Sources and maintain control over the multiples copies of Enrichment Rules that are made as a result.
