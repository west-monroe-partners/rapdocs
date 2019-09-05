---
description: This section describes the process of setting up a connection in RAP.
---

# Connection

## Step 1: Create a Connection

RAP communicates to external data services through Connections. Connections consist of configurations for **where** and **what type** of data to pull into RAP. Both Sources and Outputs can use connections. For this example, we will configure one file location to pull data from \(Source\) and one to push the data out to \(Output\).

### Opening RAP 

In a Chrome browser, navigate to the RAP Sources screen by accessing the URL provided by your RAP account team and logging in.

### Making a new Connection

Navigate to the Connections screen through the Menu, as seen below.

![Navigation Menu - Connections](../../.gitbook/assets/screenshot_0.png)

On the Connections screen, click New Connection in the top right corner. The image below highlights the correct button.

![Create a New Connection](../../.gitbook/assets/screenshot_0b.png)

### Naming your new Connection

The below image shows where to add a unique Name and write a brief Description. When naming a Connection, be sure to follow the [Naming Conventions]() guide. Use `Divvy - Input Path` for the Source Connection Name.

![New Connection Name and Description Fields](../../.gitbook/assets/screenshot_6.png)

{% hint style="warning" %}
Connection Names must be unique.
{% endhint %}

{% hint style="warning" %}
Note that all fields marked with an asterisk \(\*\) are mandatory.
{% endhint %}

#### Extra Options

{% hint style="info" %}
The below image ****shows the Active toggle. This is used to deactivate or reactivate a connection. By default, all connections are active when created. Deactivating a connection removes it from the active sources list, and stops any processes from running on the connection. Once this connection is reactivated, it resumes input/output.
{% endhint %}

## Step 2: Select Connection Type

![Active toggle](../../.gitbook/assets/image%20%2822%29.png)

The below image shows the Connection Type control. Connection type selection dynamically updates the available options.

Select `File`. This tutorial pushes a file to RAP as input and then outputs a file. For use cases, see the below tabs.

{% tabs %}
{% tab title="Table" %}
**Table** is a connection to an external database.
{% endtab %}

{% tab title="SFTP" %}
**SFTP** \(SSH File Transfer Protocol\) is a file protocol used to access files over an encrypted SSH transport.
{% endtab %}

{% tab title="File" %}
**File** is used to access files and can be local or in Amazon S3.
{% endtab %}
{% endtabs %}

![Connection Type Options](../../.gitbook/assets/image%20%28169%29.png)

## Step 3: Indicate File Path

Finally, file\_path is the path to the location of the file, excluding the filename. Recall that the input data is located at `s3://{FIRM INITIALS}.training/input/{YOUR INITIALS}`.

{% hint style="warning" %}
After configuring the **Source** connection, be sure the input file is in the correct folder or S3 bucket.
{% endhint %}

Click **Save** to finish Source Connection configuration. 

## Step 4: Create an Output Connection

Click **Save** to finish Output Connection configuration. When both Source and Output Connections are configured, move onto [Source](source.md).

## Step 4: Create Output Connection

An additional Connection will be required during the [Output](output.md) phase. Repeat the previous steps to configure an Output Connection with the following parameters:

* **Name:** `Divvy - Output Path`
* **Connection Type:** `File`
* **file\_path:** `s3://{FIRM INITIALS}.training/output/{YOUR INITIALS}` 

![Output Connection Configuration](../../.gitbook/assets/image%20%28198%29.png)

