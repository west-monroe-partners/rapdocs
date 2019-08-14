# Naming Conventions

There are many parts of RAP that allow a user to input custom names. It is important to balance many considerations when it comes to naming objects, such as searchability, sortability, semantic understanding, clarity, and brevity. 

Finding a balance is an important part of every RAP implementation. Setting conventions early and sticking with them will allow the code to be more readable and maintainable over time.

## Connections

For connection types that connect to file sources, it is important to note that file Connections can be used as both an Input and Output path. It is important to note the difference.

### Connection Type: Table

**Naming Convention:**

```text
Database Type - Database Name
```

* Example 1: `SQL Server - Divvy`

### Connection Type: SFTP & File

**Naming Convention:**

```text
Data Source Name - Path Type
```

* Example 1: `Divvy - Input Path`
* Example 2: `Divvy - Output Path`

## Sources

### Input Type - File Push & File Pull

**Naming Convention:**

```text
Source Domain - Source System : Semantic File Name [Tag]
```

* Example 1: `CMS - Divvy : Trips 2017 Q1 [Finance]`

### Input Type - Table Pull

**Naming Convention:**

```text
Source Domain – Source System : Semantic Table Name [Tag]
```

* Example 1: `CMS - Divvy : Trips [Finance]`

## Validation Rules

Validation Rules require the loosest rules for naming conventions because each Validation Rule has a specific business purpose and can be complex.

## Enrichment Rules

There are a handful different kinds of Enrichment Rules. Each has a different purpose and it should be noted in the Enrichment Rule name.

### Enrichment Type:

* **Active**: A column that is exposed to the end-database.
* **Work**: A column that is not exposed to the end-database, but is used as a calculation intermediate step.

### Operation / Data Type:

* **Lookup**: A column that takes data from a different Source
* **Numeric**: Datatype conversion to numeric
* **Timestamp**: Datatype conversion to datetime
* **Flag**: A simple one-character column that helps with reporting

**Naming Convention:**

```text
Source Domain - Enrichment State : Semantic Column Name [Operation / Data Type]
```

* Example 1: `CMS – Work : New Rider Flag [Flag]`
* Example 2: `CMS - Active : Recent Sale [Timestamp, Lookup]`

### Enrichment Templates

When creating Enrichment Templates, it is best practices to prefix them with the Data Source name.

**Naming Convention:**

```text
Source Domain - Semantic Column Name : [Operation / Data Type]
```

* Example 1: `CMS - New Rider Flag : [Flag]` 
* Example 2: `CMS - Recent Sale : [Timestamp, Lookup]`

## Outputs

### Output Type - File & SFTP

**Naming Convention:**

```text
System - Semantic File Name
```

* Example 1: `Divvy - Trips 2017 Q1`

### Output Type - Table & Virtual

**Naming Convention:**

```text
System - Semantic Table/View Name
```

* Example 1: `Divvy - Trips`

#### Loopbacks

When creating an Output for a Loopback, the Source for the next step of the Loopback should be named the same. 

* Example 1: If the Output for Step 1 of the Loopback is `Divvy - Trips`, then the Source for Step 2 of the Loopback should be named `Divvy - Trips`

