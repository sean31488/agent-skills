---
name: task-design-guide
description: Use when scope is too large to start directly, when multiple pages or features need to be broken into ordered tasks, or when a task list must be presented to the user for review before execution begins
---

# Task Design Guide

## Overview

**Core principle:** One main task = one independently testable unit of work.

Task design precedes execution. A well-designed task list guarantees every main task has a standalone test without depending on other tasks completing first.

**SEE ALSO:** task-execute-guide

## When to Use

- Scope spans multiple pages or functional areas
- User will review the task list before execution begins
- Requirements need explicit acceptance criteria
- Tasks have dependencies that must be sequenced

**When NOT to use:**
- Single isolated change (no decomposition needed)
- Tasks are already defined by the user
- Purely exploratory or research work

## Vertical Slicing

Slice tasks by **complete feature path**, not by technical layer.

| ❌ Horizontal (wrong) | ✅ Vertical (correct) |
|----------------------|----------------------|
| Task 1: All DB schema | Task 1: Registration (schema + API + UI) |
| Task 2: All API endpoints | Task 2: Login (auth + API + UI) |
| Task 3: All UI components | Task 3: Profile edit (schema + API + UI) |

Each vertical slice is independently deployable and testable.

## Task Structure

### Main Task

Granularity rule: **one feature point or one page's complete modification** = one main task.

**Wrong:** Bundling unrelated concerns.
```
❌ Task 1: Update all three pages + API + database write
```
**Right:** Each main task is testable end-to-end without the others completing first.

### Task Sizing

| Size | Files | Action |
|------|-------|--------|
| S | 1–2 | — |
| M | 3–5 | — |
| L | 5–8 | Split if title contains "and" |
| XL | 8+ | Always split further |

Also split when: acceptance criteria exceed 3 items, or task crosses two independent subsystems.

### Sub-Tasks

Each main task decomposes into 2–5 sub-tasks covering implementation layers:

```
Task 1: Modify registration page
  1.1  Add form fields
  1.2  Add validation logic
  1.3  Implement form submission
  1.4  Update state management
```

Do not skip layers or merge them to "save steps."

### Checkpoint

Each main task has one checkpoint. **Design phase defines what to verify; execution phase fills in evidence.**

Checkpoint specifies:
- Test type: `e2e` / `integration` / `unit`
- Concrete scenarios (user actions or API calls)
- Acceptance criteria (what must pass)

See `templates/plan.md` for full output format.

## Dependency Ordering

**Execute the most-depended-on task first.** For each task, ask: does it produce data or state that another task needs?

```
Task 1 → Task 2 (needs Task 1 auth state) → Task 3 (needs both)
```

When testing a dependent task: fast-path through prerequisites to generate required state, then test normally. Prerequisite tasks remain independently testable.

## Quick Reference

| Design Decision | Rule |
|-----------------|------|
| Main task size | One independently testable feature or page |
| Slicing direction | Vertical (feature path), not horizontal (layer) |
| Sub-task count | 2–5, covering all implementation layers |
| Checkpoint | Criteria at design; evidence at execution |
| Test type priority | e2e first; fall back only if infeasible |
| Dependency order | Most-depended-on task executes first |
| Cross-task testing | Fast-path prerequisites, then test target |

## Common Mistakes

**Main task too large**
If you cannot test it without touching two unrelated pages, split it.

**Ignoring dependencies**
Running a dependent task before its prerequisite produces missing-data failures. Map dependencies before ordering.

**Sub-tasks that skip layers**
Merging "add field + add validation + implement submit" into one sub-task removes the ability to isolate failures.
