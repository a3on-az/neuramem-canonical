---
id: KC-OPS-LOOP-001
title: Loop Engineering for NeuraMem Agent Workflows
type: knowledge_chip
status: accepted_operational_doctrine
created: 2026-06-26
source: Loop-Engineering-IEEE.pdf
notion: https://app.notion.com/p/38ba3f56e3a981239b65ec0369493f72
linked_context_stack: NM-CTXSTACK-SPEC-001
---

# KC-OPS-LOOP-001 · Loop Engineering for NeuraMem Agent Workflows

## Summary

Loop engineering is the discipline of designing systems that repeatedly prompt, verify, persist, and reschedule agent work. For NeuraMem, the concept is accepted as operational doctrine: loops should become governed runtime objects, not a replacement for the Context Stack memory ontology.

## Core Decision

Adopt a **Governed Loop Layer** above harness execution.

This layer does not replace L0-L3. It governs recurring or autonomous agent operation across Context Stack, RunLedger, Codex, Claude, SME, AutoResearch, and CanonOps workflows.

## Core Claim

Loops sit above harnesses. A loop discovers work, hands it off, verifies it, persists state, and schedules the next turn. Its value is not autonomous generation; its value is governed repetition with an independent capacity to say no.

## NeuraMem Implication

NeuraMem should treat loops as first-class governed runtime objects, logged through RunLedger, bounded by Context Stack policy, and prevented from mutating canon without independent evaluation and HITL or policy gates.

## Adopt

- Five-move loop model: discovery, handoff, verification, persistence, scheduling.
- Six-part loop skeleton: automation, worktrees/sandboxes, skills, connectors, sub-agents, memory.
- Generator/evaluator separation.
- Persistent loop state outside the context window.
- Hard token, retry, and budget caps.
- Human checkpoint as a permanent control surface.

## Modify

- RunLedger: add `loop_turn` event schema.
- Context Stack: add Governed Loop Layer above harness execution.
- SME KC schema: add `loop_applicability` metadata.
- CanonOps: forbid self-approval loops.
- AutoResearch: keep L2-only mutation boundary; do not let loops mutate L1 canon, governance, policy, privacy zones, or witness routing.

## Reject / Defer

- Treating loop engineering as a new NeuraMem ontology layer.
- Autonomous canon mutation.
- Evaluator-less scheduled agents.
- Scaling parallelism before evaluator proof.
- Removing human review once a loop is trusted.

## Failure Modes

- Nodding loop: verification skipped; generator self-approves.
- Amnesiac loop: persistence skipped; state lives only in chat.
- Manual loop: scheduling skipped; human still remembers to run it.
- Blind loop: discovery skipped; human still chooses all work.
- Tangled loop: handoff isolation skipped; agents collide.
- Verification debt: output accumulates between runs and right.
- Comprehension rot: loop changes exceed operator understanding.
- Cognitive surrender: human stops retaining judgment.
- Token blowout: runaway retries or helper spawning consume budget.

## Governed Loop Object

```yaml
loop:
  loop_id: string
  name: string
  owner: string
  maturity: candidate|pilot|bounded|trusted|retired
  autonomy_level: manual|assistive|bounded|autonomous
  mutable_surfaces:
    allowed: []
    forbidden: []
  required_moves:
    discovery: true
    handoff: true
    verification: true
    persistence: true
    scheduling: true
  required_controls:
    independent_evaluator: true
    budget_cap: true
    retry_cap: true
    human_checkpoint: true
    ledger_snap: true
```

## RunLedger `loop_turn` Event Schema

```yaml
loop_turn:
  loop_id: string
  turn_id: string
  trigger:
    type: schedule|event|manual|condition
    fired_at: ISO8601
  discovery:
    sources_read: []
    candidate_items: []
    selected_items: []
    skipped_items: []
  handoff:
    task_packet_id: string
    worktree_or_sandbox: string
    assigned_generator: string
  verification:
    evaluator_id: string
    evaluator_model: string
    checks_run: []
    verdict: pass|reject|needs_human|blocked
    rejection_reasons: []
  persistence:
    state_written_to: []
    ledger_event_ids: []
    canonical_write_attempted: false
  scheduling:
    next_run_at: ISO8601|null
    retry_count: 0
  controls:
    token_spend_estimate: 0
    budget_remaining: 0
    human_checkpoint_required: true
    stop_reason: string|null
```

## SME KC Extension

```json
{
  "loop_applicability": {
    "can_be_automated": true,
    "safe_autonomy_level": "assistive|bounded|autonomous",
    "required_evaluator": "string",
    "human_checkpoint": "before_canonical_write",
    "failure_modes": ["nodding_loop", "amnesiac_loop", "token_blowout"]
  }
}
```

## First Test Loop

```yaml
id: LOOP-RUNLEDGER-001
purpose: daily repo/context triage
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
  - independent evaluator rejects at least one seeded bad output
  - state persists across three turns
  - budget cap stops runaway retry
  - human can inspect and explain sampled outputs
```

## QC Gates

- Gate A — Intent fidelity: pass. Preserves the L0-L3 memory architecture and adds runtime governance only.
- Gate B — Operational utility: pass. Produces concrete schemas and first-loop test.
- Gate C — Safety/governance: pass. Forbids autonomous canon mutation and self-approval.
- Gate D — Traceability: pass. Source is linked to Loop-Engineering-IEEE.pdf and Context Stack spec.
- Gate E — Implementation readiness: partial. Direct RunLedger API snap still requires an available ledger write surface or MCP tool.
