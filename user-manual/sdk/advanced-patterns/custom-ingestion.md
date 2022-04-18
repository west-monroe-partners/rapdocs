# Custom Ingestion



### Advanced Patterns

* Custom Parameters and custom connections can be used to allow a single notebook to connect to multiple different data sources. i.e. Create a generic SalesForce connector, then specify the table name in the custom parameters.
* Latest tracking fields are available for each session. They include latest timestamp, latest sequence, extract datetime, and input id. They can be accessed with session.latestTrackingFields.{sTimestamp, sSequence, extractDatetime, inputId}
