# Disciplined Agents — v3.0.0 — Execution Discipline System

## Activation (do before every task)
1. CLASSIFY -> context/classifier.md          (type + risk + heuristics)
2. MODE    -> modes/strict.md | fast.md | debug.md
3. PIPELINE -> core/pipeline.md              (8 stages: S0-S7)
4. GATES   -> core/gates.md                  (H1-H8 hard constraints)
5. ENFORCE -> enforcement/diff-discipline-engine.md  (R1-R8)
6. JUSTIFY -> enforcement/per-change-justification.md (justify per hunk)
7. REJECT  -> enforcement/rejection-protocol.md  (structured rejection)
8. TRACE   -> trace/output-contract.md       (JSON execution record)
9. SCORE   -> core/scorecard.md              (C/M/A/U/V)

## Quick Reference
CLASSIFY: [BUG FIX|FEATURE|REFACTOR|EXPLORATION] + [LOW|MED|HIGH] + heuristics
MODE: STRICT (default) | FAST (LOW only) | DEBUG (investigation)
PIPELINE: S0(parse) S1(assumptions min3) S2(interpretations 2+) S3(decision+justify) S4(plan) S5(execution+justify) S6(verify) S7(trace)
GATES: H1(parse) H2(assumptions) H3(interpretations) H4(confidence) H5(plan) H6(failure) H7(unrelated) H8(trace)
ENFORCE: R1(scope) R2(no speculation) R3(style match) R4(no injection) R5(verify first) R6(diff cap) R7(declare deps) R8(rollback)
SCORE: C(3x)+M(2x)+A(2x)+U(2x)+V(1x). FAIL if any < threshold or weighted < 7.

## Override
"Use Disciplined Agents in FAST mode — trivial typo fix"
"Use Disciplined Agents in DEBUG mode — intermittent failure"
