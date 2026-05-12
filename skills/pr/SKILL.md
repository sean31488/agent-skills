---
name: pr
description: Use when user invokes /pr <key> to prepare a Jira/Slack update from current git changes.
---

# pr

## Overview

Generate one Traditional Chinese sentence covering staged and unstaged changes, then open the local Slack helper with `key` and `slackMessage`.

## When to Use

- Use when `/pr <key>` is requested for Jira/Slack update prep.
- Do not use for multi-paragraph changelogs or review reports.

## Steps

1. Run `rtk git diff --staged HEAD` and `rtk git diff`.
2. Write **exactly one** Traditional Chinese sentence summarizing all changes.
3. URL-encode the sentence.
4. Build `http://localhost:9000/?key=<KEY>&slackMessage=<encoded-message>`. KEY must be a plain integer (digits only, no prefix, suffix, or other characters).
5. Run `open "<URL>"` in default permissions.
6. If `open` fails due to sandbox/GUI opener constraints, rerun `open` with `sandbox_permissions: "require_escalated"` and a concise justification.
7. If retry fails, return the summary sentence, full URL, and key error reason.

## Quick Reference

- Input: `/pr <key>` where `<key>` is a plain integer (digits only)
- Output: one Traditional Chinese sentence
- URL params: `key`, `slackMessage` (encoded)
- Open policy: default `open` first, escalation retry mandatory on sandbox failure

## Common Mistakes

- Writing more than one sentence.
- Summarizing only staged or only unstaged diff.
- Changing query payload between normal and escalated attempts.
- Stopping after first `open` failure without escalation retry.
