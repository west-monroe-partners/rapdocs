---
description: >-
  This section covers the setup of the AWS environment needed for the rest of
  the Data Integration Example.
---

# Setting up

## Step 1: Connect to AWS

The Data Integration Example uses Divvy Bike data hosted on Amazon Web Services' S3 buckets. AWS is a popular platform for data storage that RAP uses internally to process data. Use of AWS during this example provides important exposure to the platform.

Navigate to the AWS S3 Management Console for the region US-East-2: [https://s3.console.aws.amazon.com/s3/home?region=us-east-2](https://s3.console.aws.amazon.com/s3/home?region=us-east-2)

If the login page appears, enter the login information provided by your RAP account team.

## Step 2: Navigate to Divvy Data

Search for the bucket `wmp-rap-sample-data` using the top search bar and select the [bucket](https://s3.console.aws.amazon.com/s3/buckets/wmp-rap-sample-data/?region=us-east-2&tab=overview).

![S3 Bucket Search](../../.gitbook/assets/image%20%28125%29.png)

Select `source-files` in order to navigate to the data file.

![Locate Source Data](../../.gitbook/assets/image%20%28172%29.png)

Right-click the `Divvy_Stations_2017_Q1Q2.csv`file and select **Copy**. For this example, the Divvy Stations data must be copied by each user to their own directory.

![Copy Divvy Stations](../../.gitbook/assets/image%20%2860%29.png)

## Step 3: Create the Environment

Use the breadcrumbs to navigate back to `wmp-rap-sample-data`.

![Navigation to wmp-rap-sample-data](../../.gitbook/assets/image%20%2839%29.png)

Navigate to the `inbox`.

![Inbox](../../.gitbook/assets/image%20%284%29.png)

Select **Create Folder**, name the folder with your initials, then select **Save**. This creates a folder for use during the Data Integration Example.

![Create Main Folder](../../.gitbook/assets/image%20%28137%29.png)

Navigate inside the new folder, and paste the `Divvy Stations` file directly inside. Select **Paste** on the modal that appears to confirm.

![Paste Divvy Stations](../../.gitbook/assets/image%20%28145%29.png)

In the same location, create a folder called `output` for RAP to output to.

![Create Output Folder](../../.gitbook/assets/image%20%2867%29.png)

You should now have the following AWS folders created:

* `s3://wmp-rap-sample-data/inbox/{YOUR INITIALS}`
  * This should have the file `Divvy_Stations_2017_Q1Q2.csv` in the folder
* `s3://wmp-rap-sample-data/inbox/{YOUR INITIALS}/output`

The environment setup is complete. Proceed to [Connection](connection.md) to begin configuring RAP.

