---
description: >-
  Enrichment Rules allow RAP to modify and transform data as it is brought in.
  Each enrichment rule creates a new column.
---

# Enrichments

Enrichments are managed from the Source screen. Enrichments provide the logic for identifying data quality issues or adding new columns to the data. 

{% hint style="info" %}
Note: The supported syntax in the expression input is specific to PostgreSQL. Refer to PostgreSQL documentation: [https://www.postgresql.org/docs/10/functions.html](https://www.postgresql.org/docs/10/functions.html)
{% endhint %}

## Enrichments Tab

The Enrichments tab allows users to select, edit, remove, or add a Source's Enrichments. By default, only Active Enrichments are listed. The **Active Only** toggle changes this setting.

![Source Enrichments - Active Only](../../.gitbook/assets/image%20%28196%29.png)

To edit an Enrichment, select the Enrichment directly. This opens the Edit Enrichment modal.

![Source Enrichments - Select an Enrichment to Edit](../../.gitbook/assets/image%20%28229%29.png)

To create a new Enrichment, select **New Enrichment**. This opens the Enrichment modal.

![Source Enrichments - New Enrichment](../../.gitbook/assets/image%20%285%29.png)

## Enrichment Parameters

On the Enrichment modal, users can modify Enrichment parameters or apply an existing [Template ](../validation-and-enrichment-rule-templates.md)using the **Enrichment Rule Type** dropdown. Selecting **Enforce** ensures that a Template cannot be modified and is only configurable through the [Templates](../validation-and-enrichment-rule-templates.md) screen, while leaving **Enforce** unchecked copies the Template into a rule specific to the Source.

![Enrichment Modal \(PLACEHOLDER\)](../../.gitbook/assets/enrichments-modal-example%20%281%29.jpg)

**Fields Available:**

| Parameter | Default Value | Description |
| :--- | :--- | :--- |
| **Type** | Enrichment | The type of the Enrichment. Validations mark records as pass/fail based on a boolean expression in the expression field. |
| **Enrichment Name\*** |  | The user-defined name of the Enrichment |
| **Attribute Name\*** |  | The name of the new column of the Enrichment |
| **Description\*** |  | The user-defined description of the Enrichment |
| **Expression Data Type** |  | The data type of the result of the Expression. |
| **Attribute Data Type** |  | The data type of the Enriched Attribute. RAP will attempt to convert the data type of the Expression Data Type to the Attribute Data Type. Leave as Default for no conversion. |
| **When expression is false, set to** | Warn | These are the flags that will be set on records that fail to be converted to another data type. Warn, Fail, or Ignore are the possible options. For Validations only. |
| **Expression** |  | Use SQL syntax to set the Enrichment Rule transformation logic. |
| **Unique Value** |  | Signifies that the Enriched Attribute will have unique values for every record. |
| **Active** | TRUE | Allows the user to set this Validation as Active or not. If Active, it affects the Source load. |

Click **Save** to save the Enrichment. Clicking **Save and Create Validation** will create an extra Validation column to mark whether the values from the Expression Data Type succeeded the conversion to the specified Attribute Data Type.

## Using Relations in Enrichment Rules

Through Relations, users can access attributes from another Source when configuring Enrichment rules.  

![Enrichments Configuration Screen \(PLACEHOLDER\)](../../.gitbook/assets/enrichments-modal-example.jpg)

When configuring the Expression property on the Enrichment configuration screen, the user must use the expression syntax specified below to access the attributes.  

<table>
  <thead>
    <tr>
      <th style="text-align:left">Expression</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Examples</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">[<em>Source Name</em>]</td>
      <td style="text-align:left">Source container</td>
      <td style="text-align:left">[Divvy Rides]</td>
    </tr>
    <tr>
      <td style="text-align:left">[This]</td>
      <td style="text-align:left">Current Source container. Equivalent to [<em>current source name</em>]</td>
      <td
      style="text-align:left">[This]</td>
    </tr>
    <tr>
      <td style="text-align:left">[<em>Relation Name</em>]</td>
      <td style="text-align:left">Non-primary Relation name, indicates path to the Source containers used
        in expression</td>
      <td style="text-align:left">[This]~{To Station Relation}~[Divvy Rides].attribute</td>
    </tr>
    <tr>
      <td style="text-align:left">.</td>
      <td style="text-align:left">Separator of Source containers and attribute names</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">~</td>
      <td style="text-align:left">Path indicator, separates Source containers and Relations</td>
      <td style="text-align:left">[Divvy Rides]~{Relation Z}~[Weather].attribute</td>
    </tr>
    <tr>
      <td style="text-align:left">[<em>Relation</em>].<em>attribute_name</em>
      </td>
      <td style="text-align:left">Attribute in the container</td>
      <td style="text-align:left">
        <p>[Divvy Rides].trip_id</p>
        <p>[Divvy Stations].latitude</p>
        <p>[This]~{To Station Relation}~[Divvy Rides].longitude</p>
      </td>
    </tr>
  </tbody>
</table>## Relations and Enrichments Examples

To illustrate the proper use of Relations and Enrichments, let's examine the example Entity-Relationship Diagram of an arrangement of Sources in RAP below. We will use this ERD for both examples. The labels near the relationship lines are the names of the Relations that the user would have configured prior to creating any Enrichments. 

![Example ERD](../../.gitbook/assets/relations-erd%20%284%29.jpg)

#### 

### Example 1: Revenue By Customer

For this example, we need to focus on a particular section of the ERD shown below.

\(image\)

One useful metric to track in these kinds of data models is _revenue by customer._ To do this, let's first create an enriched column _Revenue_ on the Order\_Detail Source_._ The Enrichment expression for this would be `[This].OrderQty * [This].UnitPrice`. All of the attributes needed for this enriched column already exist on the Order\_Detail Source, so we don't need any Relations for this metric.

The Order\_Detail Source should now look like this:

![Order\_Detail after creating the enriched column Revenue](../../.gitbook/assets/order_detail-revenue.jpg)

The last part of capturing this metric is to retrieve the full name of the customer. Breaking this last step into two parts makes this task slightly easier. Since the cardinality of the Customer-Person Relation is O \(one\), it should be simple to store the full name of the customer in the Customer Source. Let's create an enriched column on the Customer Source called _Full\_Name_ with the Enrichment expression `[This]~{Customer-Person}~[Person].FirstName + [This]~{Customer-Person}~[Person].LastName` . 

Note the special syntax when using Relations in the expression. Relations can make the Enrichment expression quite long, but marking a Relation as the Primary Relation makes referencing it much easier. If the Customer-Person Relation is Primary, the expression for Full\_Name can also be written as `[Person].FirstName + [Person].LastName` . 

The Customer Source should now look like this: 

![Customer after creating the enriched column Full\_Name](../../.gitbook/assets/customer-full_name.jpg)

Now all we need to do is capture the Full\_Name attribute in the Order\_Detail Source. Below is the section of the ERD we need to examine.   

![Section of the ERD for Example 1 with added attributes](../../.gitbook/assets/customer_full_name%20%281%29.jpg)

Finally, create an enriched column on the Order\_Detail Source called _Customer\_Full\_Name_ with the Enrichment expression `[This]~{Order_Header-Order_Detail}~[Order_Header]~{Customer-Order_Header}~[Customer].FullName`If both the Order\_Header-Order\_Detail and the Customer-Order\_Header Relations are Primary, the above expression can also be written as `[Customer].FullName`.

This time, we need to traverse two Sources to access attributes in Customer. This is allowed because in the direction we are traversing, both Relations have the cardinality O.

The Order\_Detail Source now has the attributes that make it possible to track revenue by customer.

![The final Order\_Detail Source attributes](../../.gitbook/assets/order_detail-revenue-and-customer_full_name.jpg)

### Example 2: Customer Tenure

 In the next example, we'll see how to use Relations of cardinality M \(many\). We will use the ERD we started with in Example 1.

Another useful metric to track in this kinda of data set is _customer tenure,_ or "customer loyalty". Below is the portion of the ERD we need to examine for this example.

![ERD Section for Example 2](../../.gitbook/assets/customer-tenure-erd-section.jpg)

We will be storing all of the enriched columns in the Customer Source. First, we want to know the name of each customer. Create an enriched column in the Customer Source called Full\_Name that combines the FirstName and LastName fields in the Person Source, just like in Example 1.

Customer tenure can be rephrased as "the length of time that a customer has been making purchases at the store". In order to capture this, we need to take the   

