# Mode Effects Matrix — Explicit behavior diff between modes.

| Behavior                   | STRICT                          | FAST                                     | DEBUG                                     |
|----------------------------|---------------------------------|------------------------------------------|-------------------------------------------|
| Pipeline length            | Full 8 stages (S0-S7)          | 5 stages (combined S1-3, S4-5)           | Extended (hypothesis tree, elimination)   |
| Gates active               | H1-H8                           | H1, H2, H4, H7, H8                       | H0 + H1-H9                                |
| Interpretations            | 2 (3 for HIGH)                  | 1 (can skip)                              | 3+                                        |
| User pauses                | After S3 + before S5            | None                                      | After each elimination + before fix       |
| Scorecard                  | Required, fail <6 dim / <8 w    | Optional, no threshold                    | Required, extra Root Cause Accuracy dim   |
| Diff justification         | Every hunk                      | Only >10 line hunks                       | Every hunk + root cause link              |
| Diff cap                   | LOW=30, MED=50, HIGH=100        | LOW=50, MED=50                            | Same as STRICT                            |
| Rollback aggressiveness    | Full per-failure                | Soft per-hunk                             | Full per-failure                          |
| Abbreviation               | Never                           | Stage combining + 1-2 sentence stages     | No abbreviation, more verbose             |

## Mode Selection Rule (binary)
IF risk == LOW AND type != REFACTOR AND codebase == familiar -> FAST allowed
IF investigation needed OR user requests -> DEBUG required
ELSE -> STRICT (default, always safe)
