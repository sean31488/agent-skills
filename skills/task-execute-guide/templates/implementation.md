```typescript
task(
  subagent_type: "general",
  description: "Implement: <task>",
  prompt: `
## Task: Implement

### Goal
<what to achieve - describe objective, NOT implementation method>

### Files to Modify
<file paths>

### Important
- You are ONLY authorized to implement the specific task numbered (e.g., 1.1)
- ONLY implement to achieve the stated goal
- Do NOT implement 1.2 or any other tasks even if related or seems "efficient"
- You MUST reason about implementation approach yourself
- Run static code check after implementation
- Report what was implemented and check result, then STOP
`
)
```