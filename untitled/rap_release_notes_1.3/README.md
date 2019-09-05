# 1.3

## Enhancements

* Orchestrator now recovers properly in the event of a Postgres crash
* Warning message now displayed when creating two outputs that point to the same table
* Wait Agent has been re-architected to use a database queue
* Output has been re-architected to follow new processing architecture
* Staging has be re-architected to follow new processing architecture
* Alert emails have been standardized and enhanced
* Line Terminator parsing can now deal with line terminators appearing in the data
* Agent is no longer an advanced parameter
* Outputs now output on output source id grain instead of source id, and deletes are run based on output source id

## Bug fixes

* Parameters now save correctly when changing from key to time series on single source
* Two enrichment rules enforced by the same template now show correct lookup source
* Enrichment rule regex issue fixed
* Fixed issue with work.enr\_ not being created upon V&E phase execution
* Fixed issue with V&E status code turning to P after CDC completed
* Enriched column name now consistent between dialog and table
* Source dashboard API now returns all sources
* Order by column now displays for formula rule
* Self referencing validation rules are no longer allowed
* Spaces in column names are now allowed
* Leading and trailing spaces in column names are trimmed on backend now

## Known issues



