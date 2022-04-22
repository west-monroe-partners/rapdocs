# Parameters

## Use Case

Each Custom Ingest, Custom Parse, and Custom Post Output has a Custom Parameters object that allows the users to pass values into the Notebook. The main use case for this is to reduce Notebook duplication. For example, imagine a Custom Notebook that runs a query `SELECT * FROM TableA`. Now imagine another Notebook that runs `SELECT * FROM tableB` and another running `SELECT * FROM tableC` like the image below.

![Three almost identical Notebooks sending data to IDO](<../../../.gitbook/assets/image (400).png>)

Managing these three almost identical Notebooks can create overhead and room for user error. Instead, it would be much easier to have a single Notebook that could run a different query depending on which Source was running, like the image below.

![One Notebook with a dynamic table](<../../../.gitbook/assets/image (395).png>)

Custom parameters will allow us to do this! Look below for an example similar to the use case described above.

## Example

To start, we have a Source called CustomIngest. It is a Custom Ingest Source. In its Ingestion parameters, there is a Custom Parameters object that is defaulted to {}. This object is a JSON object and follows [JSON standard](https://www.json.org/json-en.html). For our example, we will pass a "TableName" value to the Notebook set to "tableA". Configuring the Parameters to do so looks like the image below.

![Setting the Custom Parameters](<../../../.gitbook/assets/image (393).png>)

Now that the parameter is set, let's look at Custom Notebook code that would leverage it. For this example we will start with the example Custom Ingest notebook from the Hello World Example with a few added lines. The first, line 3 is an import of the Play API Json library. Since the Parameters are stored as JSON, we will need this library to parse them.&#x20;

Next look at lines 8 and 9. On line 8, we access the tableName parameter from the customParameters of the Custom Ingest Session. This returned value, which will be tableA in this case, will be stored as the tablename Scala value.

Finally look at line 9, it runs a `SELECT *` query, but the table name is dynamically set to the tablename Scala val. If we change the val, we change which table the Notebook is pulling from!

```
import com.wmp.intellio.dataops.sdk._
import org.apache.spark.sql.DataFrame
import play.api.libs.json.{JsArray, JsValue, Json}

val session = new IngestionSession("<DataOpsSourceName>") 

def ingestDf(): DataFrame = {
    val tablename = (session.customParameters \ "tableName").as[String]
    val df = spark.sql("SELECT * FROM " + tableName)
    return df
}

session.ingest(ingestDf)
```

Now the user can configure more Sources in IDO with different tableNames in the Custom Parameters i.e. {"tableName": "tableB"} and run it through the same Databricks Notebook to get different results per source!

Please note that the session.customParameters object is available for all Custom Process types (Ingestion, Parsing, and Post Output)
