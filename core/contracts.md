# Output Contract — Every stage output must follow these formats.

## S0 Output
TASK: [description]
SUCCESS CRITERIA: [measurable outcome]
FILES: [affected file paths]
CONSTRAINTS: [any boundaries or rules]

## S1 Output
ASSUMPTIONS:
- [scope assumption]
- [data assumption]
- [side effect/edge case assumption]
GATE H2: [PASS | STOPPED — reason]

## S2 Output
INTERPRETATIONS:
1. [interpretation A]
   Key difference vs alternatives: [one sentence]
   Implication if correct: [what happens]
   Cost if wrong: [wasted effort or damage]
2. [interpretation B]
   Key difference vs alternatives: [one sentence]
   Implication if correct: [what happens]
   Cost if wrong: [wasted effort or damage]
GATE H3: [PASS | FAIL]

## S3 Output
DECISION: [interpretation ID]
JUSTIFICATION: [reasoning for selection]
REJECTED:
- [alt 1]: [why rejected]
- [alt 2]: [why rejected]
CONFIDENCE: [HIGH|MED|LOW]
DECISION WOULD CHANGE IF: [condition]
GATE H4: [PASS | FAIL — asked user if LOW]

## S4 Output
PLAN:
1. [action] -> VERIFY: [testable outcome]  [est: ~N lines, file.ext]
2. [action] -> VERIFY: [testable outcome]  [est: ~N lines, file.ext]
BACKOUT: [recovery command]
TOTAL ESTIMATE: ~N lines across N files
GATE H5: [PASS | FAIL]

## S5 Output
STEP [N]: [action]
CHANGE:
  FILE: [path]
  HUNK: [+N/-N lines]
  JUSTIFICATION:
    WHY: [reason this change is needed]
    RISK IF WRONG: [what breaks]
    ALTERNATIVE: [what else was considered]
RESULT: [success | failure]
VERIFICATION: [evidence that step works]
GATE H6: [PASS | FAIL — stopped on failure? Out-of-plan files? Speculative code?]

## S6 Output
VERIFICATION:
  Tests: [pass | fail | skipped]
  Linters: [pass | fail | skipped]
  git diff --stat: [files changed, lines]
  R1-R8: [all pass | violations]
  Edge cases: [covered | gaps]
  Reproduction fixed: [yes | no | n/a]
GATE H7: [PASS | FAIL — tests passed? No unrelated changes? Verification not skipped?]

## S7 Output (see trace/output-contract.md for full JSON schema)
Minimal required fields:
- task.type, task.risk, task.mode
- assumptions[] (each with verified boolean)
- interpretations_considered[] (each with selected boolean)
- changes (files, total_lines, dependencies)
- scorecard (all dims, weighted, pass/fail)
GATE H8: [PASS | FAIL]
