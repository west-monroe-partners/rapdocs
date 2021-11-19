---
description: Lineage queries
---

# Lineage Queries

### Queries

select_ \*_ _ from meta.l\_get\_origin\_recursive(start\_node\_type, start\_object\_id) - returns recursive list of all origin nodes for the given start node, including path_

_select _ \* from meta.l\_get\_destination\_recursive(_start\_node\_type, start\_object\_id_) - _returns recursive list of all destination nodes for the given start node, including path_

__

_Each query/function returns hierarchical list of objects in parent-child format, along with path column that contains visual data flow path_

### _Node types_

| _Type_ | Description            | Object\_id                    |
| ------ | ---------------------- | ----------------------------- |
| R      | raw attribute          | _raw_\_attribute\_id          |
| S      | system                 | system\__attribute\__id       |
| E      | enrichment rule        | enrichment\_id                |
| EA     | enrichment aggregation | enrichment\__aggregation\__id |
| CM     | column mapping         | output\__source\__column\_id  |
| OC     | output column          | output\__column\__id          |

