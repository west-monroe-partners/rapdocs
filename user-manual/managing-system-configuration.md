# Managing System Configuration

Last Update: 6/8/2021 04:30 EST

## 1.0 Objective

System configuration metadata is available within the ```meta.system_configuration``` table in ```reportingprod```. The objective of the  table is to collect global configuration settings, providing key database, environmental and application information.

## 1.1 Table Schema

| Columns     | Purpose                                | Example(s)                          |
| ----------- | -------------------------------------- | ----------------------------------- |
| name        | Name of the configuration              | 'password', 'timeout', 'alert'      |
| description | What, where & why of the configuration | 'The cost of running a single node' |
| value       | Measurable attribute                   | '20 days', 'Azure', 'AWS', '.052'   |
| application | Which service(s) it belongs to         | '["core"], ["api", "sparky"]'       |
| datatype    | Type of data of the value              | text, int, numeric, json            |

