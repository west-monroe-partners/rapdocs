# Resetting Processes

During the development lifecycle, users will need to reset their sources often to change source settings, apply rules, update output tables, and restart failed processes. The following is a "reset cheatsheet" that should be referenced when deciding how to reset data on source during development.

## Ingestion Parameters Have Changed

If source query is changed, then there are a couple options that may need to happen on the source. If a completely new dataset is being pulled with the updated source query, it is recommended to delete all source data and delete the metadata on the source. Another common tactic is to have a source query that pulls all data in a historic load, and then following source queries parameterized to pull deltas. In this scenario, there is no need to clear data or metadata on the source. 

If the "Force Case Insensitive" parameter is changed from true to false - then you may need to delete all source data and delete the metadata on the source. If this is not done, and source data is case sensitive, you will most likely run into situations where the next data that's pulled in gets columns that are aliased like "column\_1".

## Parsing Parameters Have Changed

For Delimited or Fixed 

## Refresh Type or Change Data Capture Parameters Have Changed

If the refresh type on the source is changed, the source will automatically show a pop up on save that will ask you to reset CDC. It is HIGHLY RECOMMENDED to follow this prompt and reset CDC. Once you click save and reset CDC, a process will be enqueued on the source to reset CDC.

If any CDC parameters have changed, a similar prompt will show up. It is HIGHLY recommended to follow this prompt and reset CDC.

{% hint style="warning" %}
A common processing misstep that users find themselves in is to reset CDC after making changes to Rules. This results in extra processing time for the source and is unnecessary.
{% endhint %}

## Validation/Enrichment Rules Have Changed



