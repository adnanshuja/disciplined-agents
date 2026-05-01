# Disciplined Agents v3.0.0 — Execution Discipline System for LLM coding agents.

Not prompts. Not guidelines. An enforcement runtime. Built for Claude Code, Cursor, GPT/Codex.

## Problems Solved
Assumption-driven development | Overengineering | Diff contamination | Verification skipping | Scope creep | Untraceable execution

## Components
8-Stage Pipeline (S0-S7) | Hard Gates (H1-H8) | 3 Modes (STRICT/FAST/DEBUG) | Enhanced Diff Discipline (R1-R8) | Per-Change Justification | Rejection Protocol | Context Classifier + Heuristics | Execution Trace (JSON) | Self-Evaluation Scorecard (C/M/A/U/V)

## Quick Start
Claude Code: `git clone https://github.com/adnanshuja/agent-runtime.git disciplined-agents`. Prompt: "Use Disciplined Agents in STRICT mode."
Cursor: Copy `.cursor/rules/llm-execution-protocol.mdc` into your project.
Any LLM: Copy CLAUDE.md + core/pipeline.md + enforcement/diff-discipline-engine.md into system prompt.

## Structure
```
/CLAUDE.md                     entry point
/core/pipeline.md              8 stages (S0-S7)
/core/gates.md                 H1-H8 hard constraints
/core/contracts.md             output formatting contract
/core/scorecard.md             C/M/A/U/V scoring
/context/classifier.md         type+risk+heuristics classification
/context/behavioral-matrix.md  Type x Risk -> behavior
/context/heuristics.md         pattern-based risk hints
/enforcement/diff-discipline-engine.md  R1-R8 rejection rules
/enforcement/per-change-justification.md  justification per diff hunk
/enforcement/rejection-protocol.md  structured rejection flow
/modes/                        strict | fast | debug | mode-effects
/trace/                        JSON schema | output contract | example
/examples/                     bug-fix | refactor | diff gallery | full trace
/integrations/                 claude | cursor | chatgpt | vscode | antigravity | api-gateway
/.claude/plugins/disciplined-agents/  plugin (highest priority, trigger-activated)
```

## Mode Selection
STRICT — Full 8 stages, all gates, per-hunk justification. Default for MED/HIGH.
FAST — Abbreviated, combined stages. LOW risk only.
DEBUG — Extended investigation with hypothesis tree + elimination. Root cause required.

## License: MIT
