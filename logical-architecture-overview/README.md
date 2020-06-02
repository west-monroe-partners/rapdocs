# Logical Architecture

As RAP operates differently than traditional ETL tools, this section provides a deeper dive of how RAP processes data, how AWS drives data flow in RAP, and how RAP is structured internally.  The information presented in this section is helpful to know when architecting a new implementation leveraging RAP as the data processing tool of choice.

TODO - does this overlap with "RAP basics - How it Works" section?

### Audience

The intended audiences for this section are the following:

* Data architects who have experience with traditional ETL tools and solutions and are seeking to understand how RAP works in anticipation of needing to lead a new RAP implementation.
* Cloud and application architects who are looking to understand how data moves through the RAP engine.
* Experienced RAP configurators who are looking to understand more deeply how RAP works \(the how and why behind the configuration\).
* Developers looking to gain a deeper understanding of what RAP does in anticipation of needing to work on RAP core processing code.

