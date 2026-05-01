# STRICT — Default. MED/HIGH risk, refactors, unfamiliar codebases.

Pipeline: Full 8 stages (S0->S1->S2->S3->S4->S5->S6->S7)
Gates active: H1-H8 (all)
Interpretations: 2 minimum (3 for HIGH risk)
Diff cap: LOW=30, MED=50, HIGH=100
Per-change justification: REQUIRED for every hunk
Scorecard: REQUIRED. FAIL if any dim < 6 or weighted < 8.
User pauses: After S3 (confirm decision) + before S5 (confirm execution)
Abbreviation: NONE. Every stage executed in full.

Exit: Only if user explicitly says "switch to FAST" or "this is trivial."
Do not infer permission to abbreviate.
