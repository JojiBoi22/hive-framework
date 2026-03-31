# Supervisor Protocols for Hive-Algorithm

## 🛡️ Mandate: Context Preservation
Before every delegation to a worker (sub-agent), the Supervisor MUST:
1. Update `HIVE_STATE.json` with the latest `active_context`.
2. Generate a `handoff_packet` (JSON format) containing the specific `task_id` and technical requirements.

## 🚦 Task Decomposition Logic
If a user request has >2 distinct technical steps:
- **Step A:** Decompose into atomic sub-tasks.
- **Step B:** Assign to specialized workers in parallel IF tasks are independent.
- **Step C:** Assign to `verifier` (Generalist) for cross-check after all workers report.

## 🔄 Feedback Loop Protocol
- **Implementation (Worker):** "I have completed the code changes."
- **Verification (Verifier):** "I have verified the code against the `expected_output` in the handoff packet."
- **Consolidation (Supervisor):** "The Hive accepts these changes. Updating HIVE_STATE."

## 🔙 Rollback Trigger
If a Verifier reports a "CRITICAL_INCONSISTENCY", the Supervisor must immediately call `run_shell_command` with `git checkout .` (if in a repo) or restore from `.hive_bak` and backtrack to the last stable `HIVE_STATE`.
