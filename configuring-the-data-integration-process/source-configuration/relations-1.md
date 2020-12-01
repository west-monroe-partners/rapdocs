---
description: >-
  A Relation allows users to access attributes from different Sources (within
  the Source the Relation is made) and use these attributes in Enrichment rules
  and other logic further down the pipeline.
---

# !! Relations

## Creating Relations

To create a Relation, select a Source from the Sources screen, select the Relations tab, and click "New Relation" button in the top-right corner of the screen. Once the button has been clicked, the create Relation Modal will open. Fill out all the required parameters and all desired optional parameters \(more info below\) and press the save button to create your new relation. The save button will be disabled until all required parameters are filled in, and a valid expression has been entered.

![Create new Relation](../../.gitbook/assets/rap-relations-new.png)

![Create Relation Modal](../../.gitbook/assets/rap-relations-details-screen.png)

## Relation Properties

* **Relation Name:** __The name of the Relation must be unique because a Relation is simply a relationship between any two Sources in the RAP environment, and a unique identifier is needed to distinguish one Relation from another.

{% hint style="info" %}
 If no relation name is specified, the relation name will default to the following pattern:   
'_Current Source Name - Related Source Name'_
{% endhint %}

* **Related Source:** Specifies the related Source.
* **Relation Expression:**  This is a boolean expression written in SQL that "joins" the current Source \(denoted by "\[This\]"\) to the related Source \(denoted by "\[Related\]"\). The Relation will return 0, 1, or multiple records depending on the result of the expression. The relation expression will define how the resulting data will look, see the Relation Example below for further details. See the [Expressions](../expressions.md) page for more details on Expressions.
* **Primary Flag:** Marks a relation as the main relation between the two sources, for when there are multiple relations between a pair of sources. Ideally, the primary relation will be a standard foreign key relation. When making a new relation, if the new relation is between two sources that have no primary relation yet, the new relation will automatically be marked as primary. Practically, a Primary Relation is much easier to reference in Enrichments. If the Related Source via the defined Relation will be referenced many times in an Enrichment it is recommended to make it a Primary Relation.
* **Active Flag**: Indicates if the Relation is currently available to Rule expressions and can be accessed during data processing.

Click "Save" to finish creating the Relation.

{% hint style="info" %}
Across the Intellio DataOps \(RAP\) platform, a grey \(un-clickable\) "Save" button indicates there is an error with the parameters. Typically this error is within an expression field. Double check errors and expressions if you are unable to "Save" your work.
{% endhint %}

## Primary Relations



## Relation Expressions



