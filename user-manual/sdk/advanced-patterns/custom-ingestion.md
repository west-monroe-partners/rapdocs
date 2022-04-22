# Custom Ingestion

Latest tracking fields are available for each session. These fields provide information about which data has been pulled into IDO in the past. They include:

* **Latest Timestamp**&#x20;
  * For a Timestamp Source, this is the most future timestamp that has been seen in the Date Column of the Source.&#x20;
  * Use it to filter down your data pulls and avoid sending repeat data to IDO.
  * Access it in your Notebook with session.latestTrackingFields.sTimestamp
* **Latest Sequence**&#x20;
  * For a Sequence Source, this is the largest number that has been seen in the Sequence Column of the Source.&#x20;
  * Use it to filter down your data pulls and avoid sending repeat data to IDO.
  * Access it in your Notebook with session.latestTrackingFields.sSequence
* **Extract Datetime**
  * For all sources, this is the most recent timestamp that data was pulled into the Source.&#x20;
  * Use it to filter down your data pulls and avoid sending repeat data to IDO.
  * Access it in your Notebook with session.latestTrackingFields.extractDatetime
* **Input Id**&#x20;
  * For all Sources, the most recent input ID of the Source.&#x20;
  * I'm not sure why this exists to be perfectly honest
  * Access it in your Notebook with session.latestTrackingFields.inputId
