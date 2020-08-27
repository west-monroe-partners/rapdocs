# Import/Export

## Intro

In Intellio Data Ops, import/export functionality allows the user to copy sources and outputs, in their entirety, from one environment to another, without having to do any manual reconfiguration in the importing environment. There are two basic ways of exporting, top down from the source level, or top down from the output level.

### Prerequisite

Any connections used in an exported entity must have an equivalent connection with an IDENTICAL name in the importing environment.

| **Output level exports should include:** |
| :--- |
| 1. All selected outputs definitions and their mappings |
| 2. All sources mapped to selected outputs mappings, with all their enrichments. |
| 3. All relations used in those output mappings/enrichments of the sources in previous step. |
| 4. All sources used in the relations from the previous step. |
| 5. Repeat steps 3,4,5 until all necessary sources are selected. |

| Source Level Exports Should Include |
| :--- |
| 1. All selected sources, including all enrichments. |
| 2. All outputs mappings selected sources are mapped to. |
| 3. All relations used in those output mappings/enrichments of the sources in previous step. |
| 4. All sources used in the relations from the previous step. |
| 5. Repeat steps 3,4,5 until all necessary sources are selected |

