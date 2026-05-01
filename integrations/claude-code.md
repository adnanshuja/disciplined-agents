# Integration: Claude Code

Two methods: PLUGIN (global) or CLAUDE.md (per-project).

---

## Method 1: CLAUDE.md (Per-Project)

**For individual projects — copy the protocol into the repo.**

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/adnanshuja/agent-runtime/main/CLAUDE.md
```

Or download the full system:

```bash
git clone https://github.com/adnanshuja/agent-runtime.git .llm-protocol
echo "See .llm-protocol/CLAUDE.md for the LLM Execution Protocol system." >> CLAUDE.md
```

---

## Method 2: Skills Directory (Global)

**Applies to all projects via ~/.claude/skills/.**

Copy the skill definition:

```bash
cp -r skills/llm-execution-protocol ~/.claude/skills/
```

Then reference it as a skill:

```
Use skill llm-execution-protocol in STRICT mode.
```

---

## Method 3: Project CLAUDE.md Entry

**Add at the top of your project's CLAUDE.md:**

```markdown
# LLM Execution Protocol — ACTIVE

See https://github.com/adnanshuja/agent-runtime for the full system.
Core files: core/protocol.md, core/enforcement.md, context/classifier.md
```

---

## Verification

Confirm the system is active by asking:

```
Run classification on the current task.
```

If the system responds with task type + risk level → active.
If it just answers generically → not loaded.
