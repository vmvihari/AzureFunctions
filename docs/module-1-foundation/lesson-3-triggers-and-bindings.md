# Lesson 3: Triggers and Bindings

Triggers and Bindings are the defining feature of Azure Functions. They strip away boilerplate code, allowing you to focus purely on business logic rather than SDK management.

## 1. Triggers (The "When")
A **Trigger** defines *how* a function is invoked. Every function must have exactly **one** trigger. You cannot have zero triggers, and you cannot have two triggers on the same function.

*   **HTTP Trigger:** The function runs when it receives an HTTP request (REST API). You configure the allowed methods (GET, POST) and the route template (`/api/orders/{id}`).
*   **Timer Trigger:** The function runs on a schedule defined by a CRON expression. It runs as a singleton (only one instance runs at a time, even if scaled out).
*   **Event-Driven Triggers:** The function runs in response to an event (e.g., a message arriving on a Service Bus Queue, a file being uploaded to Blob Storage, or a row being modified in Cosmos DB).

## 2. Bindings (The "What")
A **Binding** is a declarative way to connect data to your function. You can have **zero or many** bindings on a single function. They abstract away the complex SDK code needed to authenticate and connect to Azure services.

### Input Bindings
Data passed *into* your function. 
*   *Example:* If your trigger provides an `OrderId`, you can use a Cosmos DB Input Binding to automatically fetch the full `OrderDocument` from the database and pass it into your function signature as a parameter. You didn't write any database querying code; the framework did it for you.

### Output Bindings
Data passed *out* of your function to another service.
*   *Example:* Instead of injecting a `ServiceBusClient`, establishing a connection, building a message batch, and sending it, you simply define a Service Bus Output Binding. In the Isolated Worker model, you usually return a specific object, and the framework automatically serializes it and pushes it to the queue when the function completes.

## Example Signature (Isolated Worker)
```csharp
[Function("ProcessOrder")]
[ServiceBusOutput("processed-orders", Connection = "ServiceBusConn")] // Output Binding
public async Task<string> Run(
    [HttpTrigger(AuthorizationLevel.Function, "post")] HttpRequestData req, // Trigger
    [BlobInput("inbox/{name}", Connection = "BlobConn")] string blobContent) // Input Binding
{
    // Business logic goes here.
    return "Message for Service Bus";
}
```

## Summary
Triggers dictate execution flow, while Bindings handle data movement. Together, they drastically reduce the amount of infrastructure code you have to write.
