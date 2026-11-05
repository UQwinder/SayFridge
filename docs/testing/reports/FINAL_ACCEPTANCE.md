# Final Release Acceptance Record

**Release: v1.0.0.** Full Regression and deployed Backend checks passed for
`8cea6def159ee9cb3e1fd9139e6ab0a52279c2b7`. The team completed the core Android
and cross-client workflows listed below.

## Release identity

| Field | Recorded value |
| --- | --- |
| Release date | 2026-10-04 (NZDT) |
| Selected code commit | `8cea6def159ee9cb3e1fd9139e6ab0a52279c2b7` |
| Release | [v1.0.0 — Android Assessment Build](https://github.com/UOA-CS734-S2-2026/project-implementation-appix/releases/tag/v1.0.0) |
| Android APK | `SayFridge-1.0.0-8cea6de.apk`; version 1.0.0, build 1 |
| APK SHA-256 | `97f6f94a874f7ebaa0e8c5a9489aadb10383e9d8c2053acf5c4f0d9879c2c1f1` |
| APK signing | Release-mode build; Android Debug certificate |
| Android emulator | Pixel 10 Pro XL (`Pixel_10_Pro_XL`); Android 17, API 37, arm64-v8a |
| Physical Android device | Xiaomi 12; HyperOS 3.0.3.0 |
| Testers | syan349, wzho537, qche584, dras728, rhan637 |
| React Render deployment commit | Not recorded |
| Flutter Web deployment commit | `2f600fbea4b522d251a474b3c2864501a652fa87`; checked 2026-10-04 through `/build-info.json` |
| Flutter Web integration deployment commit | `3333ad79b522b59d6a17fa144491b46459a5887a`; checked 2026-10-04 through `/build-info.json` |
| Backend deployment commit | `8cea6def159ee9cb3e1fd9139e6ab0a52279c2b7`; checked 2026-10-04 through `/api/v1/system/version` |
| Neon endpoint and database | `ep-divine-star-a7b6pjnw`, `neondb` |
| Alembic revision | `a261002_chat` |

The APK and Full Regression use the selected code commit. The Flutter Web primary
site includes the later documentation merge, PR #193. Existing Neon accounts,
inventory and test data are retained. No database reset or demo reseed was performed.

## Automated release validation

| Check | Result | Evidence |
| --- | --- | --- |
| Full Regression | PASS; Backend, Web, Flutter and eight Chromium E2E scenarios | [Run 37190832320](https://github.com/UOA-CS734-S2-2026/project-implementation-appix/actions/runs/37190832320), [JSON summary](../evidence/full-regression/37190832320/regression-summary.json) |
| APK integrity and metadata | PASS; archive integrity, APK signature, package/version, architectures and compiled API URL | `INSTALLATION.txt` and `SHA256SUMS.txt` in the release assets |
| Deployed Backend | PASS; health, source SHA, OpenAPI, ordinary-user login, business reads and administrator-access denial | Hosted checks on 2026-10-04 at 22:09 NZDT |
| Session APIs | PASS; refresh rotation, renewed access, logout and revoked-access rejection | Same hosted checks; the login session was found in the selected Neon database |
| Browser CORS | PASS; React and both Flutter Web origins | Same hosted checks |
| Database and Agent storage | PASS; business schema and Agent storage checks; revision `a261002_chat` | Read-only checks during the Backend deployment on 2026-10-04 |

The hosted checks read identity, spaces, inventory, operation history and expiry
results. They created and ended a test login session. They did not change inventory.

## Full Regression run history

Three manually triggered Full Regression runs passed on `main`. The JSON summaries
retain each run's commit, job outcomes and test scope. Dates, durations and the
triggering account were recorded from the supplied Actions screenshots.

| Run | Execution date/time | Tested commit | Triggered by | Duration | Outcome | Evidence |
| --- | --- | --- | --- | --- | --- | --- |
| [#1](https://github.com/UOA-CS734-S2-2026/project-implementation-appix/actions/runs/36999895826) | 2026-10-03 00:14 GMT+13 | `269531b1a7cf4843ef5f9b049a9dd8e693f710d1` | syan349 | 4m 29s | PASS | [JSON summary](../evidence/full-regression/36999895826/regression-summary.json) |
| [#2](https://github.com/UOA-CS734-S2-2026/project-implementation-appix/actions/runs/37179497627) | 2026-10-04 18:18 (displayed local time) | `d3358cda5dbe49b177c8d94ce8a1b81394364448` | syan349 | 5m 21s | PASS | [JSON summary](../evidence/full-regression/37179497627/regression-summary.json) |
| [#3](https://github.com/UOA-CS734-S2-2026/project-implementation-appix/actions/runs/37190832320) | 2026-10-04 | `8cea6def159ee9cb3e1fd9139e6ab0a52279c2b7` | syan349 | 6m 27s | PASS | [JSON summary](../evidence/full-regression/37190832320/regression-summary.json) |

All three summaries report successful `revision`, `backend-full`, `frontend-full` and
`browser-e2e` jobs. Their stated scope is the Backend offline full suite, Web
Vitest/typecheck/build, Flutter unit/widget suite and eight Chromium E2E scenarios.
They exclude live OpenAI opt-in tests, `manual_preview` screenshot exporters, and
Android/iOS builds and device testing. Detailed per-test counts are not provided
in these summaries.

Run #3 verifies the selected APK code commit, `8cea6de`. The earlier runs retain
the results for their listed commits. Core Android and cross-client results for the
release APK are recorded below.

## Performance verification — 2026-10-04

The [Performance Test Record](../records/PERFORMANCE_TEST_RECORD.md) measures
local main `a370559786f5305a36b0f48d729792451c38eaff` and hosted Backend
`e538779976a46e1a218c8e4d59bbbaeb374fa9b3`. Their committed Backend trees are
identical. The local baseline sustained 5 RPS for 10 minutes with 50,000 batches
and 500,000 operations: 3,000 valid requests, read p95 37.64 ms, write p95 30.23 ms,
and no errors or dropped arrivals. All 120 hosted read samples passed, with p95
154.61 ms. Exact balance/version persistence and contention checks passed.

Nine of 10 real-model Agent samples completed. The tenth returned
`rate_limit_exceeded`; the record retains this finding and the model timings.
The performance record identifies the code versions, data volumes and environments
used for these measurements.

## Backend release-readiness verification — 2026-10-02

The three authentication controls were rebuilt on the fresh main-derived branch
`syan349_fix_backend_readiness`, based on main `d402b79970e005e4922babba1c48d6f1dda5e36c`.
The old fix branch was deleted; its commit was not merged or cherry-picked.
These results test the uncommitted working tree. The
[verification manifest](../evidence/backend-release-readiness/verification-2026-10-02.json)
records the code/test SHA-256 values and preserves earlier same-day verification.

| Check | Actual result | Evidence |
| --- | --- | --- |
| Authentication, permissions, persistent sessions, JWT scope, safe logging, smoke-runner and Agent quantity/dialogue selection | 130 passed in 13.88 seconds; Python 3.12 and isolated local PostgreSQL 18 schemas | [Fresh-branch JUnit](../evidence/backend-release-readiness/auth-regression-fresh-main-2026-10-02.xml) |
| Backend fast CI selection | 172 passed, 743 deselected in 24.85 seconds; same local test environment | [Fresh-branch JUnit](../evidence/backend-release-readiness/fast-ci-fresh-main-2026-10-02.xml) |
| Earlier same-day real-model check for issue #144 | 2 passed in 55.68 seconds; `gpt-6-luna` and isolated local inventory, before recreating the branch | [Live-model JUnit](../evidence/backend-release-readiness/agent-half-consumption-live-2026-10-02.xml) |
| Earlier same-day deployed Backend baseline | Health/version, unauthenticated rejection, ordinary/admin login, categories, stock/expiry reads, administrator denial, refresh rotation, logout revocation and browser CORS passed | [Sanitized smoke result](../evidence/backend-release-readiness/deployment-2026-10-02.json) |
| Existing Neon baseline | Revision `a261002_categories`; 25 active public categories | Included in the sanitized smoke result |

The regression selections overlap; their counts are reported separately. The first
fresh-branch attempt could not connect to local PostgreSQL from the sandbox. After
loopback permission was granted, all 130 cases passed; the manifest records both
attempts. No shared Neon inventory or schema was changed by these fresh runs.

The earlier real-model check verifies that a single 2 kg rice batch becomes 1 kg
after consuming half. With two batches, target clarification is required before
deduction; a new purchase and inventory query preserve the pending task; selecting
the Kitchen batch resumes the deduction once and leaves the Garage batch unchanged.
It verifies Backend dialogue and persistence, with Mobile UI acceptance recorded
separately below. The initial missing-model-configuration attempt and successful
rerun are preserved in the manifest.

The 2026-10-02 hosted smoke tested `d402b799`. It created and revoked login
sessions and read business data. The selected-release checks for `8cea6de` are
recorded in the automated release validation table above.

## Manual cross-client and Android validation

Date: 2026-10-04 (NZDT). APK: `SayFridge-1.0.0-8cea6de.apk`.
Testers: yan349, wzho537, qche584, dras728, rhan637.
Devices: Pixel 10 Pro XL emulator, Android 17 (API 37), arm64-v8a;
Xiaomi 12 physical device, HyperOS 3.0.3.0.
Backend: Vercel; database: Neon.

The team confirmed these results on 2026-10-04. Emulator model and OS details
were read from the local AVD and running device.

| Workflow | Result |
| --- | --- |
| Install and start the APK | PASS |
| Register an account | PASS |
| Sign in | PASS |
| Add inventory | PASS |
| Record consumption | PASS |
| View the same saved inventory in React Web | PASS |
| Camera input | PASS |
| Speech input | PASS |

Automatic renewal, logout, quantity reconciliation, permission denial, native
reminders, network interruption, expiry/removal and direct browser navigation
have no separate manual result recorded for this APK. The Backend session APIs
and access controls passed the hosted checks listed above.

The [Manual Integration Test Record](MANUAL_INTEGRATION_TEST_RECORD.md) retains
13 development test cases and their follow-up. These dated records document the
earlier test rounds.

## Findings and reruns

| Finding | Reproduction and observed result | Fix/issue/commit | Rerun and evidence |
| --- | --- | --- | --- |
| No final-release findings recorded | — | — | — |

Keep an initial failure alongside its successful rerun. Record final accepted
behavior and any team scope decision; do not erase earlier evidence.

## Submission links

| Deliverable | Location |
| --- | --- |
| Project overview and setup | [Root README](../../../README.md) |
| Release APK and installation instructions | [SayFridge v1.0.0 — Android Assessment Build](https://github.com/UOA-CS734-S2-2026/project-implementation-appix/releases/tag/v1.0.0) |
| Architecture and decisions | [System Architecture](../../architecture/README.md) |
| Technical learning | [Learning and Design Evolution](../../learning/README.md) |
| Security/privacy evidence | [Security and Privacy](../../security/README.md) |
| Manual integration findings and follow-up | [Manual integration test record — issue #139](MANUAL_INTEGRATION_TEST_RECORD.md) |
| Historical designs and results | [Project archive](../../archive/README.md) |
| Weekly meetings, decisions and responsibilities | [Team Wiki](https://github.com/UOA-CS734-S2-2026/project-implementation-appix/wiki) |

Code and documentation are submitted through `main`. The course Private information
ZIP contains the assessment account passwords and local environment files. Passwords
are not included in this record.
