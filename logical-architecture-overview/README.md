---
description: A deeper look of how RAP is organized and processes data.
---

# !! Logical Architecture

As RAP operates differently than traditional ETL tools, this section provides a deeper dive of how RAP processes data, how data flows through RAP, and how RAP is logically organized.  The information presented in this section is helpful to know when architecting a new implementation leveraging RAP as the data processing tool of choice.

!! Intellio DataOps \(RAP\) is a modifications of SQL clauses. In some ways RAP can be thought of as a front end rapper for SQL, as each change in the interface modifies the ultimate SQL that is executed. Each step in the logical architecture modifies a different portion of the overall SQL code that executes from ingest through output.

TODO - ensure this section does not overlap too much with "RAP Basics - How it Works" section

TODO - identify high-level diagram

### Audience

The intended audiences for this section are the following:

* Data architects who have experience with traditional ETL tools and solutions and are seeking to understand how RAP works in anticipation of needing to lead a new RAP implementation.
* Cloud and application architects who are looking to understand how data moves through the RAP engine.
* Experienced RAP configurators who are looking to understand more deeply how RAP works \(the how and why behind the configuration\).
* Developers looking to gain a deeper understanding of what RAP does in anticipation of needing to work on RAP core processing code.

