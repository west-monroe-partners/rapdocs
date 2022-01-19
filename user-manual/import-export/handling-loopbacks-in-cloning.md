# Handling Loopbacks in Cloning

## Current Logic

In the current state of Import/Export in IDO, any Output Channel that is cloned will create an additional Output Channel in the same Output as the existing Output Channel. This is beneficial for Outputs that are configed in the One Big Table model, with multiple groups of Sources feeding into one large reporting Output.

![Cloning works well for an OBT model](<../../.gitbook/assets/image (383) (1).png>)

## Loopback Problems

This logic is unfortunately not the desired logic for most Loopbacks. The desired logic, seen below, requires that each Output Channel creates an entirely separate Output. Additionally each of these separated Outputs should feed into its own Loopback Source.

![Desired Loopback Cloning behavior](<../../.gitbook/assets/image (384).png>)

With the current logic,  cloning a Loopback results in a setup like the one below. It has two Output Channels within a single Output and both Loopback Sources are pulling data from that Output.&#x20;

![Actual Loopback Cloning behavior](<../../.gitbook/assets/image (382) (1).png>)

## Workaround

The best workaround for this issue is to edit the .YAML file that is exported. Two edits will need to be made for each Loopback Source

1. Editing the "output\_name" field associated with the Ouptut Channel upstream from the Loopback Source
2. Editing the "loopback\_output\_name" field for the Loopback Source to match the name in step 1

#### Steps

1. Export the desired clone Group.
2. Open the exported .YAML file.
3. CTRL+F _loopback\_output\_name_ and find the first result. (i.e. Loopback Output Group A)
4. CTRL+F the document for the Output name found in the step above.
5. Replace the Output name with a new one that leverages the desired new Group name (i.e. Loopback Output Group A becomes Loopback Output Group B)
6. Repeat steps 4 & 5 until all instances of the old Output name have been replaced.
7. Repeat steps 3 through 6 until all Loopback Outputs have been addressed.
8. Import the .YAML

The same steps can be followed for any Output that should create an entirely separate Output during the clone process rather than a new Output Channel within the same Output. Simply search for output\_name and replace any Output name that should be created as a net new Ouptut
