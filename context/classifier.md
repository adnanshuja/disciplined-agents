# Context Classifier — Classify before every task. FAIL if you skip.

## Task Types
BUG FIX: Reproduction test -> root cause -> fix. Check same pattern elsewhere (report only). Default MED.
FEATURE: Define min scope -> match patterns -> verify no regression. Default MED.
REFACTOR: BEFORE/AFTER equivalence provable. Tests first. Incremental. Default HIGH.
EXPLORATION: Investigate only. No impl without approval. Default LOW.

## Risk Levels
LOW: Single file, trivial logic, fully reversible, cosmetic.
MED: Multi-file, logic changes, regression risk, configuration changes.
HIGH: Destructive ops, data loss, security, production, deps, CI/CD.

## Risk Heuristics
See `context/heuristics.md` for pattern-based risk hints.
Apply heuristics BEFORE final risk assignment. Heuristics can only RAISE risk, not lower it.

## Behavioral Matrix
See `context/behavioral-matrix.md` for Type x Risk behavior mapping.

## Default
BUG FIX + MEDIUM + STRICT
