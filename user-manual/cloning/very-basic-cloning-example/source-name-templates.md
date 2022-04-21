# Source Name Templates

## Use Case

Source Name Templates allow users to setup IDO to dyanmically insert the GROUP name of a source into its actual Source Name. Take the Company 1 Source that was used as an example in the Groups documentation.&#x20;

![The Company 1 Source](<../../../.gitbook/assets/image (385) (1) (1).png>)

As it currently stands, this Source is a part of the Company 1 Group and the name "Company 1 Sales Order Header" reflects that properly. However but there is nothing preventing a user from switching the name to Company 2 Sales Order Detail, which would result in a total mess! IDO needs a way to automatically place the GROUP into the Source Name to keep configurations safe. The Source Name Template.



## Source Name Templates

A Source Name Template is another extremely simple object. It has 1 configurable field: Source Name. That field has 1 requirement. It must have the text ${GROUP} somehwere in it. The ${GROUP} token will tell IDO where the user wants the Group name dynamically inserted into the Source name. Take our "Company 1 Sales Order Detail" Source. It has two parts, the "Company 1" Group, and the "Sales Order Detail" that indicates what data is in the Source. Creating a Source Name Template for it would look like the image below.

![Creating the Sales Order Detail Source Name Template](<../../../.gitbook/assets/image (394) (1) (1).png>)

Notice the ${GROUP} token where "Company 1" would normally go and the rest of the Source name is normal.

## Applying the Name Template

Once Groups and Source Name Templates have been created, applying them is simple. Simply navigate to the Source Settings page, and choose the applicable Group and Source Name Template from the dropdowns. Below is an image showing the application of the "Company 1" Group and the "${GROUP} Sales Order Detail" Name Template to the Company 1 Sales Order Detail Source. Notice that the Name field is now greyed out because it will be automatically calculated by IDO based on the Group and Source Template.

![Appling a Group and Source Name Template](<../../../.gitbook/assets/image (397) (1) (1).png>)

These name templates will be extremely important when using the Clone functionality. When cloning, all of the newly created Sources will create their names based on the applied Source Name Template and Group.
