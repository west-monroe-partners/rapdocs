# Table of contents

* [Welcome to Intellio DataOps!](README.md)

## Getting Started

* [DataOps Basics](getting-started/rap-basics/README.md)
  * [How it Works](getting-started/rap-basics/how-it-works-2.md)
  * [Navigation and Interface](getting-started/rap-basics/navigation-and-interface.md)
  * [Prerequisites](getting-started/rap-basics/prerequisites.md)
* [Data Integration Example](getting-started/data-integration-example/README.md)
  * [Setting up](getting-started/data-integration-example/setting-up.md)
  * [Connection](getting-started/data-integration-example/connection.md)
  * [Source](getting-started/data-integration-example/source.md)
  * [Validation and Enrichment](getting-started/data-integration-example/validation-and-enrichment.md)
  * [Output](getting-started/data-integration-example/output.md)

## Architecture

---

* [Logical](logical-architecture-overview/README.md)
  * [DataOps Agent](logical-architecture-overview/rap-agent.md)
  * [User Interface](logical-architecture-overview/user-interface.md)
  * [Data Storage](logical-architecture-overview/data-processing.md)
  * [Data Processing Engine](logical-architecture-overview/data-processing-engine/README.md)
    * [Data Processing Steps](logical-architecture-overview/data-processing-engine/data-processing-1.md)
* [Physical](introduction-to-rap/README.md)
  * [Components](introduction-to-rap/rap-infrastructure-components.md)
  * [Metadata Model](introduction-to-rap/metadata-model.md)

## User Manual

* [Sources](user-manual/source-configuration/README.md)
  * [Settings](user-manual/source-configuration/source-details.md)
  * [Dependencies](user-manual/source-configuration/dependency-configuration.md)
  * [Relations](user-manual/source-configuration/relations-1.md)
  * [Rules](user-manual/source-configuration/enrichment-rule-configuration.md)
  * [Inputs](user-manual/source-configuration/source-inputs.md)
  * [Process](user-manual/source-configuration/process.md)
  * [Data View](user-manual/source-configuration/source-data-view.md)
* [Processing](user-manual/processing/README.md)
  * [Processing Queue](user-manual/processing/processing-queue.md)
  * [Workflow Queue](user-manual/processing/workflow-queue.md)
  * [Resetting Processes](user-manual/processing/resetting-processes.md)
* [Outputs](user-manual/output-configuration/README.md)
  * [Settings](user-manual/output-configuration/output-details.md)
  * [Mapping](user-manual/output-configuration/output-mapping.md)
  * [Process \(Output History\)](user-manual/output-configuration/output-history.md)
* [Connections](user-manual/connections.md)
* [Schedules](user-manual/schedules.md)
* [Agents](user-manual/agents/README.md)
  * [Settings](user-manual/agents/settings.md)
  * [Logs](user-manual/agents/logs.md)
* [Templates and Tokens](user-manual/validation-and-enrichment-rule-templates/README.md)
  * [Tokens](user-manual/validation-and-enrichment-rule-templates/tokens.md)
  * [Relation Templates](user-manual/validation-and-enrichment-rule-templates/relation-templates.md)
  * [Rule Templates](user-manual/validation-and-enrichment-rule-templates/rule-templates.md)
  * [Best Practices](user-manual/validation-and-enrichment-rule-templates/best-practices.md)
* [SDK](user-manual/sdk/README.md)
  * [Custom Post Output](user-manual/sdk/custom-post-output.md)
  * [Custom Ingestion](user-manual/sdk/custom-ingestion.md)
* [Import/Export](user-manual/import-export.md)
* [Managing System Configuration](user-manual/managing-system-configuration.md)

## Operations

* [Deployment & Management](operations/deployment/README.md)
  * [Deploying a Snowflake Instance](operations/deployment/deploying-a-snowflake-instance.md)
  * [Merging Latest Terraform Changes](operations/deployment/merging-latest-terraform-changes.md)
  * [Deployment to AWS](operations/deployment/deployment-to-amazon-web-services/README.md)
    * [Pre Deployment Requirements](operations/deployment/deployment-to-amazon-web-services/pre-deployment-requirements-aws.md)
    * [Performing the Deployment](operations/deployment/deployment-to-amazon-web-services/performing-the-deployment.md)
    * [New Version Upgrade Process \(Terraform\)](operations/deployment/deployment-to-amazon-web-services/new-version-upgrade-process-terraform.md)
    * [New Version Upgrade Process \(Manual\)](operations/deployment/deployment-to-amazon-web-services/using-the-deployment-service-in-ecs.md)
    * [!! Installing a New Agent On ECS \(AWS\)](operations/deployment/deployment-to-amazon-web-services/installing-a-new-rap-agent-ecs.md)
  * [Deployment to Microsoft Azure](operations/deployment/deployment-to-microsoft-azure/README.md)
    * [Pre Deployment Requirements](operations/deployment/deployment-to-microsoft-azure/pre-deployment-requirements.md)
    * [Performing the Deployment](operations/deployment/deployment-to-microsoft-azure/performing-the-deployment.md)
    * [New Version Upgrade Process \(Terraform\)](operations/deployment/deployment-to-microsoft-azure/new-version-upgrade-process.md)
  * [Installing a New Agent](operations/deployment/installing-a-new-agent.md)
* [Metadata Monitoring Dataset](operations/metadata-monitoring-dataset.md)
* [Lineage Queries](operations/lineage.md)

## Releases

* [Announcements](releases/announcements/README.md)
  * [Performance Improvements and Delta Lake in 2.4.1](releases/announcements/performance-improvements-and-delta-lake-in-2.4.1.md)
  * [Quality of Life Features in 2.4.0](releases/announcements/quality-of-life-features-in-2.4.0.md)
  * [2.3.0 Released!](releases/announcements/2.3.0-released.md)
* [Changelog](releases/changelog/README.md)
  * [2.4](releases/changelog/2.4/README.md)
    * [2.4.1](releases/changelog/2.4/2.4.1.md)
    * [2.4.0 - Hotfix](releases/changelog/2.4/2.4.0-june-15th-hotfix.md)
    * [2.4.0](releases/changelog/2.4/2.4.0.md)
  * [2.3](releases/changelog/2.3/README.md)
    * [2.3.3](releases/changelog/2.3/2.3.3.md)
    * [2.3.2](releases/changelog/2.3/2.3.2.md)
    * [2.3.1](releases/changelog/2.3/2.3.1.md)
    * [2.3.0](releases/changelog/2.3/2.3.0.md)
  * [2.2](releases/changelog/2.2/README.md)
    * [2.2.0](releases/changelog/2.2/2.2.0.md)
  * [1.7](releases/changelog/1.7/README.md)
    * [1.7.5.1](releases/changelog/1.7/1.7.5.1.md)
    * [1.7.5](releases/changelog/1.7/1.7.5.md)
    * [1.7.4.2](releases/changelog/1.7/1.7.4.2.md)
    * [1.7.4.1](releases/changelog/1.7/1.7.4.1.md)
    * [1.7.4](releases/changelog/1.7/1.7.4.md)
    * [1.7.3](releases/changelog/1.7/1.7.3.md)
    * [1.7.2.3](releases/changelog/1.7/1.7.2.3.md)
    * [1.7.2.2](releases/changelog/1.7/1.7.2.2.md)
    * [1.7.2.1](releases/changelog/1.7/1.7.2.1.md)
    * [1.7.2](releases/changelog/1.7/1.7.2.md)
    * [1.7.1.1](releases/changelog/1.7/1.7.1.1.md)
    * [1.7.1](releases/changelog/1.7/1.7.1.0.md)
    * [1.7.0.7](releases/changelog/1.7/1.7.0.7.md)
    * [1.7.0.6](releases/changelog/1.7/1.7.0.6.md)
    * [1.7.0.5](releases/changelog/1.7/1.7.0.5.md)
    * [1.7.0.4](releases/changelog/1.7/1.7.0.4.md)
    * [1.7.0.3](releases/changelog/1.7/1.7.0.3.md)
    * [1.7.0.2](releases/changelog/1.7/1.7.0.2.md)
    * [1.7.0.1](releases/changelog/1.7/1.7.0.1.md)
  * [Unsupported Versions](releases/changelog/unsupported-versions/README.md)
    * [2.1](releases/changelog/unsupported-versions/2.1/README.md)
      * [2.1.5](releases/changelog/unsupported-versions/2.1/2.1.5.md)
      * [2.1.4](releases/changelog/unsupported-versions/2.1/2.1.4.md)
      * [2.1.3](releases/changelog/unsupported-versions/2.1/2.1.3.md)
      * [2.1.2](releases/changelog/unsupported-versions/2.1/2.1.2.md)
      * [2.1.1](releases/changelog/unsupported-versions/2.1/2.1.1.md)
      * [2.1.0](releases/changelog/unsupported-versions/2.1/2.1.0.md)
    * [2.0](releases/changelog/unsupported-versions/2.0/README.md)
      * [2.0.11](releases/changelog/unsupported-versions/2.0/2.0.11.md)
      * [2.0.10](releases/changelog/unsupported-versions/2.0/2.0.10.md)
      * [2.0.9](releases/changelog/unsupported-versions/2.0/2.0.9.md)
      * [2.0.8](releases/changelog/unsupported-versions/2.0/2.0.8.md)
      * [2.0.7](releases/changelog/unsupported-versions/2.0/2.0.7.md)
      * [2.0.6](releases/changelog/unsupported-versions/2.0/2.0.6.md)
      * [2.0.5](releases/changelog/unsupported-versions/2.0/2.0.5.md)
      * [2.0.4](releases/changelog/unsupported-versions/2.0/2.0.4.md)
      * [2.0.3](releases/changelog/unsupported-versions/2.0/2.0.3.md)
      * [2.0.2](releases/changelog/unsupported-versions/2.0/2.0.2.md)
      * [2.0.1](releases/changelog/unsupported-versions/2.0/2.0.1.md)
    * [1.6](releases/changelog/unsupported-versions/1.6/README.md)
      * [1.6.5](releases/changelog/unsupported-versions/1.6/1.6.5.md)
      * [1.6.4](releases/changelog/unsupported-versions/1.6/1.6.4.md)
      * [1.6.3](releases/changelog/unsupported-versions/1.6/1.6.3.md)
      * [1.6.2](releases/changelog/unsupported-versions/1.6/1.6.2.md)
      * [1.6.1](releases/changelog/unsupported-versions/1.6/1.6.1.md)
    * [1.5](releases/changelog/unsupported-versions/rap_release_notes_1.5/README.md)
      * [1.5.4](releases/changelog/unsupported-versions/rap_release_notes_1.5/rap_patch_notes_1.5.4.md)
      * [1.5.3](releases/changelog/unsupported-versions/rap_release_notes_1.5/rap_patch_notes_1.5.3.md)
      * [1.5.2](releases/changelog/unsupported-versions/rap_release_notes_1.5/rap_patch_notes_1.5.2.md)
    * [1.4](releases/changelog/unsupported-versions/rap_release_notes_1.4/README.md)
      * [1.4.5](releases/changelog/unsupported-versions/rap_release_notes_1.4/rap_patch_notes_1.4.5.md)
      * [1.4.4](releases/changelog/unsupported-versions/rap_release_notes_1.4/rap_patch_notes_1.4.4.md)
      * [1.4.3](releases/changelog/unsupported-versions/rap_release_notes_1.4/rap_patch_notes_1.4.3.md)
      * [1.4.2](releases/changelog/unsupported-versions/rap_release_notes_1.4/rap_patch_notes_1.4.2.md)
      * [1.4.1](releases/changelog/unsupported-versions/rap_release_notes_1.4/rap_patch_notes_1.4.1.md)
    * [1.3](releases/changelog/unsupported-versions/rap_release_notes_1.3/README.md)
      * [1.3.8](releases/changelog/unsupported-versions/rap_release_notes_1.3/rap_patch_notes_1.3.8.md)
      * [1.3.7](releases/changelog/unsupported-versions/rap_release_notes_1.3/rap_patch_notes_1.3.7_table.md)
    * [1.2](releases/changelog/unsupported-versions/rap_release_notes_1.2.md)
    * [1.1](releases/changelog/unsupported-versions/1.1.md)
    * [1.0](releases/changelog/unsupported-versions/rap_patch_notes_1.0.md)

