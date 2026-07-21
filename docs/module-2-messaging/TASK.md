# Bootcamp Task: Module 2

Your goal is to transition the system to an asynchronous event-driven model.

## Step 1: Refactor the HTTP Ingest Function
1. Update `SubmitOrderFunction`. Instead of just logging the order, it should now publish the `OrderRequest` as a JSON message to an Azure Service Bus queue named `order-queue`.
2. *Hint:* You can use the `ServiceBusClient` injected via DI, OR you can use an **Output Binding** `[ServiceBusOutput("order-queue", Connection = "ServiceBusConnection")]`. Output bindings are much less code!
3. Ensure it still returns a `202 Accepted` immediately. The user shouldn't wait for processing.

## Step 2: Create the Queue Worker Function
1. Create a new function named `ProcessOrderFunction`.
2. Configure it with a `ServiceBusTrigger` listening to `order-queue`.
3. In this function, validate the order. If the `TotalAmount` is negative, throw an exception (this will test retry behavior).
4. Add a Cosmos DB **Output Binding** `[CosmosDBOutput(databaseName: "OrderDb", containerName: "Orders", Connection = "CosmosDbConnection")]`.
5. Map the incoming queue message to the output binding to save it to the database.

## Step 3: Implement the Dead Letter Queue (DLQ) Handler
1. Create a function named `OrderDLQMonitorFunction`.
2. Configure it with a `ServiceBusTrigger` listening to `order-queue/$DeadLetterQueue`.
3. Whenever a message fails processing in `ProcessOrderFunction` (e.g., the negative amount exception) and hits the max delivery count, it moves here.
4. Log a high-severity alert indicating a poison message was found.

## Step 4: Create a Product Catalog Uploader
1. Create a new function named `ProcessCatalogUploadFunction`.
2. Configure it with a `BlobTrigger` listening to the container `catalogs/{name}`.
3. This simulates a system where admins upload CSV files of new inventory.
4. When triggered, log the name of the file and its size in bytes.

## Verification
*   Add configuration for `ServiceBusConnection`, `CosmosDbConnection`, and `AzureWebJobsStorage` to your `local.settings.json`. (You can provision free tiers in Azure or use emulators).
*   POST an order to your HTTP endpoint. You should receive a 202 immediately.
*   Observe the logs to see `ProcessOrderFunction` pick up the message from the queue and write it to Cosmos DB.
*   POST an invalid order (negative amount) and observe it failing, retrying, and eventually hitting the `OrderDLQMonitorFunction`.
