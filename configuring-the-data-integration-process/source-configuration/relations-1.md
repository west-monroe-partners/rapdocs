---
description: >-
  A Relation allows users to access attributes from different Sources (within
  the Source the Relation is made) and use these attributes in Enrichment rules
  and other logic further down the pipeline.
---

# !! Relations

## Creating Relations

To create a Relation, select a Source from the Sources screen, select the Relations tab, and click "New Relation" button in the top-right corner of the screen. Once the button has been clicked, the create Relation Modal will open. Fill out all the required parameters and all desired optional parameters \(more info below\) and press the save button to create your new relation. The save button will be disabled until all required parameters are filled in, and a valid expression has been entered.

![New Relation Button](../../.gitbook/assets/image%20%28302%29.png)

![Create Relation Modal](../../.gitbook/assets/image%20%28298%29.png)

## Relation Properties

* **Relation Name:** __The name of the Relation must be unique in case more than one relation is defined between a pair of sources. If no relation name is specified, the relation name will default to the pattern: '_Current Source Name - Related Source Name'_
* **Related Source:** Specifies the source for which a relationship with the current source is being defined. 
* **Relation Expression:**  This is a boolean expression written in SQL that "joins" the current Source \(denoted by "\[This\]"\) to the related Source \(denoted by "\[Related\]"\). The Relation will return 0, 1, or many records depending on the result of the expression. 
* **Primary Flag:** Marks a relation as the main relation between the two sources, for when there are multiple relations between a pair of sources. Ideally, the primary relation will be a standard foreign key relation.
* **Active Flag**: Indicates if the Relation is currently available to Rule expressions and can be accessed during data processing.

{% hint style="info" %}
Across the Intellio DataOps \(RAP\) platform, a grey \(un-clickable\) "Save" button indicates there is an error with the parameters. Typically this error is within an expression field. Double check errors and expressions if you are unable to "Save" your work.
{% endhint %}

## Graph View vs Table View



## Primary Relations

Primary relations are relations that are designated by the user to be the main relation between a pair of sources.  The first relation between a pair of sources will be automatically set as the primary relation, this can be changed by toggling the Primary Flag on the create/edit Relation Modal. 

![Primary Flog Toggle circled at the bottom right](../../.gitbook/assets/image%20%28299%29.png)

In practice, the most important property of primary relations is that they can be accessed using the shorthand Intellio QL pattern, instead of the longhand pattern. The shorthand pattern allows the primary relation to be accessed using only the name of the related source: _\[Related Source Name\].attribute\_name_. While the long hand pattern requires the user to specify both the related source, as well as the relation name: _\[Related Source Name\]~{Relation Name}.attribute\_name._

## Relation Expressions

The relation expression will define how the resulting data will look, see the Relation Example below for further details. See the [Intellio™ QL](https://app.gitbook.com/@intellio/s/dataops/v/master/configuring-the-data-integration-process/expressions) page for more details on Expressions.

### Example Relation Expressions

#### _For relating a pair of tables by a foreign key_

> \[This\].ProductID = \[Related\].ProductID

#### _For relating a table by finding a key within one of many fields_

> \[This\].CityName IN \(\[Related\].DepartureCity, \[Related\].ArrivalCity\)

#### _For relating a table by finding a quantity within a range_

> \[This\].Subtotal BETWEEN \(\[Related\].Subtotal - 10\) AND \(\[Related\].Subtotal + 10\)

