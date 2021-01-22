---
description: >-
  Details related to the Expressions that can be used in the Intellio® DataOps
  (RAP) user interface.
---

# Intellio® QL

Expressions occur in many locations in the Intellio® DataOps \(RAP\) user interface, namely Relations, Rules, Output Mappings, and Data Viewer filters. In order to access source attributes and traverse relations within these expressions, the user must use **Intellio® Query Language**.

Expressions within Intellio® DataOps \(RAP\) follow [Spark SQL](https://spark.apache.org/docs/latest/sql-programming-guide.html). Spark SQL is negligibly different from basic SQL, so a proficiency in one typically implies a proficiency in the other.

## Syntax

{% tabs %}
{% tab title="Source Containers" %}
* **\[**_**Source Name**_**\]** -- Source container. Simply a source name wrapped in brackets.

  * **Usage Locations:** Output Mappings, Rules
  * **Usage Examples:** \[SalesOrderDetail\].OrderQuantity

* _**\[This\]** --_ Current source container. Equivalent to \[_current source name_\]. Optional for Output Mapping & Data Viewer expressions

  * **Usage Locations:** Anywhere Intellio QL is used
  * **Usage Examples:** \[This\].OrderQuantity

* _**\[Related\]** --_ Container for related source. Only allowed in relation expression.
  * **Usage Locations:** Container for related source. Only allowed in relation expression.
  * **Usage Example:** \[This\].ID = \[Related\].ID
{% endtab %}

{% tab title="Relation Containers" %}
* **{**_**Relation Name**_**}** _--_ Relation name, indicates path to the source containers used in expression. Preceded by _\[Source Container\]~_ to access non-primary relations
  * **Usage Locations:** Rules, Output Mappings
  * **Usage Example:** \[This\]~**{Non Primary Relation Name}**.attribute\_name
{% endtab %}

{% tab title="Seperators" %}
* _**Period \( . \)**_ -- Separator of source containers and attribute names. Can also be used to directly display the attribute drop down of the current source in Data Viewer and Output Mapping expressions.
  * **Usage Locations:** Anywhere Intellio QL is used
  * **Usage Examples:** \[Source Name\].attribute\_name
* _**Tilde \(~\)**_ -- Path indicator, separates source containers and Relations. Used after a source container to access non-primary relations.
  * **Usage Locations:** Rules, Output Mappings
  * **Usage Example:** \[This\]~{Relation Name}~\[Related Source Name\].attribute\_name
{% endtab %}
{% endtabs %}

### _**Source Containers**_

* \[_Source Name_\] -- Source container. Simply a source name wrapped in brackets.

  * **Usage Locations:** Output Mappings, Rules
  * **Usage Examples:** \[SalesOrderDetail\].OrderQuantity

* _\[This\] --_ Current source container. Equivalent to \[_current source name_\]. Optional for Output Mapping & Data Viewer expressions

  * **Usage Locations:** Anywhere Intellio QL is used
  * **Usage Examples:** \[This\].OrderQuantity

* _\[Related\] ****--_ Container for related source. Only allowed in relation expression.
  * **Usage Locations:** Container for related source. Only allowed in relation expression.
  * **Usage Example:** \[This\].ID = \[Related\].ID

### _**Relation Containers**_

* {_Relation Name_} ****_--_ Relation name, indicates path to the source containers used in expression. Preceded by _\[Source Container\]~_ to access non-primary relations
  * **Usage Locations:** Rules, Output Mappings
  * **Usage Example:** \[This\]~{Non Primary Relation Name}.attribute\_name

### _**Separators**_

* _Period \( . \)_ -- Separator of source containers and attribute names. Can also be used to directly display the attribute drop down of the current source in Data Viewer and Output Mapping expressions.
  * **Usage Locations:** Anywhere Intellio QL is used
  * **Usage Examples:** \[Source Name\].attribute\_name
* _Tilde \(~\)_ -- Path indicator, separates source containers and Relations. Used after a source container to access non-primary relations.
  * **Usage Locations:** Rules, Output Mappings
  * **Usage Example:** \[This\]~{Relation Name}~\[Related Source Name\].attribute\_name

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
      <td style="text-align:left">Source container. Simply a source name wrapped in brackets.</td>
      <td style="text-align:left">Output Mappings, Rules</td>
      <td style="text-align:left">[SalesOrderDetail]</td>
    </tr>
    <tr>
      <td style="text-align:left"><em>[This]</em>
      </td>
      <td style="text-align:left">
        <p>Current source container. Equivalent to [<em>current source name</em>]</p>
        <p>Optional for Output Mapping &amp; Data Viewer expressions</p>
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
        directly display the attribute drop down of the current source in Data
        Viewer and Output Mapping expressions.</td>
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

## Auto Complete Trigger Keys

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Key</b>
      </th>
      <th style="text-align:left"><b>Preceding token</b>
      </th>
      <th style="text-align:left"><b>Drop-down values</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">.</td>
      <td style="text-align:left">
        <p>[This]</p>
        <p>[Related]</p>
        <p>Source container</p>
      </td>
      <td style="text-align:left">All source attributes (raw, enriched, system) + {source non-primary relations}</td>
    </tr>
    <tr>
      <td style="text-align:left">[</td>
      <td style="text-align:left">White space or start of line</td>
      <td style="text-align:left">[This] + all related sources (directly and pass-through via primary relations)</td>
    </tr>
    <tr>
      <td style="text-align:left">`</td>
      <td style="text-align:left">White space or start of line</td>
      <td style="text-align:left">Spark SQL functions</td>
    </tr>
    <tr>
      <td style="text-align:left">~</td>
      <td style="text-align:left">Source Container</td>
      <td style="text-align:left">All Relations by relation name instead of related source name.</td>
    </tr>
  </tbody>
</table>

## Example Expressions

### Relation Expressions

More Info on relation expressions and examples can be found at the bottom of the relation page of the configuration guide [here](https://app.gitbook.com/@intellio/s/dataops/v/master/configuring-the-data-integration-process/source-configuration/relations-1#relation-expressions).

### Rule Expressions

More Info on rule expressions and examples can be found at the bottom of the rules page of the configuration guide [here](https://app.gitbook.com/@intellio/s/dataops/v/master/configuring-the-data-integration-process/source-configuration/enrichment-rule-configuration#example-expressions).

### Output Mapping Expressions

More Info on output mapping expressions and examples can be found at the bottom of the output mapping page of the configuration guide [here](https://app.gitbook.com/@intellio/s/dataops/v/master/configuring-the-data-integration-process/output-configuration/output-mapping#mapping-expressions). 

