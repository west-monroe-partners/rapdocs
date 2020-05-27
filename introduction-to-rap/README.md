---
description: A deeper look on how RAP operates internally and stores its metadata.
---

# RAP Technical Overview

As RAP operates differently than traditional ETL tools, this section provides a deeper dive of how RAP processes data, how AWS drives data flow in RAP, and how RAP is structured internally.  The information presented in this section is helpful to know when architecting a new implementation leveraging RAP as the data processing tool of choice, as well as where to look when troubleshooting issues.

TODO - add logical arch section

### Audience

The intended audiences for this section are the following:

* Cloud architects looking to understand which cloud resources RAP requires for its operation.
* Data architects who have experience with traditional ETL tools and solutions and are seeking to understand how RAP works in anticipation of needing to lead a new RAP implementation.
* Experienced RAP configurators who are looking to understand more deeply how RAP works and more efficient ways configure RAP sources by leveraging RAP's internal metadata structures.
* Developers looking to gain a deeper understanding of what RAP does in anticipation of needing to work on RAP core processing code.



