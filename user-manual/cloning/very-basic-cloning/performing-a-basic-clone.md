# Performing a Basic Clone

## Overview

In the prior two sections, we setup a Group for Company 1, a Source Name Template for Sales Order Detail, and applied both to a Source, Company 1 Sales Order Detail. On this page, we will perform our first clone to create "Basic Clone Sales Order Detail". A Source that is identical to Company 1 Sales Order Detail, except that it exists in a newly created "Basic Clone" Group.

## The Setup

Below is the Company 1 Sales Order Detail Source. It is the only Source in its Group.

![All alone](<../../../.gitbook/assets/image (388).png>)

Navigating to the Applied Objects tab in the Groups page for Company 1, the user can see that there is only one assoicated object.

![Only 1 Associated Source](<../../../.gitbook/assets/image (403).png>)

## Performing the Clone

#### Exporting the Group

Now we will clone the "Company 1" Group, to create the "Basic Clone" Group. First, the user must navigate to the Groups page of the UI and locate the Group they desire to clone. Along the right side of the screen, a 3 dot button exists. Clicking that button will open a menu with the Clone option. Clicking the Clone button will display a screen summarizing the objects included in the Group. It is HIGHLY recommended that users check this summary to ensure no extra objects are being included in the Clone.

![The Company 1 Clone Summary](<../../../.gitbook/assets/image (383).png>)

Clicking "Proceed" will download a file containing all of the Configuration details for the Group.

#### Updating the Export File

In order to clone this Group, the user will need to supply a new Group name to put all of the newly create objects into. For this example, we will use the Group "Basic Clone". To specify this new Group name, the user must open the file that was downloaded in the step above and locate the "Group" key as seen below. Simply replace the value with the desired new Group Name and resave the file.

![The Export file with Group name updated to Basic Clone](<../../../.gitbook/assets/image (395).png>)

#### Uploading the File to Perform the Clone

After resaving the file with the new Group name, the user must navigate back to the Groups page to perform the Clone. Locate and click the "Import" button along the top of the page. A popup will launch where the user can specify the Clone file that was edited in the steps above. After picking a file IDO will display a summary of all objects that will be created as part of the Clone operation. It is highly recommended that users analyze this popup to ensure no unexpected objects are included.

![The Import Summary](<../../../.gitbook/assets/image (399).png>)

After validating the Import Summary, scroll down and click the "Import" button. The window will disappear and a process will run to complete the Cloning procedure. Within 30 seconds, a refresh of the page should show the new Group.&#x20;

![The newly created Group](<../../../.gitbook/assets/image (378).png>)

The newly created Source will also be available in the Sources list with identical settings to the original Company 1 Sales Order Detail Source.

![The newly created Source](<../../../.gitbook/assets/image (381).png>)



## What's Next

This simple example showed how Cloning creates exact copies of Sources. In the next sections, we will explore how Relations and Rules can be used in Cloning to create brand new Relations and Source Specific Rules.
