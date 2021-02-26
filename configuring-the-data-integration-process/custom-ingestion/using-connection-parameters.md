# Using Connection Parameters

#### Accessing Custom Parameters - Line 5

The 5th line access the custom parameters created in the source configuration. It will be a JsObject. See documentation for Play JSON here: [https://www.playframework.com/documentation/2.8.x/ScalaJson](https://www.playframework.com/documentation/2.8.x/ScalaJson)

#### Accessing Connection Parameters - Line 6

The 6th line accesses the connection parameters for the source's custom connection. It will be a JsObject. See documentation for Play JSON here: [https://www.playframework.com/documentation/2.8.x/ScalaJson](https://www.playframework.com/documentation/2.8.x/ScalaJson) This function can also be called with a connection ID or name in order to access connections besides the one configured on the source itself. 

