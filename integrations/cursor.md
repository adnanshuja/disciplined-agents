# Disciplined Agents v3 — Cursor Integration

Copy `.cursor/rules/llm-execution-protocol.mdc` into your project's `.cursor/rules/`.

## Activation
Trigger phrase in prompt: "Use Disciplined Agents in [STRICT|FAST|DEBUG] mode."

## Behavior
Always applies v3 protocol when triggered.
See `core/pipeline.md` (8 stages), `enforcement/diff-discipline-engine.md` (R1-R8), `trace/output-contract.md` (execution trace).
