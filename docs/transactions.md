### Common Strategies to Handle Transactional Consistency
1. **Saga Pattern:**
Break the transaction into local steps with compensating actions if any step fails.

2. **Outbox Pattern:**
Store events in the same DB transaction and publish them reliably from an outbox table.

3. **Idempotent Operations:**
Ensure repeated requests/events have the same effect to support retries safely.

4. **Eventual Consistency:**
Accept temporary inconsistencies and sync data over time via events or reconciliation.

5. **Retry + Dead Letter Queues:**
Retry transient failures and move irrecoverable ones to a DLQ for investigation.

6. **Unique Transaction IDs:**
Use consistent IDs across services to track, correlate, and deduplicate operations.

7. **Audit Logs & Monitoring:**
Maintain traceable logs of all events and actions to detect and resolve inconsistencies.
