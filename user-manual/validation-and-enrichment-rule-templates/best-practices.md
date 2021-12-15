---
description: >-
  Best practices, patterns and recommendations for template configuration and
  managment
---

# Best Practices

### Do

* When first starting to work on new data source, avoid using templates. Initially it is much easier to work with Rules/Relations and then convert them to templates once you have fully developed and tested your transformations
* Use rule templates to centralize and simplify management of rules with identical or closely similar structure
* Define, document and enforce consistent naming convention for Source, Relation, Rule and Attribute names. Use it as foundation for your tokenization **plan**
* Think of each token as a master data element: create and enforce tokenization **plan** before you start making tokens. Tokens provide powerful way to provide flexible transformation logic but they can quickly become unmanageable if same structure is not followed for each source group using them
* Clearly define value that token should contain in token description and provide most common default value as an example
* Make sure token values cover only variable portion of template attributes

### Don't

* Force templates when processing rules differ substantially between sources. Use 80/20 as rule of thumb: 80% of the logic is identical, 20% is variable&#x20;
* Tokenize template if logic and attribute names are identical for each source. Tokens are optional and not required for Templates
* Avoid making token value too big: don't mix source/system references with processing logic in the same token,. e.g. ${token} = SUM(\[Transactions].sales \* 0.2)&#x20;
* Include identical, repeated or static elements into the token value
