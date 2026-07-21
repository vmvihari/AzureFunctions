# Module 1: The Foundation (Junior)

Welcome to Module 1! Before writing any code, it is crucial to build a strong theoretical foundation. In an enterprise environment, understanding *how* the underlying architecture works is just as important as knowing the syntax.

The lessons below provide deep dives into the core mechanics of Azure Functions. After reviewing these lessons, you will have the knowledge required to confidently design, discuss, and implement modern serverless applications.

## Course Lessons

Please read through the following lessons in order:

1.  **[Lesson 1: Azure Functions Hosting Models](./lesson-1-hosting-models.md)**
    *   Understand the architectural shift from In-Process to the modern Isolated Worker model and why it matters for enterprise development.
2.  **[Lesson 2: Local Development Environment](./lesson-2-local-development.md)**
    *   Learn how the Azure Functions Core Tools, `local.settings.json`, and Azurite emulate the cloud environment on your local machine.
3.  **[Lesson 3: Triggers and Bindings](./lesson-3-triggers-and-bindings.md)**
    *   Discover the defining feature of Azure Functions: how to declaratively dictate execution flow (Triggers) and data movement (Bindings) without boilerplate SDK code.
4.  **[Lesson 4: Dependency Injection (DI)](./lesson-4-dependency-injection.md)**
    *   Learn how to structure your application using standard .NET Core DI patterns, and understand the critical differences between Transient, Scoped, and Singleton lifetimes in a serverless context.
5.  **[Lesson 5: Unit Testing](./lesson-5-unit-testing.md)**
    *   Learn enterprise testing philosophies, why functions should be "thin wrappers", and how the Isolated Worker model makes mocking framework objects straightforward.

---

## Next Steps
Once you understand the theory, it's time to write some code! 
Open [TASK.md](./TASK.md) to begin building your project!
