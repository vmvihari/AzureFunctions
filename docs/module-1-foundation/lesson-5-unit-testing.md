# Lesson 5: Unit Testing

Testing in serverless environments can be notoriously difficult if the framework isn't designed for it. Fortunately, the Isolated Worker model was designed from the ground up to be highly testable.

## The Testing Philosophy
You should not be writing complex business logic directly inside the Azure Function method. 
*   **The Anti-Pattern:** A function method that is 300 lines long, parses JSON, validates business rules, connects to a database, and returns a response.
*   **The Enterprise Pattern:** The Azure Function is a thin "wrapper." Its only job is to receive the trigger, pass the data to an injected Application Service (e.g., `IOrderService`), and then map the result to an Output Binding or HTTP response.

By following this pattern, you can write standard unit tests for your `OrderService` without needing to involve the Azure Functions framework at all.

## Mocking Framework Objects
When you *do* need to test the Function class itself (to ensure routing, bindings, and status codes are correct), you will need to mock framework objects.

In the old In-Process model, mocking HTTP requests was extremely difficult because the framework relied on deep ASP.NET Core internal classes. 

In the Isolated Worker model, Microsoft introduced abstract classes like `HttpRequestData` and `FunctionContext` specifically so they can be easily mocked using libraries like **Moq** or **NSubstitute**.

### Example Testing Strategy
Using `xUnit` and `Moq`:
1.  **Arrange:** Create a mock of `FunctionContext`. Create a mock of `HttpRequestData` and populate it with a simulated JSON body (e.g., a test order) and headers. Create mocks for any injected services (like `IOrderValidator`) and setup their expected return values.
2.  **Act:** Instantiate your Function class, passing the mocked services into the constructor. Call the `Run` method, passing the mocked `HttpRequestData`.
3.  **Assert:** Verify that the function returned an `HttpResponseData` with a `202 Accepted` status code. Use Moq's `Verify()` method to ensure that `IOrderValidator.Validate()` was actually called exactly once during the execution.

## Summary
Keep your function classes thin and delegate logic to injected services. Use the Isolated Worker's abstract classes (`HttpRequestData`, `FunctionContext`) alongside mocking frameworks to verify trigger logic and response mapping.
