# Custom Post Output



* Custom Parameters and custom connections can be used to allow a single notebook to connect to multiple different data sources. i.e. Create a generic SalesForce connector, then specify the table name in the custom parameters.&#x20;
* Custom ingest sessions can be embedded within custom post output sessions to create a loopback. See the example below:

```
import com.wmp.intellio.dataops.sdk._
import play.api.libs.json._
import org.apache.spark.sql.DataFrame

//Create Post output session
val postOutputSession = new PostOutputSession("development","POstOutput","Demo Company 1 SalesOrderDetail")

//Post output logic
def postOutputCode(): Unit = {
  //spark.sql("CREATE OR REPLACE VIEW customPostOutput AS SELECT * FROM development.hub_1")
  
  //Ingestion Session to loopback source
  val ingestSession = new IngestionSession("development", "CustomIngestTest")
  
  //Ingestion Logic
  def ingestionCode(): DataFrame = {
    spark.sql("SELECT * FROM customPostOutput")
  }
  //Run ingest
  ingestSession.ingest(ingestionCode)
}

//Run post output code
postOutputSession.run(postOutputCode)
```
