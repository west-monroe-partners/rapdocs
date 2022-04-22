# Creating a Single Group Connection with Connection Name Templates

The creation of a Single Group Connection requires two Objects, a Group and a Connection Name Template. Let's take a look at the Single Group Connection from the example. In this example we will edit an already existing Connection called "Company 1 Single Group Connection".

![The Sinlge Group Connection Example](<../../../.gitbook/assets/image (377).png>)

Looking at the name of the Connection, it is extremely similar in style to the name of the Source Company 1 Sales Order Detail. It contains a reference to the Group, "Company 1", and a reference to the Data it connects to "Single Group Connection" much in the same way that the Source does. Simlarly to the Source configuration, we will also use the Group object and a Name Template to set up this Connection to be Single Group.

### The Group

For information on creating and using Groups, refer to [this page](../very-basic-cloning-example/groups.md).

### The Connection Name Template

Similar to Source Name Templates, Connection Name Templates allow users to setup IDO to dyanmically insert the GROUP name of a Connection into its actual Connection Name.

A Connection Name Template is another extremely simple object. It has 1 configurable field: Connection Name. That field has 1 requirement. It must have the text ${GROUP} somehwere in it. The ${GROUP} token will tell IDO where the user wants the Group name dynamically inserted into the Connection name. As stated above, the "Company 1 Single Group Connection" Connection name has two parts, the "Company 1" Group, and the "Single Group Connection" that indicates what the Connection connects to. Creating a Connection Name Template for it would look like the image below.

![The Single Group Connection Template](<../../../.gitbook/assets/image (420).png>)

### Applying the Group & Template

With a "Company 1" Group and a "Single Group Connection" Template created, we are ready to transform the existing "Company 1 Single Group Connection" into a true Single Group Connection. As of right now, it has no Group and no Connection Template applied. As we learned in the Creating a Multi Group Connection page, this means that it would behave as a Multi Group Connection if Cloned.

![The Singe Group Connection before applying the Group and Template](<../../../.gitbook/assets/image (405).png>)

Apply the Group and Connection Template by using the associated dropdowns. Once both are selected, the Connection Name control will be greyed out and automatically populated by IDO, just like a Source using a Group and Template.&#x20;

![The Group and Template have been applied](<../../../.gitbook/assets/image (402).png>)

The Connection has now been converted into a Single Group Connection! When the Company 1 Group is cloned, a brand new Connection associated with the newly created Group will be made if it does not exist already. In the next section, we will perform a clone of Company 1 to see how the Connections behave.

## Notes

* Note that a Connection that has a Group MUST be in the same Group as all of its Sources. In the event that a user maps a Source from a different Group into a Grouped Ouptput, IDO will throw an error and prevent the action.
* Note that while this example shows Connections attached to Sources, they are also used in Outputs. The behavior with Outputs is the same as Sources, with new Connections created on Clone for Single Group Connections, and the same Connection being used for Multi Group Connections.

