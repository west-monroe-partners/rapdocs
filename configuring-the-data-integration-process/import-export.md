# Import/Export

## Intro

In Intellio Data Ops, import/export functionality allows the user to copy sources and outputs, in their entirety, from one environment to another, without having to do any manual reconfiguration in the importing environment. There are two basic ways of exporting, top down from the source level, or top down from the output level.

### Prerequisite

Any connection\(s\) used in an exported entity must have an equivalent connection with an IDENTICAL name in the importing environment.

## Exporting

## Initiating an Output level Export

1. Navigate to the output mapping tab, of the output that has mappings that you would like to export.
2. In the second column of the table, click the checkbox of each source mapping that you want to export, or click on the top checkbox in the header row to select all 

![](../.gitbook/assets/image%20%28253%29.png)

3. Click the 'Export' button:

![](../.gitbook/assets/image%20%28254%29.png)

4. Review all the objects in the export modal that appears, in order to make sure all entities shown in the modal were intended to be included in the export, and make sure all entities that are supposed to be imported are included.



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

