# Overview

## Summary and Use Case

When configuring Source and Ouptputs in IDO, users must configure each Object to use a Connection. These Connections define the credentials and location of the Source/Output data. In an environment that uses Cloning to create multiple copies of Source & Output with the same logic, two patterns of Connection management arise, Multi Group Connections and Single Group Connections.

#### Multi Group Connections

Multi Group Connections are useful when Cloning Groups that all pull from the same Database/File Path. When a Source or Output attached to a Multi Group Connection is Cloned, the new versions will be attached to the existing Connection, similar to the image below.&#x20;

![Multi Group Connection behavior](<../../../.gitbook/assets/image (406).png>)

#### Single Group Connections

Single Group Connections are useful when Cloning Groups that have a distinct Database/File Path. When a Source attached to a Single Group Connection is Cloned, the new versions will be attached to a separate Connection from the original Sources/Outputs, one belonging specifically to the new Group, similar to the image below.

![A Single Group Connection Clone](<../../../.gitbook/assets/image (394).png>)

## The Example

In the upcoming sections, we will explore how Connection Name Templates can be utilized to control the whether a Connection is treated as Single Group or Multi Group when Cloning. For this example we will expand upon the Company 1 example introduced in the Cloning Overview by adding a couple of Connections seen in the diagram below.

![Connections of each type exist](<../../../.gitbook/assets/image (386).png>)

The Connections will be configued so that the resulting configuration after Cloning looks like the image below. Notice that the Multi Group Connection is now connected to Sources in both Groups, while the Single Group Connections are independent and attached to only one Group.

![The Configuration Post Clone](<../../../.gitbook/assets/image (381).png>)

