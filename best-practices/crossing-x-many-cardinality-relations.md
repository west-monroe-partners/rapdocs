---
description: How to properly cross one-to-many and many-to-many relationships
---

# !! Crossing X:Many Cardinality Relations

In most situations, crossing to the Many side of a one-to-many / many-to-many relationship should be discouraged, due to that traversal causing a grain change.  Generally, this would be considered a sign that a lower grain of data needs to be the driving grain in order to meet requirements.  However, there are situations where requirements will call for crossing a one-to-many or many-to-many relationship for a calculation or a value.

Some examples where this may be needed include the following:

* Crossing a junction table to get a single value \(employee serving a specific role for a client, single 
* Getting the min / max record from a header / detail relationship \(for example, scenarios where the source system is not fully normalized / data is stored at the wrong grain\)
* Calculating an aggregation of measures on a lower-grain table in order to calculate another measure on the higher grain table

{% hint style="info" %}
The primary goal of crossing to the Many side of a relation is to get a singular value or record.  If the logic does not support that goal, consider whether either the logic or the driving grain needs to be modified.
{% endhint %}

### Getting a single record

TODO - copy from slides

### Getting an aggregated value

TODO - copy from slides

### Crossing though a chain of multiple Many cardinality relations

TODO - copy from slides



