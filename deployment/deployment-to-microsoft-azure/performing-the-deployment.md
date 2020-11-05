# Performing the Deployment

## Creating Deployment Principal for Terraform

Navigate to Azure Active Directory and select "App registrations" on the left menu blade. Create a new registration. Make the name something that is appropriate for the master deployment registration - ex: Master-Terraform.

Choose "Accounts in this organizational directory only" for supported account types.

After creating the App Registration, navigate to the API permissions section of the App registration. Click "Add a permission" and navigate to "Supported legacy APIs". Then, click "Application permissions" and then choose these two permissions:

* Application.ReadWrite.All
* Directory.ReadWrite.All

Add the permissions and grant them consent if needed. 

Save the Application \(client\) ID and generate a secret for the app registration. These will be used when populating variables in Terraform Cloud in the next step.

## Setting up Terraform Cloud Workspace

Create a new workspace in Terraform Cloud. Choose "Version control workflow"

## Populating Variables in Terraform Cloud

## Running Terraform Cloud

## Configuring Databricks

## Updating Key Vaults

## Configuring Custom Endpoint

