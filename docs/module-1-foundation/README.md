# Module 1: The Foundation (Junior)

## Concepts

### 1. In-Process vs. Isolated Worker Model
Azure Functions in .NET used to run in the same process as the host (`In-Process`). Microsoft now recommends the **Isolated Worker Model**.
*   **Why?** It runs your function code in a separate .NET worker process. This completely decouples your code from the Azure Functions host, meaning you have full control over the startup, dependency injection, and you avoid dependency conflicts with the host process.
*   **Standard:** All modern .NET 8+ enterprise apps use Isolated Worker.

### 2. Local Development
*   **Azurite:** A local emulator that mimics Azure Storage (Blob, Queue, Table). Azure Functions requires a storage account to manage its internal state (like timer locks). Azurite allows you to run locally without an Azure subscription.
*   **Azure Functions Core Tools:** The CLI that allows you to run `func start` and test your functions locally.

### 3. Triggers & Bindings
*   **Trigger:** What causes the function to run (e.g., an HTTP request, a timer firing, a message on a queue). A function can only have **one** trigger.
*   **Binding:** A declarative way to connect to data from within your code (e.g., reading from Cosmos DB or writing to a Service Bus). You can have multiple input and output bindings.

### 4. Dependency Injection (DI)
Because we are using the Isolated Worker model, the startup looks exactly like a standard ASP.NET Core application (`Program.cs`). You can inject `IConfiguration`, `ILogger`, and your custom services.

### 5. Unit Testing
Testing Azure Functions involves mocking triggers (like `HttpRequestData`) and checking the outputs. We use `xUnit` and `Moq`.

---

## Next Steps
Open [TASK.md](./TASK.md) to begin building your project!
