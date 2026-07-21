# Module 5: Observability and DevOps (Senior)

## Concepts

### 1. Observability (Application Insights)
In a distributed system, a single user request might span an HTTP function, a Service Bus queue, an Orchestrator, and a Cosmos DB database. How do you track an error?
*   **Distributed Tracing:** Azure Application Insights automatically injects correlation IDs into HTTP headers and Service Bus messages. You can view an "Application Map" in Azure to see the full flow of a request visually.
*   **Custom Metrics:** You should track business value, not just CPU usage. (e.g., "Total Revenue Processed").

### 2. Infrastructure as Code (Terraform)
Clicking through the Azure Portal to create resources is unacceptable in enterprise. It is prone to human error and cannot be version-controlled.
*   **Terraform:** An open-source tool by HashiCorp to define your infrastructure (Storage, Function App, Service Bus) in configuration files (HCL).
*   **State:** Terraform keeps track of what it deployed in a state file. When you change the file, Terraform calculates the "diff" and applies only the changes.

### 3. CI/CD (Azure DevOps Pipelines)
*   **Continuous Integration (CI):** Every time you push code to Git, a pipeline compiles the .NET code and runs your xUnit tests.
*   **Continuous Deployment (CD):** Once CI passes, the pipeline runs Terraform to provision infrastructure, and then zips up your compiled .NET code and deploys it to the Azure Function App.

---

## Next Steps
Open [TASK.md](./TASK.md) to implement DevOps practices.
