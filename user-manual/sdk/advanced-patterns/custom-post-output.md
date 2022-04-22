# Custom Post Output

Custom ingest sessions can be embedded within custom post output sessions to create a Loopback. The example below will pull create an input and write data into the CustomIngestTest source every time the PostOutput Output runs!

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
