---
id: NM-CTXSTACK-LOOP-001
title: Governed Loop Layer
type: context_stack_extension
status: draft_extension_accepted_doctrine
created: 2026-06-26
source_kc: KC-OPS-LOOP-001
notion_context_stack: https://app.notion.com/p/32ca3f56e3a981398fcde9c279323623
---

# NM-CTXSTACK-LOOP-001 · Governed Loop Layer

## Purpose

Define the operational layer that governs recurring and autonomous agent loops without changing the Context Stack L0-L3 memory architecture.

## Placement

The Governed Loop Layer sits above harness execution and below human/canon promotion authority.

It governs workflows that use:

- Context Stack
- MCP gateway
- RunLedger
- SME / Knowledge Chip pipeline
- AutoResearch
- CanonOps
- Codex / Claude execution planes

## Non-Replacement Rule

The layer does **not** replace:

- L0 hot control
- L1 canonical structured memory
- L2 derived retrieval fabric
- L3 immutable archive
- MCP as stateless gateway

## Definition

A governed loop is a runtime object that discovers work, hands it off, verifies it independently, persists state outside the context window, and schedules a next turn while preserving a permanent human or policy checkpoint.

## Binding Rules

- Loops may draft, retrieve, test, summarize, and propose.
- Loops may not self-approve.
- Loops may not auto-promote canon.
- Loops may not mutate governance, policy, privacy zones, witness routing, or canonical memory without explicit policy gate plus human review.
- Any canonical write requires independent evaluator pass, provenance check, and HITL or policy gate.
- Any loop that lacks verification, persistence, budget cap, or human checkpoint remains `candidate`, not `trusted`.

## Required Moves

1. Discovery — find work from bounded sources.
2. Handoff — isolate the task in a worktree, sandbox, queue item, or task packet.
3. Verification — use a separate evaluator that can reject.
4. Persistence — write state to RunLedger, repository, queue, or durable store.
5. Scheduling — trigger the next turn by schedule, event, or condition.

## Required Controls

```yaml
loop_governance:
  generator_cannot_self_approve: true
  evaluator_default_stance: skeptical_until_verified
  required_controls:
    - independent_evaluator
    - budget_cap
    - retry_cap
    - persistent_state
    - human_checkpoint
    - ledger_snap
  canonical_write_requires:
    - independent_evaluator_pass
    - provenance_check
    - HITL_or_policy_gate
```

## First Candidate Loop

```yaml
id: LOOP-RUNLEDGER-001
name: Daily repo/context triage
autonomy_level: bounded
writes_allowed:
  - ledger_snap
  - draft_task_packet
  - inbox_item
writes_forbidden:
  - canon_promotion
  - policy_change
  - destructive_repo_action
acceptance_gate:
  - evaluator rejects at least one seeded bad output
  - state persists across three turns
  - budget cap stops runaway retry
  - sampled outputs remain explainable by operator
```

## Failure Modes Guarded

- Nodding loop
- Amnesiac loop
- Manual loop
- Blind loop
- Tangled loop
- Verification debt
- Comprehension rot
- Cognitive surrender
- Token blowout

## Implementation Notes

1. Add `loop_turn` as a RunLedger event type.
2. Add `loop_applicability` to Knowledge Chip metadata.
3. Add Loop Registry entries for existing open loops: LOOP-001, LOOP-002, LOOP-003.
4. Pilot LOOP-RUNLEDGER-001 before any autonomous canon or implementation loop.
5. Do not scale parallel agents until evaluator rejection behavior is proven.
