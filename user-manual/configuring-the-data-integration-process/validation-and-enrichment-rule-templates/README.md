---
description: >-
  Templates combined with tokens enable centralized deployment and management of
  re-usable Rules and Relations across multiple sources with similar processing
  logic.
---

# Templates and Tokens

## Introduction and Terms

#### Token

Global variable that enables customization of Templates. Token values in free text format are defined for each source that has tokenized templates applied. 

#### Rule Template

Template that has structure of a Rule. Name, Attribute Name and Expression attributes can be tokenized. When template is linked to the Source, all tokenized attributes of the template are evaluated using token values defined for the Source, and single instance of Rule is saved. Rule Template cannot be linked to the same source multiple times. Whenever template is modified, Rules from all linked Sources are re-evaluated and updated. Rule template can be linked to each source

#### Relation Template

Template that shares structure with Relation. Name, Related Source Name Name and Expression attributes can be tokenized. When template is linked to the Source, all tokenized attributes of the template are evaluated using token values defined for the Source, and Relation is saved. Template cannot be linked to the same source multiple times. Linked Source becomes \[This\] source in the saved Relation. Whenever template is modified, Relations from all linked Sources are re-evaluated and updated.

####  

![Template-Token-Source relationship diagram](../../../.gitbook/assets/image%20%28186%29.png)

