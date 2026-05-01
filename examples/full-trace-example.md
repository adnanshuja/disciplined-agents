# Full Execution Trace Example — Complete run from S0 to S7 with trace output.

**Scenario:** "Fix the registration form — error messages don't show when email is invalid."

## CLASSIFY
Type: BUG FIX
Risk: MEDIUM (touches submission flow, regression possible)
Mode: STRICT

Heuristics applied:
- 2 files? No, single file — LOW candidate
- New deps? No
- Domain: form handling — MEDIUM default
Final: MEDIUM -> STRICT

## S0: PARSE
TASK: Fix registration form error display
SUCCESS CRITERIA: Invalid email shows error message on form. Valid email submits normally.
FILES: src/handlers/registration.py (primary), tests/test_registration.py (incidental)
CONSTRAINTS: No restructuring. Match existing style. No new dependencies.
GATE H1: PASS

## S1: ASSUMPTIONS
ASSUMPTIONS:
- Error validation logic runs (the check works, display is broken) — verified: code inspection shows validation returning errors with 200
- Bug is server-side, not client-side JS — verified: confirmed form submits and response returns
- Fix is limited to response handler, not validation logic — verified: validation function is correct
GATE H2: PASS

## S2: INTERPRETATIONS
INTERPRETATIONS:
1. Response format mismatch — validation returns errors but with 200 status; frontend only reads 422
   Key difference: fix is in status code, not logic
   Cost if wrong: wasted time checking API contract (10 min)

2. Validation doesn't run before submit handler processes data
   Key difference: fix is in execution flow
   Cost if wrong: would trace entire request path (30 min wasted)

3. (Trap) Email validation regex is wrong
   Key difference: would require logic change
   Cost if wrong: debugging correct code (15 min wasted)
GATE H3: PASS

## S3: DECISION
DECISION: Interpretation 1 (response format mismatch)
JUSTIFICATION: Frontend reads 422 for errors. Server returns 200 with errors in body.
  The validation works — the display protocol is wrong.
REJECTED:
- Interpretation 2: Form submits successfully, it just shows no error display
- Interpretation 3: Email check logic is correct (tested independently)
CONFIDENCE: HIGH — confirmed by reading handler code
GATE H4: PASS

## S4: PLAN
PLAN:
1. Write reproduction test: submit invalid email -> observe 200 with errors in body
   -> VERIFY: test fails (expects 422, gets 200) — est 10 lines, test file
2. Change response handler to return 422 when errors present
   -> VERIFY: reproduction test now passes — est 3 lines, handler file
3. Confirm valid-submission flow still returns 200
   -> VERIFY: success test passes — est 0 lines
4. Run full test suite + lint + diff check
   -> VERIFY: all green — est 0 lines
BACKOUT: git checkout -- src/handlers/registration.py
TOTAL ESTIMATE: ~13 lines across 2 files
GATE H5: PASS

## S5: EXECUTION

### Step 1: Write reproduction test
```python
def test_registration_invalid_email_returns_422():
    response = client.post("/register", data={"email": "bad", "password": "ok123"})
    assert response.status_code == 422
    assert "email" in response.json
```
CHANGE: +10 lines, tests/test_registration.py
JUSTIFICATION: Reproduction test proves current behavior (200) != expected (422)
RESULT: success
VERIFICATION: Test fails as expected (HTTP 200 vs expected 422) — correct reproduction
GATE H6: PASS

### Step 2: Fix response handler
Change: when validation errors exist, return 422 instead of 200
CHANGE: +3 lines, src/handlers/registration.py (HUNK: add status code to error response)
JUSTIFICATION:
  WHY: Frontend requires 422 to display error messages
  RISK IF WRONG: Non-422 errors still return 200 (unchanged behavior)
  ALTERNATIVE: Keeping 200 — rejected because frontend won't read errors
RESULT: success
VERIFICATION: Reproduction test now passes
GATE H6: PASS

### Step 3: Verify valid submission
VERIFICATION: Success test returns 200 — PASS
GATE H6: PASS

## S6: VERIFICATION
VERIFICATION:
  Tests: PASS (all 47 tests, exit code 0)
  Linters: PASS (flake8, mypy — no new warnings)
  git diff --stat: 2 files changed, 13 insertions
  R1-R8: ALL PASS
    R1: Every line traces to task (test + fix + verification)
    R2: No speculation (no future params or abstractions)
    R3: Style matches (no type hints, no docstrings — matches file)
    R4: No injection (zero LLM instructions in code)
    R5: Tests run (exit code 0)
    R6: 13 lines < 50 cap (MEDIUM)
    R7: No new deps
    R8: HEAD noted before changes
  Edge cases: Empty email, long email, XSS in email — all handled
  Reproduction fixed: CONFIRMED (test now expects 422, passes)
GATE H7: PASS

## S7: TRACE OUTPUT
```json
{
  "schema_version": "3.0.0",
  "task": {"description": "Fix registration form error display", "type": "BUG_FIX", "risk": "MEDIUM", "mode": "STRICT"},
  "pipeline": {"stages_completed": 8, "stages_skipped": 0, "gates_passed": 8, "gates_failed": 0},
  "assumptions": [
    {"text": "Error validation runs but response format mismatches", "verified": true, "source": "code_inspection"},
    {"text": "Bug is server-side", "verified": true, "source": "confirmed with user"}
  ],
  "interpretations_considered": [
    {"id": 1, "description": "Response format mismatch", "cost_if_wrong": "10 min wasted", "selected": true},
    {"id": 2, "description": "Validation doesn't run", "cost_if_wrong": "30 min wasted", "selected": false, "reason_rejected": "Form submits"}
  ],
  "changes": {
    "files_modified": ["src/handlers/registration.py", "tests/test_registration.py"],
    "total_lines": 13,
    "dependencies_added": 0,
    "per_change_justifications": [
      {"file": "tests/test_registration.py", "lines": "+10", "reason": "Reproduction test proving current behavior is wrong", "risk": "None"},
      {"file": "src/handlers/registration.py", "lines": "+3", "reason": "Change status code to 422 when validation errors present", "risk": "None — valid submission still returns 200"}
    ],
    "violations_detected": []
  },
  "risks": {
    "detected": [{"risk": "Valid submission regression", "mitigated": true, "evidence": "Success test passes"}],
    "unmitigated": []
  },
  "alternatives_skipped": [
    {"alternative": "Rewrite entire validation function", "reason_skipped": "Speculative overengineering"}
  ],
  "scorecard": {
    "correctness": 10, "minimality": 10, "assumption_accuracy": 10,
    "verification": 10, "weighted_score": 10.0, "passed": true
  },
  "self_evaluation": {
    "correctness": 10, "minimality": 10, "assumption_accuracy": 10,
    "unnecessary_complexity": 10,
    "notes": "Surgical fix. Test proved the bug. Three-line change fixed it. Zero waste."
  },
  "metadata": {
    "started_at": "2026-05-01T10:00:00Z", "completed_at": "2026-05-01T10:12:00Z",
    "total_steps": 3, "rollbacks": 0
  }
}
```
