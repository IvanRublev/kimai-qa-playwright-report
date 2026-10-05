# Agentic fleet report - Playwright regression tests for [Kimai](https://www.kimai.org), a PHP 8.2 project

**Bottom line.** A fleet of 6 Claude Code instances, each running the Sonnet 5.5 model and
orchestrated by [Kaizero](https://kaizero.sh), turned a 350-test-scenario QA backlog into Playwright tests about 4.4× faster and 4.8× cheaper than
a human QA engineer. The fleet needed 1.64 minutes of wall-clock time per test scenario (66 test
scenarios in 1h 48m, 55 landed). The whole backlog needs an estimated 11.1 hours of agent work (95%
CI 10.1–12.3 h): 1.5 hours of preparation and 9.6 hours of fleet run. It also needs an estimated
32.2 hours of human work: 20 minutes of prompting and output review, and 31.8 hours to unblock the
11 test scenarios (17% of the 66) the agents cannot finish due to Kimai defects, test scenario
criteria the app does not meet, or a test environment limit. The total is 43.3 hours. A human QA
engineer writing the same tests would need an estimated 191 hours.

> <br>
> ✅ Test results in this report were obtained with the
> <a href="https://go.ivanrublev.com/get-qa-signoff-pro">qa-signoff-pro</a> skills package.
> <br><br>

| Question | Answer | Evidence |
|---|---|---|
| How fast? | 43.3 hours (42.2–44.4) against 191 hours by hand | [section 2](#2-speed-for-350-test-scenarios) |
| Do the tests pass? | All 98 tracked test scenarios pass (55 distinct test scenarios, 43 run on two projects, 12 on one). Two of them pass partially: the HTTPS-only TC-SEC-004 check is skipped because the environment runs over HTTP | [execution report](qa-signoff-pro/2026-10-05T16-08-45P0200/test-execution-report-2026-10-05T16-08-45P0200.md) |
| What does it cost? | About €2,155 against €10,400 of human time | [section 3](#3-cost) |

---

## 1. What was built

Suite snapshot, 5 October 2026: 350 test scenarios (`test-cases/TC-*.md`), each mapped to one
Playwright spec. 55 landed, 11 blocked, 284 open. The landed specs hold 55 files, 3,145 lines and 70
`test()` blocks, which is 57 lines per test scenario. Roles: super admin, regular user and no-login.
Each runs on Desktop Chrome and iPhone 13, so six Playwright projects.

Regression tracking: the `/run-qa-tests` skill from the
[qa-signoff-pro](https://go.ivanrublev.com/get-qa-signoff-pro) package runs the suite and writes one
row per test scenario and project into [`tracking.csv`](qa-signoff-pro/tracking.csv) (pass,
PASS_PARTIAL, fail, flaky). It flags test scenarios whose criteria hash changed (needs
re-automation), orphaned or missing specs, and regressions (failed after passing).

The run took 6.1 minutes in total (~2.7 s per test block): 134 test blocks passed.
A test file holds one or more test blocks.

The skill writes the execution report with the quality gates, plus log files in text and JSON
formats.

The full output list:

- [`tracking.csv`](qa-signoff-pro/tracking.csv)
- [`2026-10-05T16-08-45P0200/test-execution-report-2026-10-05T16-08-45P0200.md`](qa-signoff-pro/2026-10-05T16-08-45P0200/test-execution-report-2026-10-05T16-08-45P0200.md)
- [`2026-10-05T16-08-45P0200/pw-full-2026-10-05T16-08-45P0200.log`](qa-signoff-pro/2026-10-05T16-08-45P0200/pw-full-2026-10-05T16-08-45P0200.log)
- [`2026-10-05T16-08-45P0200/results-2026-10-05T16-08-45P0200.json`](qa-signoff-pro/2026-10-05T16-08-45P0200/results-2026-10-05T16-08-45P0200.json)

| Latest full run, 2026-10-05 16:08, 98 tracked test scenarios (distinct test scenario × project) | Value |
|---|---|
| Pass rate gate (≥80%) | Passed, 100.0% |
| Re-automation, orphan, missing, misplaced, regression gates | Passed, all 0 |
| Passed | 98 (96 pass, 2 PASS_PARTIAL) |
| Failed | 0 |
| Flaky | 0 |
| Skipped, tracked as PASS_PARTIAL | 2 test blocks (environment runs over HTTP) |

## 2. Speed for 350 test scenarios

| | Agent fleet, Kaizero | QA Engineer (Bons rate) |
|---|---|---|
| Preparation: spec, test scenarios, review, fixes | 1h 31m agents, 20 min human prompting and review | not counted |
| Unblocking 17% of test scenarios (about 58), human | 31.8 h | – |
| Writing tests for 350 test scenarios | 9.6 h (8.5–10.8), up to 6 agents in parallel | 190.9 h |
| **Total elapsed** | **43.3 h (42.2–44.4)** | **190.9 h, 24 working days** |
| **Fleet speed-up** | | **4.3–4.5×** |

**Preparation, measured.** Crawl and QA spec 10 min (6M tokens, $2.17). 212 then 350 test scenarios
from the spec 8 min (2M, $1.57). Review against the spec and the app code 1 h (160M, $42.29). Fixes
from the review 13 min (38M, $11.55). Total 91 min, 206M tokens, $57.58. A human adds 20 min of
prompting and output assessment on top; manual fixing is not counted.

**Fleet run, measured.** 10:07–11:55 (108.5 min), 73 Claude Code sessions, peak 6 concurrent agents,
average 4.9. 66 of 350 test scenarios processed: 55 landed, 11 blocked. Wall-clock time 1.64 min per
test scenario. Agent time 8.1 min per test scenario, median 6.2 min for the session that did the
work.

**Estimate for the backlog.** Resample the 66 measured test scenarios 20,000 times, draw 284, sum.
The remaining 284 test scenarios take a mean 7.78 h (95% CI 6.74–8.96 h). With the 1.81 h observed,
the whole backlog takes 9.59 h (95% CI 8.54–10.77 h). About 17% of test scenarios (11 of 66) block
and need a human.

**Unblocking, estimated.** 11 of 66 is 16.7%, and 16.7% of 350 is about 58 test scenarios. A human
who takes over a blocked test scenario writes its test, which costs the QA engineer rate of 0.55 h
per test scenario (190.9 h for 350 test scenarios). That is 31.8 h, added to the fleet total. The
agents could not finish these test scenarios, so they are probably harder than average and 31.8 h is
a lower bound.

**How the human figures are built.**

- **QA Engineer (Bons rate).** Bons et al. measured 6 hours to create 11 replayable Selenium IDE
  scripts for a web app in an industrial case [[1]](#ref-1). That is 0.55 h per script, 190.9 h for
  350.

## 3. Cost

| | Agent fleet | QA Engineer (Germany) | Playwright agency (Poland) | Crowd testing agency (Germany) |
|---|---|---|---|---|
| Hours | 43.3 elapsed | 190.9 (Bons rate) | 190.9 | 116.7 tester-hours, elapsed not published |
| Cost at median €54.71/h | €2,155 (€396 tokens, €18 prompting, €1,741 unblocking) | €10,445 | €5,100–9,300 | €14,350–19,250 |
| Cost range, 10th–90th percentile | | €8,090–13,491 | | |
| Fleet cost advantage at median | | 4.8× | 2.4–4.3× | 6.7–8.9× |

**Agents.** ccusage [[2]](#ref-2) priced the fleet run day at $73.24 for 183.4M tokens, which
includes the fleet run and any other session that day. That is an upper bound of $1.11 per test
scenario. For 350 test scenarios: $388. With $57.58 of preparation: $446, or €396 at the ECB rate of
1.1269 dollars per euro on 6 October 2026 [[3]](#ref-3).

**Humans.** €54.71 per hour: the German P5 Specialist median salary of €99,800 [[4]](#ref-4) over a
COCOMO II staff-year of 1,824 hours (12 person-months at 152 hours) [[5]](#ref-5). The 10th and 90th
percentiles, €77,300 and €128,900, give €42.38 and €70.67 per hour. The salary excludes employer
social contributions, so the human costs are a lower bound.

**Playwright agency, Poland.** Eastern European agencies charge $30 to $55 per hour for Playwright
work [[6]](#ref-6), which is €26.60 to €48.80 at 1.1269 dollars per euro. The column applies the
Bons rate of 190.9 hours. The hourly band is a vendor blog figure, not a quote.

**RapidUsertests.** A Berlin crowd-testing provider [[7]](#ref-7). A 20-minute unmoderated test
costs €55 per tester, or €41 per tester with a credit pack. It charges per 20-minute slot, so a
shorter test scenario still costs a full slot. The column assumes one tester and one 20-minute test
per test scenario: 350 × €41 = €14,350 to 350 × €55 = €19,250, and 116.7 tester-hours. The prices
are for usability tests, so the column is an assumption, not a quote for regression testing.

The fleet cost includes 20 minutes of human prompting and output review at €54.71/h (€18) and 31.8
hours of human work on blocked test scenarios (€1,741). It excludes manual fixing of agent-written
tests.

## 4. Limits

- The 66 measured test scenarios were P0-heavy. The remaining 284 hold fewer journey-level test
  scenarios (3.5% against 9%), so the estimate holds only if the mix stays similar.
- The human rates come from one study of a different technology, Selenium IDE (Bons), not
  Playwright. It did not measure review or maintenance of the tests.
- No human wrote tests for the test scenarios in this backlog. The QA Engineer column is a model
  estimate, not measurements. The fleet column is measurements plus a bootstrap.
- All 98 tracked test scenarios pass, but 2 pass only partially. 295 test
  scenarios have no spec yet (284 open, 11 blocked), so the pass rate covers 55 of 350 test
  scenarios.

## References

1. <a id="ref-1"></a>Bons et al., "Scripted and scriptless GUI testing for web applications: An
   industrial case", Information and Software Technology 158 (2023) 107172.
   <https://www.sciencedirect.com/science/article/pii/S0950584923000265>, open copy
   <https://zenodo.org/records/20763905>. 6 h for 11 scripts (iterations of 2 h and 4 h). Read from
   the full text, section 4.1.
2. <a id="ref-2"></a>ccusage 20.0.26. <https://github.com/ryoppippi/ccusage>. Token counts and cost
   from the Claude Code session logs.
3. <a id="ref-3"></a>European Central Bank, "Euro foreign exchange reference rates", 6 October
   2026. <https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/index.en.html>.
4. <a id="ref-4"></a>Ravio, "Compensation explorer", Germany, level P5 Specialist, percentiles 10th
   €77,300, 25th €87,300, 50th €99,800, 75th €114,100, 90th €128,900.
   <https://app.ravio.com/explore/compensation>. Checked 5 October 2026. The page needs a login.
5. <a id="ref-5"></a>Barry Boehm et al., *COCOMO II Model Definition Manual*, version 2.1, 2000, §3:
   152 hours per person-month.
   <https://www.rose-hulman.edu/class/csse/csse372/201410/Homework/CII_modelman2000.pdf>.
6. <a id="ref-6"></a>DeviQA, "How much does Playwright test automation actually cost? In-house vs.
   outsourced".
   <https://www.deviqa.com/blog/how-much-does-playwright-test-automation-actually-cost-in-house-vs-outsourced-full-breakdown/>.
   Eastern European agencies at $30 to $55 per hour. Vendor blog, not a quote.
7. <a id="ref-7"></a>RapidUsertests, "Pricing". <https://rapidusertests.com/en/pricing/>. 20-minute
   unmoderated test from €55 per tester, from €41 per tester with credits.

---

7 October 2026, Ivan Rublev https://ivanrublev.com
