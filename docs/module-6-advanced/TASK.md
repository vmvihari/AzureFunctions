# Bootcamp Task: Module 6

Your final goal is to add real-time status updates to the system.

## Step 1: Configure SignalR Connection
1. We will mock Azure SignalR Service locally. Ensure you understand how the `SignalRConnection` string would be placed in Key Vault in a real environment.
2. In your HTTP ingest function (`SubmitOrderFunction`), before kicking off the orchestration, you would normally assign a unique Connection ID or User ID to the request.

## Step 2: Implement SignalR Output Binding
1. Create a new function named `BroadcastOrderStatusFunction`.
2. This function can be triggered by the end of your Orchestrator, or by a Service Bus topic that the orchestrator publishes to when finished.
3. Configure it with a `[SignalROutput(HubName = "orderHub", Connection = "SignalRConnection")]` binding.
4. Map your output payload to send a message (e.g., `Target = "orderStatusUpdated", Arguments = new[] { orderId, "Completed" }`).

## Step 3: Event Grid Transition (Theory)
1. Re-examine your `ProcessCatalogUploadFunction` from Module 2 (the BlobTrigger).
2. The `BlobTrigger` actually uses polling behind the scenes (it checks the storage account logs every few seconds).
3. **Task:** Rewrite the signature of the function to use an `EventGridTrigger` instead. Event Grid will actively *push* the event to your function the instant the blob is created, vastly improving latency and reducing storage transaction costs.

## Verification
*   You have successfully architected a highly scalable, real-time, event-driven enterprise system using Azure Functions!
*   Review your entire architecture: HTTP Ingest -> Service Bus -> Durable Orchestrator -> Cosmos DB -> SignalR Push. This is exactly how massive scale cloud-native applications are built today.
