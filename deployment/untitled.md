# Import/Export

## Intro <a id="intro"></a>

In Intellio Data Ops, import/export functionality allows the user to copy sources and outputs, in their entirety, from one environment to another, without having to do any manual reconfiguration in the importing environment. There are two basic ways of exporting, top down from the source level, or bottom up from the output level.

### Prerequisite <a id="prerequisite"></a>

Any connection\(s\) used in an exported entity must have an equivalent connection with an IDENTICAL name in the importing environment.

## Exporting <a id="exporting"></a>

### Initiating an Output level Export <a id="initiating-an-output-level-export"></a>

1. Navigate to the output mapping tab of the output that has mappings that the user would like to export.
2. In the second column of the table, click the checkbox of each source mapping that the user wants to export, or click on the top checkbox in the header row to select all source mappings.

![](https://gblobscdn.gitbook.com/assets%2F-LhufZT729fit8K2vT1H%2F-MG4ZWMba75CH6Lqq51A%2F-MG4vpxJOk44VaqD8e1w%2Fimage.png?alt=media&token=2d9ac9be-c8d1-4d86-8d4c-d85e7cf69f4e)

3. Click the 'Export' button:

![](https://gblobscdn.gitbook.com/assets%2F-LhufZT729fit8K2vT1H%2F-MG4ZWMba75CH6Lqq51A%2F-MG4xXtNdqkG_7AHC7MW%2Fimage.png?alt=media&token=ff1b1927-6a70-4e64-adc4-41de9d61672e)

4. Review all the objects in the export modal that appears, in order to make sure all entities shown in the modal were intended to be included in the export, and make sure all entities that are supposed to be imported are included. \(Rules as to what entities are supposed to be included in an output level export can be found below\).

5. Press the 'Proceed' button at the bottom right of the modal, and the user should be prompted to save the export as a .yaml file

### Initiating a Source Level Export <a id="initiating-a-source-level-export"></a>

1. Navigate to the main sources page.

2. Check the checkbox on the far right of the table of each source the user wants to be exported.

3. In the second column of the table, click the checkbox of each source mapping that the user wants to export, or click on the top checkbox in the header row to select all sources.

4. Click the 'Select Action' drop down above the table and to the left of the New Source button

![](https://gblobscdn.gitbook.com/assets%2F-LhufZT729fit8K2vT1H%2F-MG5HA1Cht45OUleIMUK%2F-MG5IGpldTxJQs1mvyRH%2Fimage.png?alt=media&token=a894014b-3341-44e6-bf19-c31aa09f0028)

5. Click the 'Export' button.

6. Review all the objects in the export modal that appears, in order to make sure all objects shown in the modal were intended to be included in the export, and make sure all objects that are supposed to be imported are included. \(Rules as to what objects are supposed to be included in a source level export can be found below\).‌

7. Press the 'Proceed' button at the bottom right of the modal, and the user should be prompted to save the export as a .yaml file

## Importing <a id="importing"></a>

​

