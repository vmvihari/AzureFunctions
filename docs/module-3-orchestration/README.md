# Module 3: Orchestration and State (Mid/Senior-Level)

## Concepts

### 1. The Limits of Simple Queues
In Module 2, our queue worker processed the order in one go. What if processing an order requires multiple steps that take a long time? E.g., 
1. Check Inventory
2. Charge Credit Card (external API might take 30 seconds)
3. Arrange Shipping
If the function fails on step 3, how do we resume? We don't want to charge the credit card again! Writing this state-machine logic manually using Service Bus queues is incredibly complex.

### 2. Durable Functions
Durable Functions is an extension that lets you write stateful workflows in a serverless environment. It manages state, checkpoints progress, and handles restarts automatically behind the scenes (using Azure Storage).

There are three types of functions in this pattern:
1.  **Client Function:** The trigger that starts the workflow (e.g., our HTTP ingest or a Service Bus trigger).
2.  **Orchestrator Function:** The workflow logic. It calls Activity functions. It must be **deterministic** (no random numbers, no external API calls, no Guids directly). It replays its history to remember where it left off.
3.  **Activity Function:** The actual work. These do not have to be deterministic. This is where you call APIs, query databases, etc.

### 3. Common Patterns
*   **Function Chaining:** Execute Activity A, then pass result to Activity B, then to C.
*   **Fan-out/Fan-in:** Execute 10 instances of an Activity in parallel, wait for *all* of them to finish, then proceed.
*   **Async HTTP APIs:** Start a long-running process and return a `202 Accepted` with a URL the client can poll to check the status of the workflow.

### 4. Sagas and Compensation
If you chain activities together (Reserve Inventory -> Charge Payment) and "Charge Payment" fails, you need to "undo" the "Reserve Inventory" step. This is called a compensation transaction or Saga pattern.

---

## Next Steps
Open [TASK.md](./TASK.md) to implement a Durable orchestration!
