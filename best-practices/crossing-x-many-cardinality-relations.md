---
description: How to properly cross one-to-many and many-to-many relationships
---

# !! Crossing X:Many Cardinality Relations

In most situations, crossing to the Many side of a one-to-many / many-to-many relationship should be discouraged, due to that traversal causing a grain change.  Generally, this would be considered a sign that a lower grain of data needs to be the driving grain in order to meet requirements.  However, there are situations where requirements will call for crossing a one-to-many or many-to-many relationship for a calculation or a value.

Some examples where this may be needed include the following:

* Crossing a junction table to get a single value \(for example, getting an employee serving a specific role for a client, getting the primary contact name for a client with multiple contacts\)
* Getting the min / max record from a header / detail relationship \(for example, scenarios where the source system is not fully normalized / data is stored at the wrong grain\)
* Calculating an aggregation of measures on a lower-grain table in order to calculate another measure on the higher grain table

{% hint style="info" %}
The primary goal of crossing to the Many side of a relation is to get a singular value or record.  If the logic does not support that goal, consider whether either the logic or the driving grain needs to be modified.
{% endhint %}

### Getting an aggregated value

The simplest traversal scenario is aggregating across a relation to the Many side.  This can be done via an enrichment that simply aggregates the related field on the Many side of the relation, and DataOps will automatically aggregate up to the grain of your current source.

#### Example Scenario

Take an example where we have a set of Sales Orders, each having their own Line Item details.  For our purposes, we need to sum up the Line amounts up to the Sales Order.

![Example one-to-many relationship](../.gitbook/assets/image%20%28329%29.png)

In this scenario, we can create a simple aggregation enrichment rule on the Sales Order source to sum up the Line amounts as follows:

* SUM\(\[Sales Order Line\].\[LineAmount\]\)

{% hint style="info" %}
Note that aggregations require the scope of an entire source and need to be recalculated when that source is updated.  As such, those aggregations will need to be set to the "Keep Current" recalculation type and incur the resulting performance impact.
{% endhint %}

### Retrieving a single record

Retrieving a single record from the Many side of a relation requires a rule that can correctly select no more than a single record from that side of the relation.  To do so, a reliable way to specify all primary key values from the Many side of the relation given a record from the driving side of the relation needs to be determined.  In essence, this approach consists of determining a way to reduce the relation down to a M:1 or 1:1 relation \(for M:M and 1:M relations respectively\).

#### Example Scenario

Take for example a model where Locations can have multiple Attributes tied to them, each having different Attribute Types assigned in a a junction table.  For our purposes, suppose we want to get a single Location Type Attribute for all Locations.

![Example many-to-many relationship](../.gitbook/assets/image%20%28330%29.png)

In this scenario, the primary keys are the following:

_Location_

* LocationID

_LocationAttributeJunction_

* LocationID
* AttributeTypeCode

_Attribute_

* AttributeID

Relationships are also specified as follows:

* **Location &lt;-&gt; LocationAttributeJunction:**  Location.LocationID = LocationAttributeJunction.LocationID
* **LocationAttributeJunction &lt;-&gt; Attribute:**  LocationAttributeJunction.AttributeID = Attribute.AttributeID

To get from the Location table to the Attribute table, the relationship chain is 1:M + M:1, which is a combined M:M relation.  If traversing the chain only via standard joins, that would cause the grain of the Location table to be broken.

The way around this is knowing exactly which attribute type code in LocationAttributeJunction corresponds to the Location Type attribute.  Through analysis, suppose we determined that AttributeTypeCode = 10 corresponds to Location Type.

Knowing this, we can distill the 1:M relation between Location and LocationAttributeJunction down to a 1:1 relation by leveraging a relationship with the following condition:

* \[Location\].\[LocationID\] = \[LocationAttributeJunction\].\[LocationID\] AND \[LocationAttributeJunction\].\[AttributeTypeCode\] = 10

With this new relation in place, our relation chain from Location to Attribute becomes 1:1 + M:1, which combined is a M:1 relation.  Using this relation, the Attribute table can be traversed to from the Location table as usual \(making sure to specify the new 1:1 relation and not the normal 1:M relation\):

* \[This\]~\[Location to LocationAttributeJunction LocationType \(One to One\)\]~\[LocationAttributeJunction\]~\[Attribute\].\[AttributeValue\]

{% hint style="success" %}
Getting to a single value from the Many side of a 1:M or M:M relation requires distilling the Many side of the relation down to a One cardinality through additional filters.  When done correctly, the new relation can be traversed as normal \(without blowing out the driving source grain\).
{% endhint %}

### Crossing though a chain of multiple Many cardinality relations

TODO - copy from slides



