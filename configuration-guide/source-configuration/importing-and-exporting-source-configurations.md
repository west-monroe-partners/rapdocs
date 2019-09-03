# Importing and Exporting Configurations

Importing and Exporting Sources can be done from the Sources Screen. This allows the promotion or movement of configurations between different RAP environments.

## Exporting Configurations

### Selecting Sources

Click the kebab button \(**⋮**\) to bring up a Source's options. Each Source can be Exported by clicking **Export**.

![Source Drop-Down and Export Button](../../.gitbook/assets/image%20%28159%29.png)

Users can also export multiple Sources at once. To do this, first **Select Sources** to display selection options for each Source.

![Select Sources](../../.gitbook/assets/image%20%28173%29.png)

After selecting all desired Sources, choose the **Export** option in the **Select Action** drop-down and press **Submit.** This will bring users to the Source Export Parameters modal.

![Export Multiple Selected Sources](../../.gitbook/assets/image%20%28197%29.png)

### Source Export Parameters

The Source Export Parameters modal shows options for exporting Sources. Users can choose to Export dependent Sources and specify the RAP version. If re-importing the file into the same RAP instance, select the Latest RAP version.

![Source Export Parameters Modal](../../.gitbook/assets/image%20%28186%29.png)

## Importing Source Configurations

In the Source screen, the Import Sources button opens the Import Sources modal.

![Import Sources Button](../../.gitbook/assets/image%20%28183%29.png)

The Import Sources modal allows a user to browse for a Source file. The Source file has to match the running version of RAP. Choose the correct `.json` file to import.

![Import Sources Modal](../../.gitbook/assets/image%20%2868%29.png)

{% hint style="info" %}
Imported Sources attempt use the same Connection name as in their original environments. Be sure these Connections exist in the new environment **before** importing Sources.
{% endhint %}

Once a valid file is selected, a tab appears to show the Validation Results of the Source and any dependent objects in the file. Click **Import** to import the Source.

![Import Sources - Validation Results](../../.gitbook/assets/image%20%28108%29.png)

