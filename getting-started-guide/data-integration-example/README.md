---
description: This example acts as a Hello World tutorial for DataOps
---

# Data Integration Example

## Introduction

This simple introductory example looks at one specific use of DataOps.

For more detailed configuration instructions, please refer to the [Configuration Guide](../../configuring-the-data-integration-process/).

For operation documentation, refer to the [Operation Guide](https://github.com/west-monroe-partners/rapdocs/tree/dbdd67bb9146c835a1a1a52830857289997bebd8/operation-guide/README.md).

This Data Integration Example progresses in five parts:

* [Setting up](setting-up.md)
* [Connection Configuration](connection.md)
* [Source Configuration](source.md)
* [Validation and Enrichment Configuration](validation-and-enrichment.md)
* [Output Configuration](output.md)

## Requirements

{% hint style="info" %}
Access to a DataOps environment
{% endhint %}

{% hint style="info" %}
Access to an Amazon S3 Bucket for flat file input & output
{% endhint %}

{% hint style="info" %}
Access to Google Chrome, the only browser fully-certified for use with DataOps
{% endhint %}

## About the Data Source

Divvy is a Chicago-based bike sharing service. Every quarter, they release two data sets – one file for every ride taken between two stations by customers throughout the quarter, and one that accounts for every Divvy station in the city.

This tutorial runs through a scenario using the Divvy Stations data for Q1 and Q2 2017, available in a public AWS bucket here: [Divvy\_Trips\_2017\_Q1Q2.zip](https://s3.amazonaws.com/divvy-data/tripdata/Divvy_Trips_2017_Q1Q2.zip). The [Setting up](setting-up.md) section covers all the steps to using Divvy Stations data from AWS.

