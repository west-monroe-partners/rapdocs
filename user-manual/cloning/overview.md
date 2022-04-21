# Overview

## Summary and Use Case

Cloning allows users to create copies of multiple sources/outputs and the relationships that exist between them. This is particularly useful for engagements where the same style of data exists in multiple source systems. Take the below diagram for example.&#x20;

Company 1 has Sources for Sales Order Header and Sales Order Detail.&#x20;

Sales Order Header is related to global Sources for Product, Territory, and Customer.&#x20;

The Sales Order Detail Source writes its data to an Output called Sales Output.

![Company 1 Setup](<../../.gitbook/assets/image (382).png>)

Now, the user wants to configure sources for Company 2, Company 3, Company 4, etc. createing new version of the blue Sources in the diagram above all using the exact same logic and related to the exact same global Sources. After adding sources for Company 2, the environment should look like the below image.

![A setup with Company 1 & 2](<../../.gitbook/assets/image (386) (1).png>)

Without cloning, users are left to recreate all of the logic by hand, a process prone to mistakes that requires a large amount of overhead to manage. However, through the use of Groups and Templates, the entire setup of Company 1 can be copied over for Company 2 with just a few clicks.



For a video summary and a Hello World of how to setup a group of Sources for Cloning, check out [this video](https://www.youtube.com/watch?v=1bX6t-aDkNU\&list=PLFI3u1fSVqjHWR\_pv2gBbZdP-USorY8jw\&index=24). Otherwise, continue onto the next pages for a breakdown of the various cloning features and how to use them. **Note: T**he video covers the content in sections "Very Bsic Cloning Example" through "Cloning Enrichments Example". Connection Name Template and Output Name Template usage are not covered in the video.&#x20;
