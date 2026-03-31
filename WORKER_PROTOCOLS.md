# Worker Protocols for Hive-Algorithm

## 📤 Output Standard
All Workers MUST report their findings in this format to maintain Hive coherence:

```json
{
  "status": "SUCCESS | PARTIAL | ERROR",
  "task_id": "UUID-from-handoff",
  "files_affected": ["/full/path/to/file"],
  "technical_summary": "Concise summary of logic changes",
  "verification_required": true,
  "upstream_corrections": []
}
```

## 🛡️ Mandate: No Silent Failures
If a tool call fails, a Worker MUST NOT attempt a workaround that violates the `expected_output` in the handoff packet. They must report `ERROR` with an `upstream_correction` request.

## 🏷️ Metadata Enrichment
Every Worker must start each code block with a metadata tag:
`// Hive-Agent: [Type] | Task: [ID] | Purpose: [Purpose]`
