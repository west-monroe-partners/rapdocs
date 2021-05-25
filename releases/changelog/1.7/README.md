# 1.7

### Enhancements

<table>
  <thead>
    <tr>
      <th style="text-align:left">Jira ID</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Problem</th>
      <th style="text-align:left">Impact</th>
      <th style="text-align:left">Risks</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">PROD-261</td>
      <td style="text-align:left">Multiple enhancements to Keyed Sources processing and functionality</td>
      <td
      style="text-align:left">
        <p>Previously the RAP platform could not support certain scenarios with Keyed
          Sources such as:</p>
        <ul>
          <li>Windows Functions</li>
          <li>Columnstore Indexing</li>
          <li>Resetting staging, auto-reprocessing, and deletes</li>
        </ul>
        </td>
        <td style="text-align:left">New more button on the input list screen to manage resets, deletes, and
          data view.</td>
        <td style="text-align:left">High - Users will need to understand how to reset and build keyed sources
          following these new patterns</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-80</td>
      <td style="text-align:left">Manual Outputs</td>
      <td style="text-align:left">Previously the platform did not support manual outputs. This impacted
        use cases where a user would have a need to initiate independent outputs
        that vary from their standard configurations.</td>
      <td style="text-align:left">
        <p>New Manual Output tab on the Outputs screen to support the creation and
          management of manual outputs.</p>
        <p></p>
      </td>
      <td style="text-align:left">Medium - This is a feature outside of automated processing, but could
        affect existing output loads if configured incorrectly</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-89</td>
      <td style="text-align:left">Output history list</td>
      <td style="text-align:left">Previously there was no ability to view historical outputs via the RAP
        interface.</td>
      <td style="text-align:left">New Output History tab on the output details screen.</td>
      <td style="text-align:left">Limited/None - Exposed data that was already in the platform</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-206</td>
      <td style="text-align:left">Snowflake support</td>
      <td style="text-align:left">Due to Snowflake&apos;s increasing market share, there was large demand
        for RAP to support Snowflake as an output data warehouse in conjunction
        with outputting parquet files.</td>
      <td style="text-align:left">New Snowflake and Parquet File radio buttons on the output details screen.</td>
      <td
      style="text-align:left">Limited/None - New functionality</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-297</td>
      <td style="text-align:left">Agent restructuring</td>
      <td style="text-align:left">Agent needed code updates to bring code base in line with the rest of
        the platform standards.</td>
      <td style="text-align:left">More reliable and resilient application, support for major upgrades and
        plugins.</td>
      <td style="text-align:left">Medium - Requires an uninstall and new install for the updated Windows
        MSI</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-68</td>
      <td style="text-align:left">No delete option on file pull</td>
      <td style="text-align:left">Support of use cases that necessitate keeping a copy of the source file
        in the source directory.</td>
      <td style="text-align:left">New checkbox on the source detail screen for file pulls.</td>
      <td style="text-align:left">Limited/None - New functionality</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-210</td>
      <td style="text-align:left">Scheduling parameters not required</td>
      <td style="text-align:left">Support scenarios that necessitate for the scheduler not to run for certain
        sources.</td>
      <td style="text-align:left">Cron text boxes on source details screen are now optional fields.</td>
      <td
      style="text-align:left">Limited/None - Making schedules not mandatory</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-266</td>
      <td style="text-align:left">Prioritization grouping for enrichment rules</td>
      <td style="text-align:left">Previously it was difficult to find and manage chained enrichment rules.
        With the prioritized grouping feature, the system automatically assigns
        a priority group number and presents that value in the enrightments tab.</td>
      <td
      style="text-align:left">New priority column and values on the enrichments tab.</td>
        <td style="text-align:left">Limited/None - Exposed data that was already in the platform</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-295</td>
      <td style="text-align:left">Refactor output actor and worker</td>
      <td style="text-align:left">Code updates needed to bring code base in line with the rest of platform
        standards.</td>
      <td style="text-align:left">Code more organized.</td>
      <td style="text-align:left">Low - Code organization updates</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-339</td>
      <td style="text-align:left">Import/Export system version compatibility with renamed stage.output table
        column.</td>
      <td style="text-align:left">Output refactoring required backend changes to the Postgres database layer.</td>
      <td
      style="text-align:left">To support output changes, the &#x2018;database&#x2019; column on the
        stage.output table has been renamed to &#x2018;output_sub_type&#x2019;.
        This will now resolve issues with import/export between 1.7 and 1.6 environments.</td>
        <td
        style="text-align:left">Low - Stems from some code organization updates, only affects import/export
          for these particular releases</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-343</td>
      <td style="text-align:left">Delete sources</td>
      <td style="text-align:left">Source lists became too long and cluttered with inactive sources.</td>
      <td
      style="text-align:left">New delete button on source details tab.</td>
        <td style="text-align:left">Limited/None - New functionality</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-344</td>
      <td style="text-align:left">Convert text NULL to PostgreSQL NULL during staging</td>
      <td style="text-align:left">System was interpreting NULL values, on certain sources, as text instead
        of values.</td>
      <td style="text-align:left">Update to processing.</td>
      <td style="text-align:left">Limited/None - New functionality</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-369</td>
      <td style="text-align:left">Leveraging date from an input file name</td>
      <td style="text-align:left">When a keyed source doesn&apos;t have a date, there can be a need to associate
        the date from the input file name with each record in that file. This allows
        users to do high level latest record analysis.</td>
      <td style="text-align:left">New get_date_from_file_name checkbox on the source details page.</td>
      <td
      style="text-align:left">Limited/None - New functionaltiy</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-376</td>
      <td style="text-align:left">Auto-reprocess Grouping</td>
      <td style="text-align:left">Grouped auto-reprocess by lookup expression.</td>
      <td style="text-align:left">Update to processing.</td>
      <td style="text-align:left">Medium - Processing changes</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-384</td>
      <td style="text-align:left">Validation and enrichment processing logic</td>
      <td style="text-align:left">Certain enrichment scenarios would fail when a lookup source had not completed
        staging.</td>
      <td style="text-align:left">Update to processing.</td>
      <td style="text-align:left">Low - Preventing failure scenarios</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-409</td>
      <td style="text-align:left">SAP HANA support</td>
      <td style="text-align:left">Unfulfilled demand for supporting SAP HANA sources.</td>
      <td style="text-align:left">New radio button on the connections details screen.</td>
      <td style="text-align:left">Limited/None - New functionality</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-442</td>
      <td style="text-align:left">File name as a system column</td>
      <td style="text-align:left">Use cases where the availability of file name as an S column would be
        helpful to understanding effective dates.</td>
      <td style="text-align:left">New S Column added.</td>
      <td style="text-align:left">Limited/None - Exposed data that was already in the platform</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-452</td>
      <td style="text-align:left">Set the version parameter when submitting an export</td>
      <td style="text-align:left">Inability to set system version parameter on export creating versioning
        conflicts with export.</td>
      <td style="text-align:left">New popup modal when initiating export from sources screen.</td>
      <td style="text-align:left">Limited/None - New functionality</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-456</td>
      <td style="text-align:left">Reorganization of sources advanced parameters</td>
      <td style="text-align:left">With the evolution of source parameters, a refactoring of advance parameters
        was necessary.</td>
      <td style="text-align:left">
        <p></p>
        <ul>
          <li>get_date_from_file_name (all)</li>
          <li>date_column (key)</li>
          <li>datetime_format (key)</li>
          <li>delete_file (all)</li>
          <li>post_processing_folder (all)</li>
          <li>disable_schedule (all)</li>
        </ul>
      </td>
      <td style="text-align:left">Limited/None - Cosmetic reorganization</td>
    </tr>
    <tr>
      <td style="text-align:left">PROD-491</td>
      <td style="text-align:left">Ability to reset all outputs</td>
      <td style="text-align:left">While users could reset individual outputs, there was no option to reset
        all with one click.</td>
      <td style="text-align:left">On the sources inputs tab there is an reset all option within the more
        menu on the header row.</td>
      <td style="text-align:left">Limited/None - Quality of life upgrade</td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>



