# Module 4: Enterprise Security & Resilience (Senior)

## Concepts

### 1. Managed Identities (Passwordless Connections)
Storing connection strings in your configuration or code is a massive security risk. 
*   **The Problem:** `Endpoint=sb://mysb.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=123...` (If this leaks, anyone has full control of your queue).
*   **The Solution:** Managed Identities. Azure assigns an identity (like a service account) to your Function App. You grant that identity "Service Bus Data Receiver" role on the queue. 
*   **In Code:** You simply provide the host name (`mysb.servicebus.windows.net`) and use `DefaultAzureCredential` from the Azure Identity SDK. It automatically handles token acquisition securely behind the scenes.

### 2. Azure Key Vault
For secrets that you *must* manage (e.g., an API key for Stripe or a third-party service), you store them in Azure Key Vault. Your Function App uses its Managed Identity to securely read the secrets from Key Vault at startup.

### 3. Resiliency with Polly
If you call an external API (like a payment provider) and it times out, what do you do?
*   **Retries:** You should automatically retry transient failures (e.g., HTTP 503 Service Unavailable) with exponential backoff (wait 2 seconds, then 4, then 8).
*   **Circuit Breaker:** If the payment API is completely down and fails 10 times in a row, stop calling it immediately (open the circuit) for 30 seconds to prevent overwhelming the downstream system, then try again.
*   **Polly:** The standard .NET library for implementing these resilience patterns.

### 4. Azure API Management (APIM)
Enterprise APIs are rarely exposed directly to the public internet. They are put behind a gateway. APIM provides routing, rate-limiting, IP whitelisting, caching, and analytics for your Azure Functions.

---

## Next Steps
Open [TASK.md](./TASK.md) to secure and harden your application.
