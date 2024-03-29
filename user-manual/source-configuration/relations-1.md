---
description: >-
  Relations define intra-source connections and enable users to configure
  lookups and cross-source aggregates
---

# Relations

## Creating Relations

To create a Relation, select a Source from the Sources screen, select the Relations tab, and click "New Relation" button in the top-right corner of the screen. Once the button has been clicked, the create Relation Modal will open. Fill out all the required parameters and all desired optional parameters (more info below) and press the save button to create your new relation. The save button will be disabled until all required parameters are filled in, and a valid expression has been entered.

{% hint style="info" %}
**Self Relations:** The user can define a relation between the current source and itself by selecting the current source in the related source modal. Self relations can _never_ be primary.
{% endhint %}

![New Relation Button](<../../.gitbook/assets/image (302).png>)

![Create Relation Modal](<../../.gitbook/assets/image (298).png>)

## Relation Properties

* **Relation Name:** __ The name of the Relation must be globally unique. If no relation name is specified, the relation name will default to the pattern: '_Current Source Name - Related Source Name'_
* **Related Source:** Specifies the source for which a relationship with the current source is being defined.&#x20;
* **Relation Expression:** This is a boolean expression written in SQL that "joins" the current Source (denoted by "\[This]") to the related Source (denoted by "\[Related]"). The Relation will return 0, 1, or many records depending on the result of the expression.&#x20;
* **Primary Flag:** Useful when there is only one valid relation between two sources, or a set of traversals between sources. Has no logical significance, but allows for a short-hand syntax in IntellioQL
* **Active Flag**: Indicates if the Relation is currently available to Rule expressions and can be accessed during data processing.

{% hint style="info" %}
Across the Intellio DataOps (RAP) platform, a grey (un-clickable) "Save" button indicates there is an error with the parameters. Typically this error is within an expression field. Double check errors and expressions if you are unable to "Save" your work.
{% endhint %}

## Graph View vs Table View

The Relations tab has two main views: the graph view and the table view.&#x20;

#### Graph View

The graph view allows users to see a visual representation of the current source, all sources related to the current source either directly or through a primary relation chain, and all the relations that connect these sources.

![Graph View](<../../.gitbook/assets/image (304).png>)

#### Table View

The table view allows users to quickly view, filter, search, and sort all relations that involve the current source.

![Table View](<../../.gitbook/assets/image (303).png>)

## Primary Relations

Primary relations are relations that are designated by the user to be the main relation between a pair of sources.  The first relation between a pair of sources will be automatically set as the primary relation, this can be changed by toggling the Primary Flag on the create/edit Relation Modal.&#x20;

![Primary Flog Toggle circled at the bottom right](<../../.gitbook/assets/image (299).png>)

In practice, the most important property of primary relations is that they can be accessed using the shorthand Intellio QL pattern, instead of the longhand pattern. The shorthand pattern allows the primary relation to be accessed using only the name of the related source: _\[Related Source Name].attribute\_name_. While the long hand pattern requires the user to specify both the related source, as well as the relation name: _\[This]\~{Relation Name}\~\[Related Source Name].attribute\_name._

## Relation Expressions

The relation expression defines the SQL applied to the ON condition of a JOIN statement. See the Relation Example below for further details. Expressions must resolve to a boolean value, and every relation expression must contain an instance of each of the source containers _\[This]_ and _\[Related]_ . In essence, each row in _\[This]_ source (in other words, the current source) is related to every row in the _\[Related]_ source in which the relation expression is true. See the [Intellio™ QL](https://app.gitbook.com/@intellio/s/dataops/v/master/configuring-the-data-integration-process/expressions) page for more details on expression syntax.

### Example Relation Expressions

#### _For relating a pair of tables by a foreign key_

> \[This].ProductID = \[Related].ProductID

#### _For relating a table by finding a quantity within a range_

> \[This].Subtotal BETWEEN (\[Related].Subtotal - 10) AND (\[Related].Subtotal + 10)

#### _For relating a table by using an inequality_

> \[This].Subtotal <= \[Related].Subtotal
