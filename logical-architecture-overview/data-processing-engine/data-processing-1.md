# Data Processing Steps

![](../../.gitbook/assets/2.0-process-steps.jpg)

Data processing in RAP consists of 9 possible steps.

### Ingest

This is the first step to get data into RAP.  Flat files sources \(CSVs\) are brought over as-is, and database / JDBC sources are extracted as Avro files.

!! Where is this data? How is it transformed?

### Parse

Flat files / CSVs are converted to Avro files.  This is done to allow consistency for downstream steps, so those steps do not need to be aware of the original ingestion format.

!! Where is this data? How is it transformed?

### Capture Data Changes

This is where data changes and updates for the source are determined.

For keyed sources, this is done via row hashes.

For time series sources, data changes are done on a time or sequence overlap.  As time series data is usually large and tends not to have an easily defined primary key, ranges are much more efficient to determine changes.

### Enrich

### Profile

### Refresh / Upsert

### Recalculate

### Output

### Post-Processing

