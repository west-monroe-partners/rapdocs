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

Your RAP account team will provide you with credentials to an AWS account. Once you have logged in, navigate to the S3 service from the Services dropdown menu in the top-left corner of the AWS Management Console.

![Navigate to the S3 service in AWS \(Step 1\)](../../.gitbook/assets/aws-management-console.jpg)

![Navigate to the S3 service in AWS \(Step 2\)](../../.gitbook/assets/aws-management-console-2.jpg)

## Step 3: Explore the S3 Environment

S3 is a storage service in AWS that allows you to store files in a structured hierarchy as you would in a conventional file system such as a Windows computer, for example.

The S3 environment should already be set up based on the requirements of your particular project. Typically, each RAP project will have an _input_ container to hold files that will be ingested into RAP, and an _output_ container which will be the destination of files exported from RAP. 

The S3 environment may have other containers that may be integral to the project workflow. Your RAP account team will let you know how all of the containers in your S3 environment are to be used. 

