# !! Relations and Rules Detailed Example

## Relations and Rules Examples

To illustrate the proper use of Relations and Rules, let's examine the example Entity-Relationship Diagram of an arrangement of Sources in DataOps below. We will use this ERD for both examples. The labels near the relationship lines are the names of the Relations that the user would have configured prior to creating any Enrichments. 

![Example ERD](../../.gitbook/assets/relations-erd%20%284%29.jpg)

#### 

### Example 1: Revenue By Customer

For this example, we need to focus on a particular section of the ERD shown below.

![ERD Section for Example 1](../../.gitbook/assets/revenue-by-customer-erd-section.jpg)

One useful metric to track in these kinds of data models is _revenue by customer._ To do this, let's first create an enriched column _Revenue_ on the Order\_Detail Source_._ The Enrichment expression for this would be `[This].OrderQty * [This].UnitPrice`. All of the attributes needed for this enriched column already exist on the Order\_Detail Source, so we don't need any Relations for this metric.

The Order\_Detail Source should now look like this:

![Order\_Detail after creating the enriched column Revenue](../../.gitbook/assets/order_detail-revenue.jpg)

The last part of capturing this metric is to retrieve the full name of the customer. Breaking this last step into two parts makes this task slightly easier. Since the cardinality of the Customer-Person Relation is O \(one\), it should be simple to store the full name of the customer in the Customer Source. Let's create an enriched column on the Customer Source called _Full\_Name_ with the Enrichment expression `[This]~{Customer-Person}~[Person].FirstName + [This]~{Customer-Person}~[Person].LastName` . 

Note the special syntax when using Relations in the expression. Relations can make the Enrichment expression quite long, but marking a Relation as the Primary Relation makes referencing it much easier. If the Customer-Person Relation is Primary, the expression for Full\_Name can also be written as `[Person].FirstName + [Person].LastName` . 

The Customer Source should now look like this: 

![Customer after creating the enriched column Full\_Name](../../.gitbook/assets/customer-full_name.jpg)

Now all we need to do is capture the Full\_Name attribute in the Order\_Detail Source. Below is the section of the ERD we now need to examine.   

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

Customer tenure can be rephrased as "the amount of time that a customer has been making purchases at the store". In order to capture this, we need to take difference between the most recent date and the earliest date that a customer has made a purchase.

To capture the most recent purchase date, create a new enriched column on the Customer Source called _Most\_Recent\_Order\_Date_ with the Enrichment expression `MAX([This]~{Customer-Order_Header}~[Order_Header].OrderDate)` \(or `[Order_Header].OrderDate` if Customer-Order\_Header is Primary\).

Notice the `MAX()` function surrounding the expression. This is an example of an aggregate function. We need to use an aggregate function in this case because the cardinality of the Relation Customer-Order\_Header is M, and has the potential to return more than one record as indicated by the one-to-many relationship from Customer to Order\_Header. The Enrichment expression cannot return multiple values, so the aggregate function consolidates these into one returnable value. 

{% hint style="info" %}
RAP uses Spark SQL for aggregate functions. Click [here](https://jaceklaskowski.gitbooks.io/mastering-spark-sql/spark-sql-aggregate-functions.html) for a complete list of Spark aggregate functions. 
{% endhint %}

For the earliest purchase date, create another enriched column called _Oldest\_Order\_Date_ with the same expression, but using the `MAX()` aggregate function instead. The Customer Source should now look like this:

![Customer Source with added attributes](../../.gitbook/assets/customer-most-recent-and-oldest.jpg)

Finally, in order to capture customer tenure, create an enriched column that takes the difference between the most recent order date and the earliest order date. These fields are in a date format, so we'll need to use a specific aggregate function. The Enrichment expression is `DATEDIFF([This].Earliest_Order_Date, [This].Most_Recent_Order_Date)` .

