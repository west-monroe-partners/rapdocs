# Creating a Multi Group Connection

We will start off by taking a look at an example Connection. It is configured to pull data from the AdventureWorks database and is attached to the Company Sales Order Header Source.

![The Multi Group Connection](<../../../.gitbook/assets/image (399).png>)

Notice in the above image that the Group and Connection Template fields are both blank and the Connection Name is hard coded to "Multi Group Connection".&#x20;

With this seup, the Connection is not assicated with any Group. It exists separately from all of the Groups and is able to be connected to ANY Source or Output in the environment. Think of this Connection in a similar way to the Global Sources in the Cloning with Relations example. When a Source attached to this Connection is cloned,the newly created Source will be attached to the EXACT same Connection.

That is it for creating a Multi Group Connection. Multi Group is treated as the default behavior when Cloning and requires very little additional configuration. See the next section for steps required to create a Single Group Connection using Connection Name Templates.
