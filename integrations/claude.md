# Disciplined Agents v3 — Claude Code Integration

## Quick Start
```bash
git clone https://github.com/adnanshuja/agent-runtime.git disciplined-agents
```

Add to project CLAUDE.md:
```
See disciplined-agents/ for Disciplined Agents v3 execution framework.
```

## Activation Chain
1. CLASSIFY -> context/classifier.md (type + risk + heuristics)
2. MODE -> modes/[strict|fast|debug].md
3. PIPELINE -> core/pipeline.md (S0-S7, 8 stages)
4. GATES -> core/gates.md (H1-H8 hard gates)
5. ENFORCE -> enforcement/diff-discipline-engine.md (R1-R8)
6. JUSTIFY -> enforcement/per-change-justification.md
7. REJECT -> enforcement/rejection-protocol.md
8. TRACE -> trace/output-contract.md
9. SCORE -> core/scorecard.md (C/M/A/U/V)

## Trigger
"Use Disciplined Agents in STRICT mode."
"Use Disciplined Agents in FAST mode — typo fix."
"Use Disciplined Agents in DEBUG mode — intermittent 500 error."

## Plugin
.claude/plugins/disciplined-agents/ — highest priority, trigger-activated, pre/post-task hooks.
