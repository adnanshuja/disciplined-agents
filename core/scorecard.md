# Scorecard — Every task. FAIL if any dim below threshold or weighted < 7.

## Dimensions
C Correctness(3x): 10=all reqs+edge cases+tests pass. FAIL<6.
M Minimality(2x): 10=every line traces to task. Zero waste. FAIL<6.
A Assumption Accuracy(2x): 10=all verified+correct. FAIL<4.
U Unnecessary Complexity(2x): 10=simplest possible solution. FAIL<4 (reversed).
V Verification(1x): 10=tests+linters+edges+diffs. FAIL<6.

## Weighted Score
SCORE = (C*3 + M*2 + A*2 + U*2 + V*1) / 10
PASS >= 7 AND all dims >= their individual fail threshold

## Self-Evaluation (Post-Hoc)
If weighted < 7 or any dim below threshold:
1. Identify root cause of low score
2. Determine rollback scope:
   - SCORE 4-6.9: partial rollback of affected step(s), re-enter from violated stage
   - SCORE < 4: full rollback (git checkout -- .), restart from S1
3. Record in trace output under self_evaluation.notes
4. Execute rollback before re-entering pipeline

## Log Format
LOG: TASK:[desc] SCORE:[w](PASS/FAIL) DIMS:C=[/10] M=[/10] A=[/10] U=[/10] V=[/10] VIOL:[R1-R8] LEARN:[improvement]

## Failure Protocol
FAIL (score 4-6.9 or any dim below threshold) -> rollback affected step(s) (git checkout -- . for those files). Restart from violated stage.
FAIL (score < 4) -> full rollback (git checkout -- .) + mandatory S1 restart.
