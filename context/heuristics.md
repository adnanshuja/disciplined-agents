# Risk Heuristics — Pattern-based risk hints. Applied before final risk assignment.

## File-Name Heuristics
File path matches any of these patterns -> risk HIGH:
- *migration*, *seed*, *destroy*, *drop*, *delete*
- *rollback*, *revert*, *reset*, *purge*, *truncate*

## Domain Heuristics
Diff touches any of these domains -> risk HIGH:
- database, auth, authentication, payment, billing
- security, encryption, PII, GDPR, compliance
- production config, deployment, CI/CD, secrets

## Dependency Heuristics
New non-stdlib import introduced -> risk at least MEDIUM

## Scope Heuristics
2+ files in diff -> at least MEDIUM risk
5+ files in diff -> at least HIGH risk

## Risk-Lowering Heuristics
Only comments, whitespace, documentation, tests, config defaults -> LOW candidate
Single file, trivial logic, fully reversible -> LOW candidate

## Rules
Heuristics can only RAISE risk, never lower it.
Final risk = max(heuristic_risk, initial_risk).
