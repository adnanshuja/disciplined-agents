# DEBUG — Bug investigation, performance, unexpected behavior.

Pipeline: Extended. S0->(reproduce)->(hypothesis tree)->S1->S2(3+) -> S2.5->S2.75->S3(deferred)->S3.5->S5->S6->S7

S0 REPRODUCE: Before any analysis. Exact steps+env+frequency.
  Cannot reproduce -> STOP. No speculative fixes.

S1: Add "what could NOT be the cause" to assumptions.

S2: 3+ interpretations. Include improbable but falsifiable ones.

S2.5 HYPOTHESIS TREE: Categories -> hypotheses.
  Each must be falsifiable (provably wrong).

S2.75 ELIMINATION: Probe each to prove it WRONG.
  Cross off eliminated. Report remaining confidence.

S3: Deferred. No commitment until evidence gathered.

S3.5 ROOT CAUSE: Confirm cause + evidence + why tests missed it.
  Create failing test that proves root cause.

S5: Fix root cause, NOT symptom. Fail test must pass.
  Per-change justification must reference root cause.

S6: Extended. Confirm reproduction gone. Benchmark if perf-related.

Gates: H0 (reproduce before analysis) + H1-H9 (H9: root cause confirmed by failing test)
Interpretations: 3+ minimum
Per-change justification: REQUIRED + must link to root cause
Scorecard: REQUIRED. Extra dimension: Root Cause Accuracy (0-10).
User pauses: Report after each elimination round. Confirm root cause before fix.
