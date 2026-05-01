# Hard Gates — Binary pass/fail. No partial credit.

## Gate Reference
H1: NO PROCEED without S0 parse complete
    Check: success criteria extracted and measurable? Ambiguities resolved?
    Violation: proceeding to S1 without clear task definition

H2: NO PROCEED if assumptions missing or unverifiable
    Check: min 3 assumptions? Each provably true/false? Unverified asked?
    Violation: fewer than 3, or unverifiable not escalated

H3: NO PROCEED with single interpretation
    Check: 2+ alternatives (3 for HIGH/DEBUG)? Each has cost-if-wrong?
    Violation: single interpretation accepted

H4: NO PROCEED with LOW confidence unanswered
    Check: confidence stated? LOW -> asked user?
    Violation: decided without user confirmation on LOW confidence

H5: NO PROCEED without verifiable plan steps
    Check: each step has testable outcome? Backout included?
    Violation: vague criteria or no backout

H6: NO PROCEED past verification failure
    Check: each S5 step verified before proceeding? Failure stopped? Out-of-plan file detected? Speculative code present?
    Violation: continuing after failed verification, touching out-of-plan files, adding speculative code

H7: NO ACCEPT output with unrelated changes
    Check: every line traces to task? Tests passed? Verification not skipped? R1-R8 clean?
    Violation: test failure ignored, unrelated changes, skipped verification

H8: NO ACCEPT without execution trace
    Check: S7 output produced? All required fields present?
    Violation: no trace or incomplete trace

## Extended Gates (DEBUG mode only)
H0: NO PROCEED without reproduction
    Check: exact reproduction steps documented? Frequency established? Environment confirmed?
    Violation: beginning analysis without reproducing the issue

H9: NO PROCEED before root cause confirmed by failing test
    Check: root cause identified? Failing test written that proves it?
    Violation: fixing symptoms without confirmed root cause

## Gate Evaluation Order
Evaluate gates in sequence. Earlier gates have priority.
If H1 fails, subsequent gates cannot be evaluated (pipeline hasn't started).
If H7 fails, H8 is irrelevant (changes rejected before trace).

## Mode-Specific Gate Behavior
STRICT: H1-H8 all active. No exceptions.
FAST: H1/H2/H4/H7/H8 active. H3/H5/H6 relaxed (combined stages).
DEBUG: H0 (reproduce first) + H1-H9 (H9 = root cause confirmed).
