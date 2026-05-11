```typescript
task(
  subagent_type: "general",
  description: "Fix - Retry <N>/3",
  prompt: `
## Task: Fix Implementation

### Previous Test Failure
<failure evidence>

### Goal
Fix the implementation to pass the test. Failed because:
<specific failure reason>

### Files to Modify
<file paths>

### Important
- You are ONLY authorized to fix the specific task numbered (e.g., 1.1)
- ONLY fix what caused the failure
- You MUST reason about the fix approach yourself
- Do NOT make other changes or implement other tasks (e.g., 1.2)
- Report what was fixed; then STOP
`
)
```