# Performing the Clone

As a reminder, this is the current setup of the environment.&#x20;

![The example setup](<../../../.gitbook/assets/image (418).png>)



## Performing the Clone

We will clone the Company 1 Group and create a new Group called "Connection Cloning". To perform the clone, follow the steps described [here](../very-basic-cloning-example/performing-a-basic-clone.md), this time updating the Group name to "Connection Cloning". Be sure to validate all of the screen to ensure no unexpected objects are exported or cloned!

## The Results

Two new Sources have been created in the environment, "Connection Cloning Sales Order Detail" and "Connection Cloning Sales Order Header"

![The newly created Sources](<../../../.gitbook/assets/image (407).png>)

#### The Multi Group Connection

First let's take a look at the Multi Group Connection that already existed in the environment before the Clone. Navigating to that Connection, the Applied Objects tab shows that the Sales Order Header Sources from both Groups are attached to the Connection. The Multi Group logic has been successful!

![The Connection is applied to both Groups](<../../../.gitbook/assets/image (383).png>)

#### The Single Group Connection

Next, let's take a look at the Company 1 Single Group Connection. Navigating to the Applied Objects tab for that Connection, we can see that it is still only applied to the Company 1 Source as we intended.

![The Company 1 Single Group Connection is still only applied to Company 1 Sources](<../../../.gitbook/assets/image (409).png>)

Looking at the list of Connections, there is a newly created "Connection Cloning Single Group Connection". Looking at its Applied Objects tab, it is associated only with Sources in the Connection Cloning Group that we just created! The Sinlge Group Connection logic has also been cloned successfully!

![The Connection Cloning Single Group Connection](<../../../.gitbook/assets/image (399).png>)

One **EXTREMELY IMPORTANT** note: When a Single Group Connection is created by the Clone operation, it will not have any credentials. Because IDO stores all credentials encrypted, it is unable to copy over the existing credentials from the original Connection. Users must manually update the Connections with proper credentials. A warning is printed in the description of the Connection to warn users of this fact.

![The newly created Connection with warning message](<../../../.gitbook/assets/image (414).png>)





The clone has been completed successfuly and preserved the desired user logic! The image below shows the new environment setup with the Connection Cloning objects added.

![The final result of the Clone](<../../../.gitbook/assets/image (408).png>)
