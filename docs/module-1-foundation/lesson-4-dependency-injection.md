# Lesson 4: Dependency Injection (DI)

Dependency Injection is a core tenet of modern software architecture, allowing for loose coupling, modularity, and testability.

## DI in the Isolated Worker Model
Because the Isolated Worker model runs as a standalone console application, its DI setup is virtually identical to an ASP.NET Core Web API.

Everything starts in the `Program.cs` file. You configure a `HostBuilder`, register your services, and start the application.

```csharp
var host = new HostBuilder()
    .ConfigureFunctionsWorkerDefaults()
    .ConfigureServices(services =>
    {
        // Registering your business logic services
        services.AddScoped<IOrderValidator, OrderValidator>();
        services.AddSingleton<ICacheService, RedisCacheService>();
    })
    .Build();

host.Run();
```

## Service Lifetimes
Understanding lifetimes is critical in serverless environments to prevent memory leaks and ensure thread safety.

1.  **Transient (`AddTransient`):** A new instance of the service is created *every single time* it is requested from the DI container. Use this for lightweight, stateless services.
2.  **Scoped (`AddScoped`):** A new instance is created *once per function invocation* (e.g., once per HTTP request). If multiple classes inject this service during the same request, they share the exact same instance. This is highly recommended for services like Entity Framework Core `DbContext` or transaction managers.
3.  **Singleton (`AddSingleton`):** A single instance is created the first time it is requested, and that exact same instance is shared across *every* function invocation for the life of the worker process. 
    *   **Crucial Rule:** Singletons must be thread-safe! Since multiple executions of your function might be running concurrently on the same machine, they will all be accessing the exact same Singleton instance simultaneously. Use Singletons for heavy, reusable objects like `HttpClient` or `CosmosClient`.

## Injecting Services
Once registered in `Program.cs`, you simply request the interfaces in your function class's constructor.

```csharp
public class SubmitOrderFunction
{
    private readonly IOrderValidator _validator;
    private readonly ILogger<SubmitOrderFunction> _logger;

    public SubmitOrderFunction(IOrderValidator validator, ILogger<SubmitOrderFunction> logger)
    {
        _validator = validator;
        _logger = logger;
    }

    [Function("SubmitOrder")]
    public async Task<HttpResponseData> Run(...)
    {
        // _validator is available to use!
    }
}
```

## Summary
The Isolated Worker embraces standard .NET DI practices. Proper lifetime management (especially distinguishing between Scoped and Singleton) is key to writing scalable and stable functions.
