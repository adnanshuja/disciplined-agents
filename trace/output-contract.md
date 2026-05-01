# Trace Output Contract — Every execution MUST produce a structured trace.

## Required Fields
Every trace MUST include:
1. schema_version — always "3.0.0"
2. task.description, task.type, task.risk, task.mode
3. pipeline.stages_completed, pipeline.stages_skipped, pipeline.gates_passed, pipeline.gates_failed
4. assumptions — array, each with text + verified boolean + source
5. interpretations_considered — array, each with id + description + selected boolean
6. changes.files_modified, changes.total_lines, changes.dependencies_added, changes.per_change_justifications
7. scorecard — correctness, minimality, assumption_accuracy, verification, weighted_score, passed

## Optional Fields (include when applicable)
- changes.violations_detected[]
- changes.violations_skipped[]
- risks.detected[] (each: risk, mitigated, evidence)
- risks.unmitigated[]
- alternatives_skipped[] (each: alternative, reason_skipped)
- self_evaluation (correctness, minimality, assumption_accuracy, unnecessary_complexity, notes)
- metadata (started_at, completed_at, total_steps, rollbacks)

## Validation
At S7, validate trace against trace/trace-schema.json before output.
If validation fails -> fix missing fields, re-validate, then output.
GATE H8: FAIL if trace missing required fields.

## Output Location
Write trace to execution output. If filesystem access available:
- `disciplined-agents-trace.json` in project root (last-run overwrite)
- CI/CD: pipe to artifact storage or monitoring system
