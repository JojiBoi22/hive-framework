# Hive-Algorithm Multi-Agentic Framework (v1.0)

## 🧠 Architectural Core

### 1. Supervisor-Worker Hierarchy
- **Supervisor (Gemini CLI):** Maintains global mission context, performs task decomposition, and executes adaptive complexity routing.
- **Specialized Workers:** 
  - `codebase_investigator`: Deep research and dependency mapping.
  - `generalist`: High-volume implementation and refactoring.
  - `verifier`: Specialized verification agent (Generalist in review mode) to ensure code coherence.

### 2. Memory Systems
- **Short-Term Memory (STM):** Managed via `HIVE_STATE.json` to track active variables, current sub-task status, and immediate feedback.
- **Long-Term Memory (LTM):** Utilizes `save_memory` for user preferences and architectural patterns that persist across sessions.

### 3. Context Window Management
- **Sliding Window:** For multi-turn tasks, the Supervisor must prepend a 3-sentence "State Summary" to every sub-agent prompt, discarding data older than 5 turns unless explicitly promoted to STM.

### 4. Feedback Loops
- **Iterative Refinement:** Every "Implementation" task by a worker MUST be followed by a "Verification" task by a different agent instance before the Supervisor accepts the result.

---

## 📡 Communication Protocols

### 1. Structured Handoff Protocol
All worker assignments must follow this JSON-structured prompt format:
```json
{
  "context_summary": "Current state of the hive",
  "task_id": "UUID",
  "dependencies": ["List of required file paths"],
  "expected_output": "Specific technical criteria",
  "metadata_tags": { "origin": "Hive-Supervisor", "purpose": "Refinement" }
}
```

### 2. Error Propagation
- Downstream agents encountering blocks must return an `UPSTREAM_CORRECTION_REQUIRED` signal with a root-cause analysis instead of attempting a "best-guess" fix.

### 3. Metadata Enrichment
- All significant code blocks generated must include a header comment:
  `// Hive-Agent: [Name] | Task: [ID] | Purpose: [Purpose]`

---

## 🚀 Workflow Optimization

### 1. Adaptive Complexity Routing
- **Simple Tasks (<3 files):** Handled directly by Supervisor.
- **Complex Tasks (>3 files or high logic density):** Routing to Worker Swarm.

### 2. Rollback & Consistency
- **Snapshotting:** Before batch operations, the Supervisor must verify a clean git state or create `.hive_bak` files.
- **Consistency Checkpoints:** Intersperse `grep_search` validation turns after every 3 implementation steps.

### 3. Parallel Processing with Synchronization
- Use parallel tool calls for independent research, but utilize `wait_for_previous: true` for "Implementation -> Verification" sequences.
