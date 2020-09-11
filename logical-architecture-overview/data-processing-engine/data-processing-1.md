# !! Data Processing Steps

![](../../.gitbook/assets/2.0-process-steps.jpg)

Data processing in RAP consists of 9 possible steps. During each step is can be helpful to think of a columns being appended to a table. During each step the data is modified with additional columns which represent id's, keys, updates, and metadata. Especially as business logic is applied in CDC through refresh.

### Ingest

This is the first step to get data into RAP.  Flat files sources \(CSVs\) are brought over as-is, and database / JDBC sources are extracted as Avro files. During this phase data is strictly copied into the appropriate Amazon S3 or Microsoft Azure storage location. The data is not transformed in any way.

### Parse

Flat files / CSVs are converted to Avro files.  This is done to allow consistency for downstream steps, so those steps do not need to be aware of the original ingestion format. In the parse step the file is opened, observed and transformed into the standard format of an Avro file. The copy of the data is stored in the parse section of the Amazon S3 or Microsoft Azure storage locations.

### Change Data Capture \(CDC\)

With all of the data in same Avro format, Change Data Capture is the first step that enacts business logic on the data. This step tags data changes, and applies specific column and meta data based on the configured Refresh Type.

| Refresh Type | Impact |
| :--- | :--- |
| Keyed | Refresh compares the key value and associated time stamp to update the data to be the most up to date. |
| Sequence \(Time Series\) | Refresh looks at time or sequence overlap. Since time series data is usually large, ranges are often more efficient to determine up to date data. |
| FULL | Deletes data previously in RAP and uploads the data as if starting from scratch. |
| NONE | All data is assumed to be new data, and no refresh check occurs. |

### Enrich

Enrich executes business rules, calculations, and transformations against the data. Enrich is the primary step for these transformations. The scope for the calculations in this step are at the row level. Custom columns are created per entry. No windowing or aggregation occurs at this step.

### Profile

### Refresh / Upsert

Refresh represents the merge against the HUB table and the creation of the "one source of truth" dataset. Only one HUB table exists per source.

### Recalculate

Recalculate modifies the HUB table, and applies business logic that requires the entire table: cross row calculations, windowing, ranking. !! Parameter Keep current. When &lt;&gt; is defined as "keep current" the hub table applies the appropriate calculation to the HUB table at this step.

!! Note that calculations occurring at this step are more resource intensive.

### Output

Output is typically just the mapping of sources to a destination. RAP by default persists logic all the way to the output warehouse.  

RAP does not allow the user to adjust the grain of the data, so at this point aggregations, unpivots and relational database logic occurs. 

### !! Post-Processing

Post-Processing is a performance step.



!! Expensive Calculation. As you move through the logical data flow the resources become more intensive to utilize.

