---
description: Released 2/22/2021
---

# 2.3



### New features

|   | Templates |
| :--- | :--- |
| **What** | Templates provide mechanism for centralized management of  sources with similar processing logic.  |
| **Why** | Templates enable reusable pattern for quick data integration from multiple "parallel" source systems \(ERP, EHR, etc.\) |
| **Impact** | Templates introduce new way to create and manage Relations and Rules \(Enrichment and Validation\). Relations and Rules created from the Template are centrally managed using Template management screens |
| **Details** | [Templates and Tokens](../../configuring-the-data-integration-process/validation-and-enrichment-rule-templates/) |

|   | Move Agent to Connection |
| :--- | :--- |
| **What** | Connections have new direction attribute: Source or Output. Same connection can no longer be used on both. Source connection has optional Agent attribute \(relocated from Source Settings\) . When no Agent is specified for the Connection, it is using direct Spark ingestion. |
| **Why** | Convenience: users are no longer required to specify Agent for every new Source. This also allows to better manage dev/prod agents: Agent attribute stays with the Connection and no longer  exported/imported. This also enabled us to restrict access from Agents to sensitive, encrypted Connection data. Now only agent specified on the Connection will have access.   |
| **Impact** | Agent ver. 2.2+ will automatically update to 2.3. If you're upgrading from 2.1.x or prior version, upgrade to ver. 2.2 first before upgrading to 2.3, otherwise all deployed agents would need to be manually redeployed |
| **Details** | [2.3.0](2.3.0.md) |

|  | Fixed Source Data Deletion |
| :--- | :--- |
| **What** | Separate 'Delete All Source Data' button on the Source inputs tab into two distinct buttons: 'Delete Source Data' and 'Delete Source Metadata'. 'Delete Source Data' deletes inputs, input history, processes in workflow queue, and drops the hub tables. 'Delete Source Metadata' deletes raw attributes, and drops hub tables.  |
| **Why** | Old 'Delete All Source Data' button did not clear source raw attributes, which forced users to recreate sources from scratch if a problem arose in the source's raw attributes. |
| **Impact** | Allows users to reset sources to their initial state, and clear broken raw attributes on the source. This button will also produce a list of any dependent rules, relations, or output mappings that are preventing metadata deletion. |
| **Details** | [2.3.0](2.3.0.md) |

|  | Changed Default Value of the 'Force Case Insensitive' Source Parameter |
| :--- | :--- |
| **What** | Newly configured sources will now default to converting all ingested column names to lowercase |
| **Why** | Greater attribute name standardization |
| **Impact** | This will affect all newly configured sources. So in order to prevent this lowercasing, users will need to manually toggle this source parameter before the initial ingestion of a source. |
| **Details** | [2.3.0](2.3.0.md) |

