# Import/Export

## Intro

In Intellio Data Ops, import/export functionality allows the user to copy sources and outputs, in their entirety, from one environment to another, without having to do any manual reconfiguration in the importing environment. There are two basic ways of exporting, top down from the source level, or top down from the output level.

### Prerequisite

Any connection\(s\) used in an exported entity must have an equivalent connection with an IDENTICAL name in the importing environment.

## Exporting

### Initiating an Output level Export

1. Navigate to the output mapping tab of the output that has mappings that the user would like to export.
2. In the second column of the table, click the checkbox of each source mapping that the user wants to export, or click on the top checkbox in the header row to select all source mappings.

![](../.gitbook/assets/image%20%28254%29.png)

3. Click the 'Export' button:

![](../.gitbook/assets/image%20%28255%29.png)

4. Review all the objects in the export modal that appears, in order to make sure all entities shown in the modal were intended to be included in the export, and make sure all entities that are supposed to be imported are included. \(Rules as to what entities are supposed to be included in an output level export can be found below\).

5. Press the 'Proceed' button at the bottom right of the modal, and the user should be prompted to save the export as a .yaml file

| **Output level exports should include:** |
| :--- |
| 1. All selected outputs definitions and their mappings |
| 2. All sources mapped to selected outputs mappings, with all their enrichments. |
| 3. All relations used in those output mappings/enrichments of the sources in previous step. |
| 4. All sources used in the relations from the previous step. |
| 5. Repeat steps 3,4,5 until all necessary sources are selected. |

### Initiating a Source Level Export

1. Navigate to the main sources page.

2. Check the checkbox on the far right of the table of each source the user wants to be exported.

3. In the second column of the table, click the checkbox of each source mapping that the user wants to export, or click on the top checkbox in the header row to select all sources.

4. Click the 'Select Action' drop down above the table and to the left of the New Source button

![](../.gitbook/assets/image%20%28252%29.png)

5. Click the 'Export' button.

6. Review all the objects in the export modal that appears, in order to make sure all objects shown in the modal were intended to be included in the export, and make sure all objects that are supposed to be imported are included. \(Rules as to what objects are supposed to be included in a source level export can be found below\).‌

7. Press the 'Proceed' button at the bottom right of the modal, and the user should be prompted to save the export as a .yaml file

| Source Level Exports Should Include |
| :--- |
| 1. All selected sources, including all enrichments. |
| 2. All outputs mappings selected sources are mapped to. |
| 3. All relations used in those output mappings/enrichments of the sources in previous step. |
| 4. All sources used in the relations from the previous step. |
| 5. Repeat steps 3,4,5 until all necessary sources are selected |

