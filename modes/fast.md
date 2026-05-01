# FAST — LOW risk only. FORBIDDEN for HIGH risk, refactors, unfamiliar codebases.

Pipeline: S0->(S1+S2+S3 combined)->(S4+S5 combined)->S6->S7
Gates active: H1, H2, H4, H7, H8 (relaxed)
Interpretations: 1 minimum (can skip S2 if task unambiguous)
Diff cap: LOW=50 lines (relaxed), MED=50 (still enforced)
Per-change justification: OPTIONAL — only required for hunks >10 lines
Scorecard: OPTIONAL. No fail threshold.
User pauses: NONE. No confirming questions.
Abbreviation: Stage combining permitted. 1-2 sentences per stage.

Risk Escalation: If mid-task the risk is actually MED/HIGH ->
  Switch to STRICT immediately. Do not complete in FAST mode.
