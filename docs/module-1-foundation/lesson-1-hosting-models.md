# Lesson 1: Azure Functions Hosting Models

Understanding the architectural difference between the **In-Process** and **Isolated Worker** models is fundamental to modern Azure Functions development.

## The Legacy Way: In-Process Model
Historically, .NET Azure Functions ran in the same memory process as the Azure Functions Host itself.
*   **How it worked:** The Azure host (written in .NET) loaded your DLL directly into its process and invoked your methods.
*   **The Problem:** Because you shared the process with the host, you were strictly bound to the exact .NET version the host was using. If the host used `Newtonsoft.Json v10`, and your code required `v13`, you experienced painful dependency conflicts. You also lacked control over the startup process and dependency injection, as the host managed everything.

## The Modern Standard: Isolated Worker Model
Microsoft introduced the **Isolated Worker Model** to solve these architectural limitations. Today, all new enterprise .NET 8+ applications should use this model.
*   **How it works:** Your function app runs as a completely independent, separate .NET process (effectively a standalone console application). 
*   **Communication:** The Azure Functions Host still handles the heavy lifting (listening for HTTP requests, pulling from Service Bus, managing scaling), but it communicates with your isolated worker process under the hood using **gRPC**.

### Key Advantages of the Isolated Worker
1.  **Full Control:** You have a `Program.cs` file, giving you complete control over application startup, middleware, and Dependency Injection, exactly like a standard ASP.NET Core web API.
2.  **No Version Conflicts:** Because your code runs in a separate process, you can use whatever NuGet packages and versions you want without colliding with the host's dependencies.
3.  **.NET Version Independence:** You can upgrade to the latest .NET versions (like .NET 8 or 9) immediately upon their release, without having to wait for Microsoft to update the underlying Functions Host.
4.  **Custom Middleware:** You can write middleware that intercepts requests before they hit your function logic (ideal for custom authentication, logging, or exception handling).

## Summary
When building a new Azure Function, you are essentially building a standard .NET application that registers with the Azure Functions framework. The "magic" is handled out-of-process, leaving you with clean, standard C# code.
