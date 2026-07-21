# Module 6: Real-Time & Advanced Production Patterns (Expert)

## Concepts

### 1. Real-Time Client Push (Azure SignalR Service)
When an order finishes processing via a long-running Durable Function, how does the web front-end know?
*   **Bad Way:** The front-end polls the API every 3 seconds.
*   **Enterprise Way:** The front-end connects to Azure SignalR Service via WebSockets. When the Durable Function finishes, it uses a **SignalR Output Binding** to push a message directly to that specific user's browser instantly.

### 2. High-Throughput Streaming
Service Bus Queues are amazing for transactions (like our order processing). But what if we want to log 100,000 user clicks per second?
*   **Azure Event Hubs:** Designed for massive data ingestion (Big Data/Analytics). It uses partitions instead of queues.
*   **Azure Event Grid:** Designed for reactive infrastructure. Instead of a function waking up to poll a blob container, Event Grid instantly routes a "BlobCreated" event to your function the millisecond it happens.

### 3. Advanced Scaling and Cold Starts
*   **Cold Starts:** In the Consumption plan, if your function hasn't been called in 20 minutes, the server spins down. The next request causes a "Cold Start" (taking 2-10 seconds to load .NET). This is unacceptable for synchronous user-facing APIs.
*   **Premium Plan:** Keeps a minimum number of instances "pre-warmed" to guarantee zero cold starts, while still scaling dynamically.
*   **KEDA (Kubernetes Event-driven Autoscaling):** For massive enterprises running Kubernetes, KEDA monitors your Azure Service Bus queue length and tells Kubernetes to spin up more containers of your Azure Function to handle the load.

---

## Next Steps
Open [TASK.md](./TASK.md) to implement real-time push!
