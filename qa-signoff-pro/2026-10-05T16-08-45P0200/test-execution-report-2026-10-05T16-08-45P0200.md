# Test Execution Report — 2026-10-05

Source: `qa-runs/tracking.csv`

**Release gate: ✅ PASS**

## Quality Gates

| Gate | Status | Value |
|------|--------|-------|
| Pass Rate ≥80% | ✅ | 100.0% |
| Needs Re-automation = 0 | ✅ | 0 |
| Orphaned Tests = 0 | ✅ | 0 |
| Missing From Run = 0 | ✅ | 0 |
| Misplaced Tests = 0 | ✅ | 0 |
| No regressions | ✅ | 0 |

## Fixed since last run

- TC-WEB-170 (user-iphone-13): failed in 2026-10-05T15-15-17P0200

## Needs work

### Summary

- **Passed partially** (2, `status` = `PASS_PARTIAL`) — does not block the gate; some tests of the test case were skipped, see Passed partially: TC-SEC-004 (no-login-desktop-chrome), TC-SEC-004 (no-login-iphone-13)

### Regressions

None

### Failed

None

### Flaky

None

### Passed partially

- TC-SEC-004 (no-login-desktop-chrome): when HTTPS is used, Strict-Transport-Security is present — skip: environment runs over HTTP
- TC-SEC-004 (no-login-iphone-13): when HTTPS is used, Strict-Transport-Security is present — skip: environment runs over HTTP

### Missing from run

None

## Execution

- Pass rate: 100.0%
- Total tracked: 98
- Executed (pass/fail): 98 (100.0%)
- Passed: 98
- Failed: 0
- Regressions (failed after passing): 0
- Flaky (passed on retry): 0
- Fixed since last run: 1

## Reconciliation

- Needs re-automation: 0
- Orphaned tests: 0
- Passed partially (some tests skipped): 2
- Missing from run: 0
- Misplaced tests: 0
- Untracked tests: 0
