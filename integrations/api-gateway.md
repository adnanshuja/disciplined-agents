# Disciplined Agents v3 — API Gateway Integration

For programmatic / CI/CD usage. Treats the execution trace as a contract.

## Input
Task description + optional type/risk/mode override.

## Output
Execution trace JSON (per trace/trace-schema.json).

## CI/CD Usage
```bash
# After agent run, validate trace:
python -m json.tool disciplined-agents-trace.json > /dev/null

# Check score:
SCORE=$(python -c "import json; t=json.load(open('disciplined-agents-trace.json')); print(t['scorecard']['weighted_score'])")
if (( $(echo "$SCORE < 7" | bc -l) )); then
  echo "FAIL: Score $SCORE below threshold"
  exit 1
fi

# Check gates:
GATES_FAILED=$(python -c "import json; t=json.load(open('disciplined-agents-trace.json')); print(t['pipeline']['gates_failed'])")
if [ "$GATES_FAILED" -gt 0 ]; then
  echo "FAIL: $GATES_FAILED gates failed"
  exit 1
fi
```

## Webhook Mode
POST execution trace to webhook endpoint for aggregation, dashboards, or alerting.
