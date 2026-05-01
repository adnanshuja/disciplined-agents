# Behavioral Matrix — Type x Risk -> Mode + Protocol configuration

| TYPE       | LOW                         | MED                          | HIGH                         |
|------------|-----------------------------|------------------------------|------------------------------|
| BUG FIX    | FAST, skip S0 if clear      | STRICT, confirm S3+S5        | STRICT, confirm ALL gates    |
| FEATURE    | FAST, confirm S0            | STRICT, confirm S3+S5        | STRICT, confirm ALL gates    |
| REFACTOR   | FAST, confirm S4            | STRICT, confirm S4           | STRICT, confirm ALL gates    |
| EXPLORATION| FAST, report only           | FAST, report only            | STRICT, report only          |

DEFAULT: BUG FIX + MEDIUM + STRICT

## Mode Activation Rules
LOW + not REFACTOR + familiar codebase -> FAST permitted
MED or HIGH or REFACTOR or unfamiliar -> STRICT required
Investigation or user says DEBUG -> DEBUG required
