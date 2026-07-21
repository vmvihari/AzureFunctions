# Bootcamp Task: Module 5

Your goal is to prepare the application for automated deployment to Azure.

## Step 1: Application Insights & Telemetry
1. In `Program.cs`, ensure Application Insights telemetry is enabled for the Isolated Worker (`builder.Services.AddApplicationInsightsTelemetryWorkerService()`).
2. Inject `TelemetryClient` into your Orchestrator or Activity functions.
3. Track a custom business event when an order succeeds: `_telemetryClient.TrackEvent("OrderFulfilled", properties: new { OrderId = id, Amount = amount });`.

## Step 2: Terraform Infrastructure
1. Create an `infrastructure` folder at the root of the repo.
2. Create a `main.tf` file.
3. Write Terraform code (using the `azurerm` provider) to provision:
   *   An Azure Resource Group
   *   An Azure Storage Account (required by Functions)
   *   An Application Insights instance
   *   An Azure App Service Plan (Consumption Y1 or Premium EP1)
   *   An Azure Function App (Windows or Linux)
   *   An Azure Service Bus Namespace and Queue

## Step 3: Azure DevOps YAML Pipeline
1. Create a file named `azure-pipelines.yml` at the root of the repo.
2. Define a multi-stage pipeline:
   *   **Stage 1 (Build):** Run `dotnet build`, `dotnet test`, and `dotnet publish` to zip the output. Publish the zip as a pipeline artifact.
   *   **Stage 2 (Deploy Infra):** (Optional/Mock) Run `terraform init` and `terraform apply` to provision the resources defined in Step 2.
   *   **Stage 3 (Deploy Code):** Use the `AzureFunctionApp@2` task to deploy the zipped artifact to the Function App created by Terraform.

## Verification
*   If you have an Azure Subscription, try running `terraform apply` locally to see if your infrastructure provisions correctly!
*   If you deploy the code, check Application Insights in the Azure Portal to see your custom "OrderFulfilled" events appearing.
