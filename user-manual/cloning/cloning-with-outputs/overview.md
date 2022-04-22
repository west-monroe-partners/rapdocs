# Overview

## Summary and Use Case

When configuring Output Channels in IDO, two use cases commonly arise. We will refer to them as  "Multi Group Outputs" and "Single Group Outputs"

### Multi Group Outputs

A Multi Group Output contains mappings from Sources in a number of different IDO Groups. To build on the earlier Company 1 example, imagine a scenario where Company 1 and Company 2 Groups exist in an environment. Each Group has a Sales Order Detail Source and the user wants to write all of Sales data into a single Output table. The setup looks something like the below image.

![Both Sources are mapped into the same Output](<../../../.gitbook/assets/image (402).png>)

In the event that the user wants to perform a Clone to create "Company 3", the desired behavior would be to add an additional Channel into the existing Output A for the Company 3 Sasles Order Detail Source to write data into, similar to the image below.

![The Company 3 Source has been mapped into the same Output](<../../../.gitbook/assets/image (419).png>)

### Single Group Outputs

A Single Group Output contains mappings from Sources that are all in the same Group (or just one Source). Again imagine a scenario where Company 1 and Company 2 Groups exist in an environment. The user wants to write Sales data to a SEPARATE table for each Source, similar to the image below.

![The Sources each write to separate Outputs](<../../../.gitbook/assets/image (411).png>)

In the event that the user performs a Clone to create Company 3, the desired behavior is to create an entirely new Output with the Company 3 Source(s) mapped into it, similar to the image below.

![The newly created Company 3 Source has its own Output](<../../../.gitbook/assets/image (379).png>)

## The Example

In the upcoming sections, we will explore how Output Name Templates can be utilized to control the whether an Output and its Output Channels are treated as Single Group or Multi Group. For this example we will expand upon the Company 1 example introduced in the Cloning Overview by adding a couple of outputs seen in the diagram below.

![Our starting example with 2 Outputs](<../../../.gitbook/assets/image (397) (1).png>)

We will configure the Outputs so that after Cloning, the environment looks like the image below. Notice the newly created Output Cloning Single Group Output and the new Channel in the Multi Group Output.

![Desired Clone Output](<../../../.gitbook/assets/image (405).png>)



