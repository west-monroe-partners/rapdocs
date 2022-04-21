# Creating a Multi Group Output

We will start off by taking a look at an example Output. It has a single Output Channel attached to Demo Company 1 Sales Order Detail.&#x20;

![A single Ouput Channel in the Multi Group Output](<../../../.gitbook/assets/image (386).png>)

Navigating back to the Output Settings page, notice in the below image that the Group and Output Template fields are both blank and the Output Name is hard coded to "Multi Group Output".&#x20;

![Blank Group and Template Names](<../../../.gitbook/assets/image (389).png>)

With this seup, the Output is not assicated with any Group. It exists separately from all of the Groups and is able to be connected to ANY Source in the environment. Think of this Output in a similar way to the Global Sources in the Cloning with Relations example. When a Source attached to this Output is cloned, it will create a new Channel to the EXACT same Output.

That is it for creating an Multi Group Output. Multi Group is treated as the default behavior when Cloning and requires very little additional configuration. See the next section for steps required to create a Single Group Output using Output Name Templates.
