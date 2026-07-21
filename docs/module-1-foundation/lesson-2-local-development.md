# Lesson 2: Local Development Environment

Enterprise systems require robust local development experiences. You must be able to run and test everything on your machine without relying on deploying to the cloud.

## Azure Functions Core Tools
The Core Tools provide a local command-line experience. When you run `func start`, the core tools emulate the Azure Functions Host runtime locally on your machine.
*   It exposes a local HTTP server for your API endpoints.
*   It monitors local configuration files.
*   It hooks into debuggers (like Visual Studio or VS Code) allowing you to set breakpoints and step through code.

## Application Configuration: `local.settings.json`
In Azure, your configuration (like database connection strings) is stored securely in **App Settings** or Key Vault. Locally, these settings are stored in `local.settings.json`.
*   **Never Commit This File:** This file is for local secrets only and is ignored by `.gitignore` by default.
*   **Structure:** Environment variables are placed inside the `"Values"` object. 
*   **Access:** Your function code reads these seamlessly using the standard `IConfiguration` interface, completely unaware of whether it is running locally or in Azure.

## Azurite: The Storage Emulator
Azure Functions *requires* an Azure Storage Account to function properly, even if your code isn't explicitly reading or writing blobs. 
*   **Why?** The internal Azure Functions Host uses Storage for state management. For example, if you have a `TimerTrigger` set to run every 5 minutes, the host uses Blob Storage leases (distributed locks) to ensure that if you scale out to 10 instances, only *one* instance actually executes the timer.
*   **Local Emulation:** Instead of provisioning a real cloud storage account just for local testing, we use **Azurite**. Azurite is an open-source emulator that runs locally (often via Docker or an NPM package) and perfectly mimics Azure Blob, Queue, and Table storage.
*   **Configuration:** In your `local.settings.json`, you will see a setting `"AzureWebJobsStorage": "UseDevelopmentStorage=true"`. This connection string tells the local runtime to connect to Azurite instead of the cloud.

## Summary
To run a function locally, you need three things: your compiled code, the Azure Functions Core Tools (to host it), and Azurite (to provide the underlying state management).
