# Execution Pipeline — Non-optional. All 8 stages in sequence. No skipping.

## S0: PARSE
Extract: task description, constraints, affected files, success criteria.
Reject ambiguous requests ("what does 'fix' mean?").
GATE H1: NO PROCEED without S0 parse complete. Ambiguous = FAIL.

## S1: ASSUMPTIONS
List every assumption. Min 3 bullets (scope, data, side effects, edge cases).
Each must be verifiable (provably true/false). Unverifiable -> STOP and ask.
Format: `ASSUMPTIONS:` then `- [assumption]` per bullet.
GATE H2: NO PROCEED if < 3 assumptions or any unverifiable without asking.

## S2: INTERPRETATIONS
2+ interpretations (3+ for HIGH risk or DEBUG mode).
Each: key difference vs alternatives, implication-if-correct, cost-if-wrong.
Include at least one interpretation agent considers unlikely (trap avoidance).
Format: `INTERPRETATIONS:` then `1. [A] 2. [B]`.
GATE H3: NO PROCEED with single interpretation. NO PROCEED if no cost-if-wrong.

## S3: DECISION + JUSTIFICATION
Select one interpretation. Explicitly reject each alternative with reasoning.
State confidence: HIGH | MED | LOW.
State what new information would change your decision.
Insufficient info -> return to S2.
Format: `DECISION: [A]` then `JUSTIFICATION:` then `CONFIDENCE: [HIGH|MED|LOW]`.
GATE H4: LOW confidence -> ask user. NO PROCEED if alternatives not rejected.

## S4: PLAN
Ordered steps with per-step verification criteria.
Each step: `[Action] -> VERIFY: [testable outcome]`.
Include backout step. Estimate line count and affected files.
Format: `PLAN:` then `1. [Action] -> VERIFY: [criteria]`.
GATE H5: Vague criteria -> rewrite. No backout -> FAIL. Plan overruns estimate -> STOP.

## S5: EXECUTION
One step at a time. Verify after each step before proceeding.
Per-change justification: every diff hunk needs WHY + RISK IF WRONG + ALTERNATIVE.
No side effects. No speculative code. No unrelated changes.
Format: `STEP [N]: [Action]` then `CHANGE: [hunk]` then `JUSTIFICATION: [why+risk+alt]` then `RESULT: [success|failure]` then `VERIFICATION: [evidence]`.
GATE H6: FAIL on failed verification without stop. FAIL on out-of-plan file. FAIL on speculative code.

## S6: VERIFICATION
Run all tests + linters. `git diff --stat` for unrelated changes. Check R1-R8.
Check edge cases (empty, error, boundary).
For bug fixes: confirm reproduction is gone.
Format: `VERIFICATION:` then test/linter/diff/enforcement/edge-case pass/fail.
GATE H7: FAIL on test failure. FAIL on unrelated changes. FAIL on skipped verification.

## S7: TRACE OUTPUT
Output structured execution trace (JSON format per trace/output-contract.md).
Include: assumptions, interpretations, decisions, changes, risks, scores.
Format: `TRACE:` then JSON block.
GATE H8: NO ACCEPT without complete trace. FAIL on incomplete trace.

## VIOLATIONS
State which stage + gate violated. Rollback (`git checkout -- .`).
Log to trace output. Re-enter from violated stage. Score in scorecard.
NO partial completion of stages.
