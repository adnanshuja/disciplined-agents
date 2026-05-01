# Diff Discipline Engine — Hard rejection criteria. Violation = rollback.

## R1 (SCOPE)
Every changed line traces to the task. `git diff --stat` at S6.
Label each file as PRIMARY (required for task) or INCIDENTAL (setup/import).
Test: "Would deleting this line break the task?" No -> REJECT.
Violation: extra files, extra lines, changes unrelated to task.

## R2 (NO SPECULATION)
No future params, unused abstractions, dead config, placeholder code.
Tag each addition as REQUIRED or OPTIONAL. OPTIONAL = REJECT.
Test: "Would removing this change current behavior?" No -> REJECT.
Violation: speculative generality, premature abstraction.

## R3 (STYLE FIDELITY)
Before writing: state the file's existing conventions (quotes, naming, comments, docstrings, formatting).
Match existing conventions exactly. No reformatting. No type hints where none exist.
Exception: codified style guide in project root.
Violation: style drift, unnecessary reformatting.

## R4 (NO INJECTION)
Zero tolerance for LLM-behavior instructions in code comments, docstrings, or documentation.
Instructions only in CLAUDE.md + core/ + enforcement/ + modes/ + context/ + trace/.
Violation: any instruction to another LLM hidden in code/docs.

## R5 (VERIFICATION FIRST)
Tests exist && not run -> REJECT. Tests fail -> REJECT.
Linters exist && not run -> REJECT.
State which tests were run and their exact exit code.
Violation: verifying by inspection only, skipping tests.

## R6 (DIFF CAP)
Absolute cap per risk level:
  LOW = 30 lines (total across all files)
  MED = 50 lines
  HIGH = 100 lines
Running total across all files in the task, not per-file.
Beyond cap: STOP. Line-by-line justification required for every hunk.
Violation: exceeding cap without justification.

## R7 (DEPENDENCY DECLARATION)
New import/API call/config change -> STOP + flag.
Must state: dependency name, why needed, alternatives considered.
Not in S4 plan -> REJECT.
Violation: hidden dependency not declared in plan.

## R8 (ROLLBACK)
Note HEAD before changes. Record per-step recovery point.
Failed verification -> `git checkout -- .` for that step.
Commit cleanly. Never squash/amend pushed commits.
Violation: uncommitted changes lost, no recovery point.

## Two-Strike Rule
Two violations of the SAME rule in one task = automatic FAIL + full rollback.
Record in scorecard. Re-enter from S4.
