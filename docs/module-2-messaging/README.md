# Module 2: Event-Driven Architecture (Mid-Level)

## Concepts

### 1. Asynchronous Messaging (Decoupling)
In our current architecture, the `SubmitOrderFunction` (from Module 1) does everything synchronously. If Cosmos DB is down, or if processing takes 10 seconds, the client receives an error or times out.
**The Solution:** Decouple ingestion from processing using message queues.

### 2. Azure Service Bus
Azure Service Bus is an enterprise message broker.
*   **Queues:** Point-to-point communication. `SubmitOrderFunction` puts a message on the queue. Exactly *one* background function pulls it off and processes it.
*   **Topics:** Publish/Subscribe (1-to-many). A message is published, and multiple different subscribers can process copies of it independently.

### 3. Idempotency & Dead Letter Queues (DLQ)
*   **Idempotency:** A function must be designed so that if it executes twice with the same message (e.g., due to a network blip during retry), it does not create duplicate side-effects (like charging a credit card twice).
*   **Dead Letter Queue (DLQ):** If a message fails processing repeatedly (e.g., malformed JSON), Service Bus will eventually move it to a special DLQ so it stops blocking the main queue. You need a function to inspect the DLQ.

### 4. Azure Blob Storage Bindings
Blob Storage is meant for unstructured data (images, CSVs, JSON files).
*   **BlobTrigger:** Executes immediately when a file is uploaded to a specific container.
*   **Bindings:** You can use input/output bindings to easily read/write blobs without writing the heavy Azure Storage SDK code.

### 5. Cosmos DB Output Bindings
Azure Cosmos DB is Microsoft's globally distributed NoSQL database. We will use an output binding in our Service Bus worker to easily persist the order JSON as a document in Cosmos DB.

---

## Next Steps
Open [TASK.md](./TASK.md) to begin the next phase of the project!
