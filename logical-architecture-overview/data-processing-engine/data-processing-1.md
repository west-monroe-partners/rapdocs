# Data Processing Steps

![The logical data flow with data locations underneath. ](../../.gitbook/assets/2.0-process-steps.jpg)

Data processing in DataOps consists of 9 possible steps. During each step is can be helpful to think of a columns being appended to a table. During each step the data is modified with additional columns which represent id's, keys, updates, and metadata. Especially as business logic is applied in CDC through refresh.

### Ingest

This is the first step to get data into DataOps.  Flat files sources \(CSVs\) are brought over as-is, and database / JDBC sources are extracted as Avro files. During this phase data is strictly copied into the appropriate Amazon S3 or Microsoft Azure storage location. The data is not transformed in any way.

### Parse

Flat files / CSVs are converted to Avro files.  This is done to allow consistency for downstream steps, so those steps do not need to be aware of the original ingestion format. In the parse step the file is opened, observed and transformed into the standard format of an Avro file. The copy of the data is stored in the parse section of the Amazon S3 or Microsoft Azure storage locations.

### Change Data Capture \(CDC\)

With all of the data in same Avro format, Change Data Capture is the first step that enacts business logic on the data. This step tags data changes, and applies specific column and meta data based on the configured Refresh Type.

| Refresh Type | Impact |
| :--- | :--- |
| Keyed | Refresh compares the key value and associated time stamp to update the data to be the most up to date. |
| Sequence \(Time Series\) | Refresh looks at time or sequence overlap. Since time series data is usually large, ranges are often more efficient to determine up to date data. |
| FULL | Deletes data previously in DataOps and uploads the data as if starting from scratch. |
| NONE | All data is assumed to be new data, and no refresh check occurs. |

### Enrich

Enrich executes business rules, calculations, and transformations against the data. Enrich is the primary step for these transformations. The scope for the calculations in this step are at the row level. Custom columns are created per entry. No windowing or aggregation occurs at this step. There is no change in the grain of the data at this step.

When thinking of the data processing steps in terms of the components and subcomponents of a SQL statement, Enrich accounts for actions such as such as a single SELECT column \(Enrichment Rule\), or a single WHERE clause \(Validation Rule\)​.

### Refresh / Upsert

Refresh represents the merge against the HUB table and the creation of the "one source of truth" dataset. Only one HUB table exists per source. Depending on the refresh type, information pertaining to history and tracking changes may also be captured.

### Recalculate

Recalculate modifies the HUB table, and applies business logic that requires the entire table: cross row calculations, windowing, ranking. In the Sources setting there is a parameter that when defined as "keep current" will directly affect the Recalculate step. When "keep current" is checked every time new data is ingested and processed through the logical data flow, the Hub table at the this step of recalculate will activate and process business logic relevant to the entire Hub table. Recalculations modify the Hub table in place, as processing that require the entire Hub table would generate the same results regardless if run multiple times.

{% hint style="info" %}
As you move along the data processing steps the resources to manipulate the data become much more intense. As much as possible utilize the Enrich stage and steps earlier in the data processing flow.
{% endhint %}

### Output

Output is the mapping of a source to a destination. Intellio DataOps \(RAP\) by default persists logic all the way to the outputted warehouse. Output maps Hub Table columns to an output file, and then sends the Output file to the appropriate destination. Data processing up until this point does not adjust the grain of the data, so at this point aggregations, unpivots and relational database logic can occurs

Historically DataOps could only output data which was contained in the ingestion location, but in RAP 2.0 and the addition of Relations, Output now includes the data in the ingestion source as well as related data.

### Post-Processing / Synopsis

The Post-Processing step, sometimes referred to as the Synopsis step, is a performance step. This is the step when aggregate calculations are conducted on the output. This step is the most resource expensive. At this step aggregation across Sources can occur. The reason for Post-Processing / Synopsis is to provide a rollup to third party \(typically\) BI tools such as Looker, Tableau. This step could occur on the BI tool side, but to ensure validity of the data it is recommended to occur on the Intellio DataOps side.

{% hint style="info" %}
Due to the intensive resource utilization in the Post-Processing step, there may be instances where Output could be redirected to an Ingestion location and the data is processed through DataOps a second time to reduce resource costs.
{% endhint %}

