# Resetting Processes

During the development lifecycle, users will need to reset their sources often to change source settings, apply rules, update output tables, and restart failed processes. The following is a "reset cheatsheet" that should be referenced when deciding how to reset data on source during development.

The following buttons are considered the "source level reset" buttons and will be referred to throughout the guide.

![](../../.gitbook/assets/image%20%28359%29.png)

The following buttons are considered the "input level reset" buttons and will be referred to throughout the guide.

![](../../.gitbook/assets/image%20%28358%29.png)

## Ingestion Parameters Have Changed

If source query is changed, then there are a couple options that may need to happen on the source. If a completely new dataset is being pulled with the updated source query, it is recommended to delete all source data and delete the metadata on the source. Another common tactic is to have a source query that pulls all data in a historic load, and then following source queries parameterized to pull deltas. In this scenario, there is no need to clear data or metadata on the source. 

If the "Force Case Insensitive" parameter is changed from true to false - then you may need to delete all source data and delete the metadata on the source. If this is not done, and source data is case sensitive, you will most likely run into situations where the next data that's pulled in gets columns that are aliased like "column\_1".

## Parsing Parameters Have Changed

For sources with the File Connection Type, a parsing process will run for every input on the source. The parse process is what parses the data from flat file to an IDO usable format. Changing any parsing parameter should require a reset of the parse process on the input level reset buttons. If the change to the parsing parameter could alter the raw metadata on the source, it's recommended to delete all source data, clear the metadata on the source, and reload data for the source.

## Refresh Type or Change Data Capture Parameters Have Changed

If the refresh type on the source is changed, the source will automatically show a pop up on save that will ask you to reset CDC. It is HIGHLY RECOMMENDED to follow this prompt and reset CDC. Once you click save and reset CDC, a process will be enqueued on the source to reset CDC.

If any CDC parameters have changed, a similar prompt will show up. It is HIGHLY recommended to follow this prompt and reset CDC.

{% hint style="warning" %}
A common processing misstep that users find themselves in is to reset CDC after making changes to Rules. This results in extra processing time for the source and is unnecessary.
{% endhint %}

{% hint style="danger" %}
There is a known bug in IDO version 2.3.3 and lower where resetting CDC on any input except the oldest one on keyed sources can cause duplicate data in the hub table. This issue is fixed in version 2.4.0
{% endhint %}

## Validation/Enrichment Rules Have Changed

If any Validation or Enrichment rules are updated or created, there are two options to propagate the changes to the data.

1. Use the source level reset button called "Recalculate". Recalculate will then run and check the rules and current hub table and run any rules that have been updated or are new, resulting in an up to date hub table. The benefit of recalculate is that it will run on the entire source, and not each individual input, saving the user processing time. Recalculate will also skip the entire "refresh" process, which can also have long and expensive processing time.
2. Use the source level reset button called "Reset All Enrichment". An enrichment process will be queued up and run on each input on the source, with the normal processes to follow on the source.
3. Reset enrichment for an individual input. The enrichment process will run and the normal following processes will run on the input.

## Output Parameters/Mappings Have Changed

If any changes have been made to the Output settings or Output Mappings, then there are two options to reset the Output.

1. Use the source level reset button called "Reset All Output". The benefit of resetting output this way is that refresh types are optimized to run in this fashion, especially when running deletes on the target output table.
2. Use the input level reset button called "Reset Output". This will reset the output for the individual inputs data. Is generally useful for unit testing changes to output settings before running all inputs on the source through output.

## Ingestion Has Failed

A failed Ingestion process can not be reset. Delete the failed input using the input level "Delete" button and use the source level button "Pull Now" to kick off a new Ingestion process.

## Parsing Has Failed

Once the error has been resolved, use the input level "Reset Parsing" button to enqueue a new parse process for the input. Source level reset button can be used here as well if multiple inputs have failed.

## Capture Data Changes Has Failed

Once the error has been resolved, use the input level "Reset CDC" button to enqueue a new Capture Data Changes process for the input. Source level reset button can be used here as well if multiple inputs have failed.

## Enrichment or Refresh Has Failed

Once the error has been resolved, use the input level "Reset Enrichment" button to enqueue a new Enrichment process for the input. If Refresh failed, the Enrichment process will run first, then Refresh will run. Source level reset button can be used here as well if multiple inputs have failed.

## Output Has Failed

Once the error has been resolved, use the input level "Reset Output" button to enqueue a new Output process for the input. Source level reset button can be used here as well if multiple inputs have failed.



