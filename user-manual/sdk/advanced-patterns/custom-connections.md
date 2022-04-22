# Custom Connections

Custom Connection allow users to store sensitive parameters and pass them into Custom Ingest and Custom Parse notebooks. IDO will encrypt and mask these parameters so that they are never visible in plain text to anyone using the platform. They are useful for storing passwords used to access data.

To create a Custom Connection, navigate to the Connections tab and click New Connection. The Connection Direction should be set to Source and the Connection Type should be set to Custom. This will cause two parameters to appear. Public Connection Parameters and Private Connection Parameters.

![An empty Custom Connection](<../../../.gitbook/assets/image (385).png>)

### The Parameters

The Public Connection Parameters are stored unencrypted and visible to all users of IDO. Never store a password or sensitive information here, but feel free to keep a username or URL here. The format of the parameter is a JSON. Find more infomation about JSON standard [here](https://www.json.org/json-en.html). For this example we will update the value to {"username":"exampleUser"}.

The Private Connection Parameters are stored encrypted and masked from IDO users. Once they are saved, you'll have to retype the values if you ever want to update them. This is a good place to store passwords. Like the Public Parameters, this parameter is also a JSON For this example, we will store the value {"password":"example123"}

![Typing the values into the Connection](<../../../.gitbook/assets/image (380).png>)

### Saving the Connection

Clicking the save button at the bottom of the page will save the Connection to the backend. Notice that the Public Parameters are still visible in plain text, but the Private Parameters are not masked. The next step will pull these values into an example noteboook.

![The Private Parameters have been masked](<../../../.gitbook/assets/image (397).png>)

### Using the Connection

For this example we will start with the example Custom Ingest notebook from the Hello World Example with two added lines. The first, line 3 is an import of the Play API Json library. Since the Parameters are stored as JSON, we will need this library to parse them. The 2nd is line 8. In this line, we access the session.connectionParameters object. This object is automatically available on any Custom Ingest or Custom Parse session. Line 8 pulls the username value out of the connection Parameters using the Play Json library and saves it into the username Scala value.&#x20;

```
import com.wmp.intellio.dataops.sdk._
import org.apache.spark.sql.DataFrame
import play.api.libs.json.{JsArray, JsValue, Json}

val session = new IngestionSession("<DataOpsSourceName>") 

def ingestDf(): DataFrame = {
    val username = (session.connectionParameters \ "username").as[String]
    val values: List[Int] = List(1,2,3,4,5) 
    val df: DataFrame = values.toDF()
    return df
}

session.ingest(ingestDf)
```

The password value can be accessed in the same way as the username value, just replace the "username" portion wiith "password". However it is recommended to NEVER save a sensitive value into a val as someone might print it out or find it in a stack trace. Instead, plug the entire `(session.connectionParameters \ "username").as[String]` snippet into the location where the password is required.

