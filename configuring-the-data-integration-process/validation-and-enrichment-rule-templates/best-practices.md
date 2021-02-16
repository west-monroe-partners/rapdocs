---
description: >-
  Best practices, patterns and recommendations for template configuration and
  managment
---

# Best Practices

### Do

* When first starting to work on new data source, avoid using templates. Initially it is much easier to work with Rules/Relations and then convert them to templates once you have fully developed and tested your transformations
* Use rule templates to centralize and simplify management of rules with identical or nearly identical structure
* Define and enforce transparent naming convention for Source, Relation, Rule and Attribute names. This will make tokenization
* Think of each token as a master data element and create and enforce tokenization plan before you start making tokens. Tokens provide powerful way to provide flexible transformation logic but they can quickly become unmanageable if same structure is not followed for each source group using them
* Clearly define value that token should contain in token description and provide most common default value as an example
* Make sure token values cover only variable portion of template attributes

### Don't

* Tokenize template if logic and attribute names are identical for each source
* Mix source/system references with processing logic in the same token,. e.g. ${token} = SUM\(\[Transactions\].sales \* 0.2\) 
* Include identical/static elements into the token value

