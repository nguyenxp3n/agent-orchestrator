# Playbook: Operating in Environments Without Docker

## Trigger
The repository or test suite expects Docker, but container runtimes are unavailable or hardware resources are constrained.

## Procedure
1. **Identify the underlying requirement**: Determine what Docker was providing (database instance, message broker, or clean build environment).
2. **Deploy lightweight alternatives**:
   - Use SQLite or in-memory database drivers for unit and component tests.
   - Connect to a shared local database instance using separate schemas or database names per worker.
   - Employ in-memory mock stubs for external third-party services.
3. **Adjust parallel concurrency**: If resource isolation cannot be proven on local hardware, reduce the number of concurrent workers and serialize task execution.