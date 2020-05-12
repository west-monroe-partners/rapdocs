---
description: >-
  This section covers the setup of the AWS environment needed for the rest of
  the Data Integration Example.
---

# Setting up

## Step 1: Download the Divvy Data

The Data Integration Example uses Divvy Bike data hosted on Amazon Web Services' S3 buckets. Click to download the file [Divvy\_Stations\_2017\_Q1Q2.csv](https://wmp-rap-sample-data.s3.us-east-2.amazonaws.com/source-files/Divvy_Stations_2017_Q1Q2.csv). Save it locally.

## Step 2: Connect to AWS

AWS is a popular platform for data storage that RAP uses internally to process data. Use of AWS during this example provides important exposure to the platform.

Navigate to the AWS S3 Management Console for your firm. [https://s3.console.aws.amazon.com/s3/home?region=us-east-2](https://s3.console.aws.amazon.com/s3/home?region=us-east-2)

If the login page appears, enter the login information provided by your RAP account team.

## Step 2: Navigate to the Training Folder

Search for the bucket `training` using the top search bar and select the bucket with your firm's initials as a prefix. It should look like `{firm initials}.training`. Select the bucket.

![S3 Bucket Search](../../.gitbook/assets/image%20%28242%29.png)

In the training bucket, there should be an **input** and **output** folder.

![Training Folder](../../.gitbook/assets/image%20%28211%29.png)

## Step 3: Create a Unique Input Folder

Select the **input** folder.

![Input Folder](../../.gitbook/assets/image%20%28144%29.png)

Click **Create folder** and create a folder with your initials in the **input** folder. If you have the same initials as somebody else, add a suffix that is unique, such as your birthday. Click **Save** to create the folder.

![Create Input Initials Folder](../../.gitbook/assets/image%20%28126%29.png)

Select the input folder with your initials.

![Select Initials Folder](../../.gitbook/assets/image%20%2866%29.png)

In the input initials folder, select **Upload**.

![Upload Button](../../.gitbook/assets/image%20%28239%29.png)

The **Upload** modal should appear. Drag the file **Divvy\_Stations\_2017\_Q1Q2.csv** into the modal. Select **Upload** to upload the file with default settings. 

![Upload File](../../.gitbook/assets/image%20%2869%29.png)

The file should now be in your unique input folder. Verify that it has been uploaded.

![](../../.gitbook/assets/image%20%28195%29.png)

## Step 4: Create a Unique Output Folder

Use the breadcrumbs to navigate back to `training` folder.

![Navigation to the training folder](../../.gitbook/assets/image%20%2846%29.png)

Select the `output` folder.

![Output Folder](../../.gitbook/assets/image%20%28138%29.png)

Click **Create folder** and create a folder with your initials in the **output** folder. If you have the same initials as somebody else, add a suffix that is unique, such as your birthday. Click **Save** to create the folder.

![Create Output Initials Folder](../../.gitbook/assets/image%20%28173%29.png)

You should now have the following AWS folders created:

* `s3://{FIRM INITIALS}.training/input/{YOUR INITIALS}`
  * This should have the file `Divvy_Stations_2017_Q1Q2.csv` in the folder
* `s3://{FIRM INITIALS}.training/output/{YOUR INITIALS}`

The environment setup is complete. Proceed to [Connection](connection.md) to begin configuring RAP.

