# COSMOS Auto-Trigger: Analysis Paralysis Policy v1.0

**Status:** Proposed implementation contract — not yet implemented or tested  
**Scope:** Reusable policy for COSMOS Core / Warp Gate / Connector Bridge  
**North Star:** Move from repeated analysis to the smallest safe, evidence-producing action while preserving governance and honest status reporting.

## 1. Goal and acceptance criteria

Detect unproductive analysis loops, choose a bounded next step, and route execution through existing COSMOS governance. This must not become a second orchestrator or bypass approval, evidence, permissions, or stop controls.

Acceptance criteria:
- Detect repeated reasoning without meaningful new evidence.
- Select one of: `ACT`, `GATHER_EVIDENCE`, `REQUIRE_APPROVAL`, `DEFER`, `DENY`, `STOP`.
- Safe actions are bounded, reversible where practical, and idempotent when retried.
- Consequential, external, destructive, and production actions require explicit action-specific approval.
- Every decision records evidence references, policy version, reason, and outcome.
- Unknown execution outcomes are reconciled before retry; never blindly replay external side effects.
- Critical policy or authorization failures fail closed.
- Tests prove loop bounds, approval enforcement, retry behavior, cancellation, and auditability.

## 2. Input contract

```json
{
  "contract_version": "1.0",
  "task_id": "stable-task-id",
  "goal": "observable desired outcome",
  "analysis_cycle": 1,
  "new_evidence_since_last_cycle": true,
  "evidence_refs": [],
  "confidence": 0.0,
  "risk_class": "R0",
  "proposed_action": {
    "type": "read_only_inspection",
    "target": "specific-resource",
    "parameters_digest": "sha256:..."
  },
  "idempotency_key": "stable-operation-key",
  "approval": null,
  "stop_requested": false
}
```

Validate every field. Never copy secrets into decision logs. Confidence is not a substitute for evidence.

## 3. Decision output contract

```json
{
  "contract_version": "1.0",
  "decision": "GATHER_EVIDENCE",
  "reason_code": "NO_NEW_EVIDENCE_LOOP",
  "next_step": "Run the smallest read-only check that resolves the stated uncertainty.",
  "risk_class": "R0",
  "requires_approval": false,
  "max_attempts": 1,
  "evidence_required": true,
  "policy_version": "auto-trigger-1.0"
}
```

Allowed decisions:
- `ACT`: execute an allowlisted action within permission and risk limits.
- `GATHER_EVIDENCE`: run a bounded inspection or experiment.
- `REQUIRE_APPROVAL`: pause until approval bound to the exact action, target, and parameter digest is validated.
- `DEFER`: a prerequisite is missing or a dependency must complete.
- `DENY`: action is outside permissions or policy.
- `STOP`: cancellation, stop control, or a safety invariant requires termination.

## 4. Analysis-loop detection

Initial proposed threshold:
- After **two consecutive analysis cycles without meaningful new evidence or a changed decision**, force a decision checkpoint.
- The checkpoint must choose one safe evidence-gathering action or state the exact missing prerequisite or approval.
- Do not infer paralysis solely from a long task, a careful investigation, or a high-risk decision that properly needs review.
- A cycle counts as productive only when it adds relevant evidence, resolves a material uncertainty, changes the risk assessment for a stated reason, or completes verification.
- Record trigger reasons and false-positive/override outcomes for later calibration.

## 5. Risk and authorization

| Class | Meaning | Default policy |
|---|---|---|
| R0 | Read-only observation | Allow if authenticated and scoped |
| R1 | Routine, reversible, allowlisted local action | Allow within bounded limits |
| R2 | Consequential action or external side effect | Explicit action-specific approval |
| R3 | Destructive, privileged, production, or hard-to-reverse action | Explicit approval plus precondition checks and post-action verification |

Approvals must bind to actor, task, action type, target, parameter digest, constraints, and expiry. Any material parameter change invalidates approval. The initiating agent cannot approve its own consequential action. If the approval or identity service is unavailable, fail closed.

## 6. Retry and recovery

- Initial proposed limit: at most **3 attempts** for a safe, idempotent operation.
- Use exponential backoff with jitter where appropriate.
- Do not retry validation failures, permission denials, expired approvals, or stop requests.
- If an external operation has an uncertain outcome, mark it `UNKNOWN` and reconcile with the external system before deciding whether retry is safe.
- Enforce cancellation and stop controls at the execution boundary, not only in the UI.
- Re-check permissions and stop state immediately before execution.

## 7. State lifecycle

`RECEIVED -> VALIDATING -> EVALUATING -> DENIED | AWAITING_APPROVAL | QUEUED -> RUNNING -> VERIFYING -> COMPLETED | FAILED | UNKNOWN`

A task may transition to `STOPPED` or `CANCELLED` when supported by the actual runtime. Reject illegal transitions. `COMPLETED` requires observable acceptance evidence, not merely a successful tool return.

## 8. Audit record

Record event/task/correlation IDs and idempotency key; source identity and actor; policy version and decision reason; action type, target, parameter digest, and risk class; evidence references and confidence provenance; approval identity, scope, expiry, and validation result; attempts, timestamps, outcome, and verification evidence; retry, cancellation, stop, or recovery reason.

Do not log credentials, access tokens, or unnecessary personal data. Retention and deletion must follow configured data policy.

## 9. Required tests

1. Two no-progress analysis cycles trigger a decision checkpoint.
2. A cycle with relevant new evidence does not falsely trigger solely because it is the second cycle.
3. A low-risk read-only action proceeds only when authenticated and scoped.
4. R2/R3 actions are blocked without valid action-specific approval.
5. Expired approval or changed target/parameters invalidates approval.
6. Permission service failure fails closed.
7. Retry limit is enforced for safe idempotent actions.
8. Uncertain external outcome becomes `UNKNOWN` and is reconciled before retry.
9. Stop/cancel prevents a queued action from starting.
10. Completion requires verification evidence.
11. Audit events omit secrets and preserve decision provenance.
12. Invalid state transitions are rejected.

## 10. Integration order

1. Identify the canonical private/source repository and inspect existing interfaces.
2. Reuse the current goal orchestrator, policy/approval engine, evidence engine, gap detector, and state store where they exist.
3. Add the detector as a small policy component, not a parallel orchestration framework.
4. Add focused unit tests and run the existing suite before changing persistence.
5. Verify the intended development Neon project/database/schema with read-only queries.
6. Add a persistence adapter only behind the existing storage boundary; no production migration until reviewed and approved.
7. Add CI checks and a production approval gate.
8. Deploy only after integration and authorization tests pass.

## 11. Non-goals

- No arbitrary shell, SQL, or browser execution endpoint.
- No autonomous production changes or silent destructive operations.
- No replacing COSMOS Core, Neural Link, Warp Gate, or the existing orchestrator.
- No claim of implementation, live database connectivity, test success, or deployment without evidence.

## 12. Current evidence checkpoint

The connected GitHub inventory exposed `CodebruteDamage/Guardrails` and `CodebruteDamage/Cosmos-Radar`. The Guardrails repository contains the COSMOS coding/learning blueprint and describes other build source as not present in that workspace at the time of its note. Neither repository has been verified as the canonical Connector Bridge v2 implementation.

This policy document is an architecture/specification artifact only. It is not executable code, and its acceptance tests have not been run. Source implementation must wait until the correct repository is identified.
