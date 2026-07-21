# Bootcamp Task: Module 1

Your goal is to scaffold the project and build your first two functions.

## Step 1: Scaffold the Project
1. Open your terminal in the root of the bootcamp folder (`E:\Git\AzureFunctions`).
2. Create an `apps` directory and navigate into it.
3. Use the Azure Functions Core Tools (or Visual Studio / Rider) to create a new .NET 8 Isolated Worker project named `OrderFulfillmentApp`.
   *   *Hint:* If using CLI, you can create the folder `OrderFulfillmentApp`, navigate inside, and run `func init . --worker-runtime dotnet-isolated --target-framework net8.0`
4. Inside the `apps` directory, create a standard xUnit test project named `OrderFulfillmentApp.Tests` and add a project reference to your function app.

## Step 2: Create the HTTP Ingest Function
1. Create a function named `SubmitOrderFunction`.
2. Configure it with an `HttpTrigger` that accepts `POST` requests on the route `/api/orders`.
3. Define a C# record or class for the incoming payload (e.g., `OrderRequest` with properties like `OrderId`, `CustomerId`, `TotalAmount`).
4. Read the body of the `HttpRequestData`, deserialize it, and log the incoming order using an injected `ILogger`.
5. Return a `202 Accepted` status code.

## Step 3: Implement Unit Tests
1. In your `tests` project, write a unit test for `SubmitOrderFunction`.
2. You will need to mock `HttpRequestData` and `FunctionContext` (which can be tricky in the isolated worker, look up Moq strategies for `HttpRequestData`).
3. Verify that when a valid JSON payload is sent, a 202 is returned and the logger is invoked.

## Step 4: Create a Timer Trigger
1. Create a function named `GenerateDailySalesReportFunction`.
2. Configure it with a `TimerTrigger` to run at midnight every day. (Hint: Learn how NCronTab expressions work, e.g., `"0 0 0 * * *"`).
3. For now, simply inject `ILogger` and log a message saying "Generating daily sales report...".

## Verification
*   Start Azurite locally.
*   Run the project (`func start` or hit F5 in your IDE).
*   Use Postman or curl to send a POST request to your `SubmitOrderFunction`. Verify the logs.
*   Verify your unit tests pass.
