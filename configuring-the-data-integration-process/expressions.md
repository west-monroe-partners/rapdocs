---
description: >-
  Details related to the Expressions that can be used in the Intellio DataOps
  (RAP) user interface.
---

# !! Expressions

Expressions occur in many locations in the Intellio DataOps \(RAP\) user interface, such as Relations, Rules, ...

Expressions within Intellio DataOps \(RAP\) follows [Spark SQL](https://spark.apache.org/docs/latest/sql-programming-guide.html). Spark SQL is negligibly different from basic SQL, so a proficiency in one typically implies a proficiency in the other.

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Expression</b>
      </th>
      <th style="text-align:left"><b>Description</b>
      </th>
      <th style="text-align:left">Usage Locations</th>
      <th style="text-align:left"><b>Examples</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">[<em>Source Name</em>]</td>
      <td style="text-align:left">Source container</td>
      <td style="text-align:left">Output Mappings</td>
      <td style="text-align:left">[SalesOrderDetail]</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>[This]</em>
      </td>
      <td style="text-align:left">
        <p>Current source container. Equivalent to [<em>current source name</em>]</p>
        <p>Optional for Output Mapping &amp; Dataviewer expressions</p>
      </td>
      <td style="text-align:left">All</td>
      <td style="text-align:left">[This]</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>[Related]</em>
      </td>
      <td style="text-align:left">Container for related source. Only allowed in relation expression.</td>
      <td
      style="text-align:left">Relations</td>
        <td style="text-align:left">[Related]</td>
    </tr>
    <tr>
      <td style="text-align:left">{<em>Relation Name</em>}</td>
      <td style="text-align:left">Relation name, indicates path to the source containers used in expression.
        Preceded by <em>[This]~</em> to access non-primary relations</td>
      <td style="text-align:left">Rules, Output Mappings</td>
      <td style="text-align:left">[This]~{Non Primary Relation Name}.attribute_name</td>
    </tr>
    <tr>
      <td style="text-align:left">.</td>
      <td style="text-align:left">Separator of source containers and attribute names. Can also be used to
        directly display the attribute drop down of the current source in Dataviewer
        and Output Mapping expressions.</td>
      <td style="text-align:left">All</td>
      <td style="text-align:left">[Source Name].attribute_name</td>
    </tr>
    <tr>
      <td style="text-align:left">~</td>
      <td style="text-align:left">Path indicator, separates source containers and Relations. Used after <em>[This]</em> to
        access non-primary relations.</td>
      <td style="text-align:left">Rules, Output Mappings</td>
      <td style="text-align:left">[This]~{Relation Name}~[Related Source Name].attribute_name</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
Across the Intellio DataOps \(RAP\) platform, a grey \(un-clickable\) "Save" button indicates there is an error. Typically this error is within an expression field. Double check errors and expressions if you are unable to "Save" your work.
{% endhint %}





