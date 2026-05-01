# Per-Change Justification — Every diff hunk must be justified. Unjustified = rejection.

## Required Format
FILE: [path]
CHANGE [N]: [+N/-N lines at line X]
  WHY: [one sentence explaining why this change is required by the task]
  RISK IF WRONG: [what breaks or regresses if this change is incorrect]
  ALTERNATIVE: [what other approach was considered and rejected]

## Example
FILE: src/handlers/registration.py
CHANGE 1: +3 lines (error handling block at line 42)
  WHY: Task requires returning 422 on validation failure instead of 200 with errors
  RISK IF WRONG: Non-422 responses for other error types would be masked
  ALTERNATIVE: Returning 400 — rejected because frontend reads 422 specifically

CHANGE 2: +1 line (import added at line 5)
  WHY: jsonify is already imported in this file — this change adds the Response import
  RISK IF WRONG: None (stdlib, already used elsewhere in codebase)
  ALTERNATIVE: Using jsonify directly — rejected because Response gives explicit status code

## Enforcement
- Every hunk in the diff requires a justification block
- Hunks with no justification -> REJECT
- Justification must reference the task (not general improvement)
- "This is good practice" is NOT a valid justification
- Justification must identify risk — "None" only if truly zero risk

## Mode-Specific Rules
STRICT: every hunk requires justification
FAST: hunks under 10 lines can skip justification; 10+ lines must be justified
DEBUG: every hunk requires justification + must reference root cause
