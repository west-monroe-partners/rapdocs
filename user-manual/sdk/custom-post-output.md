# Custom Post Output

The Custom Post Output SDK will allow users to define code in a Databricks Notebook or a custom Jar that will be run after the output process. This page is a "Hello World" for custom post output. Full Scaladoc for the SDK can be found [here](https://docs.intellio.wmp.com/com/wmp/intellio/dataops/sdk/PostOutputSession.html).&#x20;

## Creating a Custom Post Output&#x20;

Any output can have post output commands attached to it. Simply select a Cluster Configuration that specifies the notebook containing the desired commands.&#x20;

Notebooks can also be run manually from Databricks.

See [here](../system-configuration/cluster-and-process-configuration-overview/cluster-configuration/cluster-configuration-for-custom-processing-steps.md) for more information about setting up a Custom Post Output Cluster Configuration.



### Creating your first Notebook

Below is a sample of notebook code that sets up a post output session and then prints a simple Hello World.  A line by line breakdown can be found below.&#x20;

&#x20;Users will need to replace the _**`<DataOpsOutputName> and <DataOpsOutputChannelName>` **_ with the name of the associated custom DataOps output and output channel respectively.

```
import com.wmp.intellio.dataops.sdk._

val session = new PostOutputSession("<DataOpsOutputName>", "<DataOpsOutputSourceName>") 

def helloWorld(): Unit = {
log("Hello World!")
println("Hello World!")
}

session.run(helloWorld)
```

#### Import - Line 1

The first line is a standard import statement. It is needed to utilize all of the DataOps SDK functionality.

#### Post Output Session - Line 3&#x20;

The 3rd line creates a new DataOps Post Output session. When this line is run, a new Post Output process record will be created in DataOps to track the process. It will also begin a heartbeat that constantly communicates with DataOps to ensure the job has not crashed.

#### Creating The helloWorld Function - Line 5-7

The 5th-7th lines are the key part of the post output where the custom user code will go. These lines define a function that is of Unit type. In the example, the code will write a log to DataOps, then print Hello World as well. In the future, replace the code within helloWorld with custom code in order to run your custom code.

#### Executing The Custom Post Output - Line 10

The 10th line runs the custom post output. It pulls the data as specified in the helloWorld function.

### Running the Custom Post Output

To execute this Hello World Example. Simply run the Databricks cell with the code snippet. A progress bar will display at the bottom of the cell, and once it is finished a complete Custom Post Output process will be present in IDO for the Custom Source associated with the output channel!

