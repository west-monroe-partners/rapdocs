# Creating A Single Group Output

The creation of a Single Group Output requires two Objects, a Group and an Output Name Template. Let's take a look at the Single Group Output from the example. In this example we will edit an already existing Output.

![The Company 1 Single Group Output](<../../../.gitbook/assets/image (418).png>)

Looking at the name of the Output, it is extremely similar in style to the name of the Source Company 1 Sales Order Detail. It contains a reference to the Group, "Company 1", and a reference to the Data it processes "Single Group Output" much in the same way that the Source does. Simlarly to the Source configuration, we will also use the Group object and a Name Template to set up this Output to be Single Group.

### The Group

For information on creating and using Groups, refer to [this page](../very-basic-cloning-example/groups.md).

### The Output Name Template

Similar to Source Name Templates, Output Name Templates allow users to setup IDO to dyanmically insert the GROUP name of an Output into its actual Output Name.

An Output Name Template is another extremely simple object. It has 1 configurable field: Output Name. That field has 1 requirement. It must have the text ${GROUP} somehwere in it. The ${GROUP} token will tell IDO where the user wants the Group name dynamically inserted into the Output name. AS stated above, the "Company 1 Single Group Output" Output name has two parts, the "Company 1" Group, and the "Single Group Output" that indicates what data is in the Output. Creating an Output Name Template for it would look like the image below.

![The Single Group Output Template](<../../../.gitbook/assets/image (400).png>)

### Applying the Group & Template

With a "Company 1" Group and a "Single Group Output" Template created, we are ready to transform the existing "Company 1 Single Group Output" into a true Single Group Output. As of righ now, it has no Group and no Output Template applied. As we learned in the Creating a Multi Group Output page, this means that it would behave as a Multi Group Output if Cloned.

![The Output before applying the Group and Template](<../../../.gitbook/assets/image (390).png>)

Apply the Group and Output Template by using the associated dropdowns. Once both are selected, the Output Name control will be greyed out and automatically populated by IDO, just like a Source using a Group and Template.&#x20;

![Group and Output Template applied](<../../../.gitbook/assets/image (398).png>)

The Output has now been converted into a Single Group Output! When the Company 1 Group is cloned, a brand new Output associated with the newly created Group will be made. In the next section, we will perform a clone of Company 1 to see how the Outputs behave.

## Notes

Note that an Output that has a Group MUST be in the same Group as all of its mapped Sources., or the mapped Sources must be ungrouped. In the event that a user maps a Source from a different Group into a Grouped Ouptput, IDO will throw an error and prevent the action.
