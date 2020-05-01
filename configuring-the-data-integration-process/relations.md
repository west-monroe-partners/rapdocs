---
description: >-
  By defining relationships between Sources of any type, users can access
  attributes from multiple Sources and use them in Enrichment rules further down
  the pipeline.
---

# Relations

Relations allows the user to define a relationship between 2 Sources. Through that Relation, the user has access to all of the attributes of both Sources when configuring Enrichment rules.

## Creating Relations

To create a Relation, select a Source from the Sources screen, select the Relations tab, and click "New Relation" in the top-right corner of the screen.

![](../.gitbook/assets/create-a-relation%20%281%29.jpg)

Relations have a few crucial properties:

* **Relation Name:** __The name of the Relation must be unique. Although Relations are created at the Source level, a Relation is simply a relationship between any 2 Sources in the RAP environment, and a unique identifier is needed to distinguish one Relation from another.
* **Related Source:** Specifies the related Source. After setting this property, RAP will automatically determine the _cardinality_ of the Relation.
* **Relation Expression:** \(below Related Source property\) This is a boolean expression written in SQL that "joins" the current Source \(denoted by "This"\) to the related Source \(denoted by "Related"\). The number of records that are returned by this expression depends on the Refresh Type of the related Source_._
* **Relation Cardinality:** Specifies the number of records the relation will return when referenced in an Enrichment rule. There are 2 types of cardinality:
  * **O:** Stands for "one". This means that the related Source is a Key Source and that the Relation will return _only 1 record_ when the Relation expression matches.
  * **M:** Stands for "many". This means that the related Source is of any Refresh Type other than Key and that the Relation will return _1 or more records_ when the Relation expression matches.
* **Primary Flag:** Specifies whether the Relation is a primary Relation. This property is intended for the Relation that will be referenced the most when configuring Enrichment rules since they are much easier to reference. A Source can have only 1 primary Relation.

   

![Relation Configuration Screen](../.gitbook/assets/relations-modal-example.jpg)

