# Bootcamp Task: Module 3

Your goal is to replace the simple queue worker with a stateful Durable Functions orchestrator.

## Step 1: Create the Activity Functions
Create three standard Activity functions (using `[ActivityTrigger]`). These represent the steps of fulfillment:
1.  **`ReserveInventoryActivity`**: Takes an `OrderId` and items. Simulate a delay and return `true` if successful.
2.  **`ProcessPaymentActivity`**: Takes an `OrderId` and amount. Simulate calling a payment gateway (e.g., Stripe). Randomly throw an exception to simulate payment failure sometimes.
3.  **`ReleaseInventoryActivity`**: Takes an `OrderId`. This is our **compensation** activity to undo step 1 if step 2 fails.

## Step 2: Create the Orchestrator Function
1. Create a function named `OrderFulfillmentOrchestrator`. Use the `[OrchestrationTrigger]`.
2. Implement the workflow:
   *   Call `ReserveInventoryActivity`.
   *   Wrap the payment step in a `try/catch`. Call `ProcessPaymentActivity`.
   *   If `ProcessPaymentActivity` throws an exception, catch it, log the failure, call `ReleaseInventoryActivity`, and terminate the orchestration.
   *   If successful, call an `ArrangeShippingActivity` (you can mock this).

## Step 3: Update the Client Function
1. Modify your HTTP trigger `SubmitOrderFunction` (or create a new one) to act as the **Durable Client**.
2. Inject `DurableTaskClient`.
3. Instead of putting a message on Service Bus, use the client to start the `OrderFulfillmentOrchestrator` workflow, passing the order details as input.
4. Return the standard Durable Functions `CreateCheckStatusResponse` to the caller. This provides URLs for the client to check the status of the workflow.

## Verification
*   Run the project.
*   Submit an HTTP request. You will receive a JSON response containing `statusQueryGetUri`.
*   Copy that URI and send a GET request to it in your browser/Postman.
*   Watch the status change from `Running` to `Completed` (or `Failed`).
*   Check your logs to verify the orchestration replayed correctly and that the compensation activity executed if the payment failed.
