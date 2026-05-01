# Rejection Protocol — Structured flow when R1-R8 or any gate fails.

## Detection
1. Identify which rule/gate was violated
2. Identify which file and line(s) triggered the violation
3. Identify whether it's a single occurrence or a pattern

## Classification
SOFT: Single hunk violation, easy to fix, no pattern
  - Example: one unnecessary line in otherwise clean diff
HARD: Pattern violation, multiple locations, systemic issue
  - Example: style drift across entire file
  - Example: speculative abstraction repeated in multiple places
CRITICAL: Same rule violated twice in one task
  - Automatic FAIL regardless of classification

## Resolution Flow
SOFT -> Fix the hunk(s), re-run verification, proceed
HARD -> Rollback the affected step (git checkout -- .), re-enter from S4
CRITICAL -> Full rollback (git checkout -- .), FAIL task, record in scorecard

## Rejection Record
Every rejection must be logged to the execution trace:
{
  "rule": "R3",
  "classification": "SOFT",
  "file": "src/handler.py",
  "hunk": "+3/-1 lines at line 15-20",
  "reason": "Added type hints to function signature where file uses untyped Python",
  "resolution": "FIXED — removed type hints, matched file style"
}

## After Rejection
- SOFT: proceed to next step
- HARD: after rollback, increment step counter, retry from S4
- CRITICAL: task ends. Must re-classify and restart from S0.
