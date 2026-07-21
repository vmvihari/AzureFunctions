# Bootcamp Task: Module 4

Your goal is to harden the application by removing connection strings and adding retry policies.

## Step 1: Implement DefaultAzureCredential
1. Update your project to use Identity-based connections.
2. In `local.settings.json`, remove the connection strings for Service Bus and Cosmos DB.
3. Replace them with identity configurations (e.g., `ServiceBusConnection__fullyQualifiedNamespace` = `your-namespace.servicebus.windows.net`).
4. In your code, you will rely on the Azure SDKs and the bindings to use `DefaultAzureCredential`.
5. *Note:* For local testing, `DefaultAzureCredential` will use your Azure CLI login (`az login`) or Visual Studio credentials to authenticate with Azure resources!

## Step 2: Integrate Azure Key Vault (Mock)
1. Add a mocked API Key setting for your payment gateway in `local.settings.json` (e.g., `PaymentGatewayApiKey`).
2. Update `Program.cs` to configure the DI container to pull configuration from Key Vault if running in Azure, otherwise use local settings. (Look into `builder.Configuration.AddAzureKeyVault`).

## Step 3: Implement Polly Resiliency
1. In `Program.cs`, register an `HttpClient` for your Payment Gateway service using `AddHttpClient`.
2. Add the Polly NuGet packages (`Microsoft.Extensions.Http.Polly`).
3. Attach a Transient Fault Handling policy to the `HttpClient` that retries 3 times with exponential backoff if the payment service returns a 5xx error or times out.
4. Inject this configured `HttpClient` into your `ProcessPaymentActivity` and use it.

## Step 4: APIM Configuration (Theory)
1. Create a `mock-apim-policy.xml` file in this folder.
2. Write a basic APIM policy XML that applies a rate limit of 5 calls per minute to your HTTP ingest function. (This is a theory exercise, you don't need to deploy APIM yet).

## Verification
*   Ensure the function still runs locally and can connect to Azure Service Bus/Cosmos DB using *only* your local developer credentials, with no connection strings in the configuration.
*   Simulate a failure in your mocked Payment Gateway and observe Polly automatically retrying the HTTP calls in the logs.
