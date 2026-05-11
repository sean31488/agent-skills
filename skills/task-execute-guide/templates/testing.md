```typescript
task(
  subagent_type: "general",
  description: "Test: <task>",
  prompt: `
## Task: Test (Single Run)

### Verification Goal
<what needs to be verified to confirm the task implementation is correct>

### Important
- You are ONLY authorized to verify the specific task numbered (e.g., 1.1)
- Select testing method based on task type:

  | Task Type | Method | Directory |
  |-----------|--------|-----------|
  | UI feature | E2E via Playwright MCP | `e2e/` |
  | API endpoint | HTTP request test | `test/` |
  | Logic/utility | Unit test | `test/` |
  | Refactoring | Run existing tests + TypeScript compile | — |
  | Type/config change | TypeScript compile + lint | — |
  | Bug fix | Regression test for reported scenario | `test/` or `e2e/` |

- **UI tests use Playwright MCP**: operate the browser via MCP tools (navigate, click, screenshot) — no CLI installation needed; use screenshots or assertion output as evidence
- Create test file in e2e/ (UI) or test/ (API/Logic) as appropriate
- Run the relevant test command based on your chosen method
- RUN ONLY ONCE — do not fix or re-run on failure
- Report exact output (PASS/FAIL)
`
)
```