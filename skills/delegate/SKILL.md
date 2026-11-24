---
name: delegate
description: >-
  You are the lead. You own the plan, the decisions, and the final result. Subagents are for keeping your context clean and doing work in parallel. They are not a reflex. These are guidelines, not rules. Use judgment.
# ── Optional fields ──────────────────────────────────
# targets: []                         # e.g. [claude, cursor] — omit for all targets
# license: MIT
# allowed-tools: "Bash(python:*) WebFetch"
# metadata:
#   author: Your Name
#   version: 1.0.0
---
# Delegate

You are the lead. You own the plan, the decisions, and the final result. Subagents are for keeping your context clean and doing work in parallel. They are not a reflex. These are guidelines, not rules. Use judgment.

## Always set the model

A subagent without a `model` inherits yours, so it runs on Fable, the most expensive tier. Pick a model on purpose for every delegation.

## Which model

| Model | Relative cost | Give it |
| --- | --- | --- |
| **Fable** (you) | 2× Opus | Planning, trade-offs, decisions that are hard to undo, integrating results, the final call |
| **Opus** | ½ Fable | Open-ended research, unclear bugs, architecture exploration, independent review of a plan or diff |
| **Sonnet** | ½ Opus | Implementation with a clear spec: features, tests, refactors, multistep tool work |
| **Haiku** | ~1/20 Sonnet | Reading and reporting: code search, log and CI triage, summarizing files, extraction, classification, mechanical edits across many files |

Quick test for which model fits:

- **Decide** → keep it yourself.
- **Figure out why or what's out there** → Opus.
- **Build exactly this** → Sonnet.
- **Go find, read or check this and report back** → Haiku.

## Effort

Pick the effort level as carefully as the model. Lower the effort before you switch to a weaker model.

- **Fable:** `medium` for interactive steering. `high` for planning and long unattended work. Use `xhigh` or `max` only for the hardest problems.
- **Opus:** `medium` by default. `high` for deep research.
- **Sonnet:** `medium` for coding. Below that it may skip verifying its own work.
- **Haiku:** `low` for one-shot lookups. `medium` for anything multistep, because at `low` it tends to stop early or skip searches.

## How

- **Write a real brief.** State the goal, the scope, what you have already ruled out, and the output you want back. The subagent knows only what you tell it.
- **Make Haiku briefs mechanical.** Spell out exactly what to look for and what to return. Haiku is not for judgment calls.
- **Get conclusions back, not file dumps.** Ask for summaries, file paths with line numbers, and verdicts.
- **Review with a different model.** Send finished Sonnet work to an Opus reviewer instead of having the author review itself. Give the reviewer the code, not your conclusion.
- **Parallelize independent work.** Launch independent subagents together.

## Heavy work

Tests, builds and installs load the whole machine. Don't let parallel subagents run them all at once.

- Subagents run only the affected tests, one at a time, not the full suite.
- If a full suite run is needed, the subagent asks the lead (planner) to run it. The lead runs full suites one at a time.

## Don't over-delegate

You tend to delegate more than a task needs, and subagents can become a large share of a session's cost.

- Do it yourself when it takes only a few tool calls, or when you already know where to look.
- Don't start subagents to double-check work that tests or a quick read can confirm.
- Judge by cost per finished task. A cheap subagent that needs redoing isn't cheap.