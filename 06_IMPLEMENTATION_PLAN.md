# Gel Abor: Implementation Plan

Version 1.0, 10 October 2026
Read with: all other documents in this folder

## 1. How to use this plan

- Work through the phases in order. Each phase ends with a "Done when" check. Don't start the next phase until it passes.
- The plan assumes one developer (me) working about 15 hours a week. If I have more or less time, stretch or shrink the weeks, but keep the order.
- If I use a coding assistant such as Claude Code, give it all six documents at the start and ask it to build one phase at a time.

## 2. Timeline problem to settle first

Chapter One promises a 12-week pilot. The build below takes about 9 weeks. Together that is about 21 weeks, which is longer than one trimester. There are two honest options:

| Option | What it means | Trade-off |
|--------|---------------|-----------|
| A. Start building now, during Pre-Capstone | Phases 0 to 2 happen this term. The rest of the build and the full 12-week pilot fit in the Capstone term | Needs time this term alongside other courses |
| B. Shorten the pilot | Build in the first part of Capstone, then pilot for about 6 weeks | Chapter One's 12-week pilot would need to change to match |

Decide this with the supervisor and update Chapter One if option B is chosen.

## 3. Phases

### Phase 0: Prepare and co-design (weeks 0 to 2, before or alongside coding)

The mission says co-design comes first, so this phase is not optional.

Tasks:
- [ ] Confirm the pilot county with the supervisor
- [ ] Apply for ALU ethics approval and get any county permission letter needed
- [ ] Confirm the SMS provider delivers to South Sudan (+211) numbers. Send a test SMS to a +211 number. If it fails, choose the fallback provider
- [ ] Run listening sessions (in person or by phone) with separate groups: women, youth, displaced families, chiefs or church leaders, and county officials. Ask about past floods: "Last time the water rose, what did you do, and who did you tell?"
- [ ] Record the current way reports reach the county and how long it takes. This is the baseline for the main pilot measure
- [ ] Agree with participants what success means for them (community-defined measures)
- [ ] Test the four depth pictures and the key words in Dinka
- [ ] Find a Dinka translator (and a Nuer one if needed)
- [ ] Keep a list of who was consulted and who could not be reached

Done when:
- Pilot county, SMS provider and ethics status are confirmed in writing
- Listening session notes are saved, with a list of design changes they suggest
- PRD open questions 1 to 6 have answers or a named person following them up

### Phase 1: Project setup (week 1 of build)

Tasks:
- [ ] Create the monorepo (`apps/web`, `apps/api`, `packages/shared`) with TypeScript, ESLint, Prettier
- [ ] Add the shared enums and Zod schemas from 05_BACKEND_SCHEMA.md
- [ ] Connect to MongoDB Atlas, add Mongoose models for counties, users, reports, logs, SMS, tokens, audit, counters
- [ ] Seed the nine counties and the pilot county's payams and bomas
- [ ] GitHub Actions: lint, type check, test, build
- [ ] Deploy a "hello" version of web and API to staging

Done when: staging web page loads, `/healthz` returns ok, CI passes on `main`.

### Phase 2: Sign-in and roles (week 2)

Write the permission tests first, then the code that makes them pass.

Tasks:
- [ ] Permission matrix test file covering every endpoint in TRD section 3 for each role (most will fail at first)
- [ ] Register, login, refresh, logout, `/me`
- [ ] bcrypt hashing, lockout after 5 failed attempts, rate limits
- [ ] Middleware: `authenticate`, `requireRole`, `scopeToCounty`
- [ ] State admin can create county authority accounts
- [ ] Audit log entries for sign-in events and account changes
- [ ] Web: Welcome, Register, Sign in screens, role-based redirect

Done when: all auth tests pass, and a reporter, a county authority and a state admin each land on their own home screen on staging.

### Phase 3: Reporting (weeks 3 and 4)

Tasks:
- [ ] `POST /reports` with multipart upload, server-side county assignment, `clientId` duplicate check, `refNo` from counters
- [ ] Cloudinary upload with authenticated delivery, and the permission-checked media endpoint
- [ ] `GET /reports/mine`, `GET /reports/:id`
- [ ] Reporter screens: Home, steps 1 to 5, Sent, My reports, Report detail
- [ ] Voice recording with MediaRecorder (60 s limit, playback, re-record)
- [ ] Photo shrinking on the phone
- [ ] GPS with 10 s timeout and payam/boma fallback
- [ ] Service worker app shell and IndexedDB outbox with retry and background sync
- [ ] Dinka strings for all reporter screens (from the translator)

Done when:
- A report sent in airplane mode is delivered after reconnecting, with no duplicate
- The reporter flow works on a real low-cost Android phone on throttled "Slow 3G"
- A voice report takes under 60 s in a quick self-test
- Permission tests for report endpoints pass

### Phase 4: SMS alerts (week 5)

Tasks:
- [ ] `SmsProvider` interface with the chosen provider, fallback, and `ConsoleProvider`
- [ ] Agenda job `sms.send` with 3 retries and saving every attempt to `smsMessages`
- [ ] Templates in English and Dinka, GSM-7 or UCS-2 detection, segment count
- [ ] New report alert to all active county authorities and backup numbers
- [ ] Delivery webhook with shared secret
- [ ] Daily SMS cap per county
- [ ] Script: submit 30 simulated reports and measure `sentAt - receivedAt`

Done when: at least 95% of the 30 simulated reports have an SMS accepted within 60 s on staging, and real SMS arrive on a +211 test phone.

### Phase 5: County Authority dashboard (weeks 6 and 7)

Tasks:
- [ ] `GET /reports` with filters, scoped to the officer's county
- [ ] `PATCH /reports/:id/status` with the transition rules, required notes, `verifiedAt` and response log
- [ ] SMS to the reporter on status change
- [ ] `POST /reports/:id/notes` with `visibleToReporter`
- [ ] `/stats/summary`, `/stats/trends`, `/stats/areas`
- [ ] Screens: Dashboard (stat cards, chart, recent list, mini map), Reports table, Map, Report detail with actions, Profile
- [ ] Situation report PDF with pdfkit
- [ ] Short link `/r/:refNo` opening the report after sign-in

Done when: a county officer can go from SMS link to verified report in under 1 minute, and cannot open any report from another county (checked by test).

### Phase 6: State Administrator dashboard (week 8)

Tasks:
- [ ] State overview with all nine counties
- [ ] County drill-down reusing the county dashboard
- [ ] Users management (create, edit, deactivate, reset password)
- [ ] Counties management (payams, bomas, backup numbers)
- [ ] System Health page with the five checks from TRD section 10
- [ ] Audit log page with filters and CSV export

Done when: the full permission matrix test passes 100% and System Health shows correct OK, Warning and Down states when a check is forced to fail.

### Phase 7: Hardening and usability (week 9)

Tasks:
- [ ] Security review against TRD section 11 (headers, CORS, rate limits, logs without personal data)
- [ ] Performance budgets from TRD section 12 (bundle size, response times)
- [ ] Accessibility checks (axe, contrast, tap sizes, 200% zoom)
- [ ] Playwright end-to-end tests for the main flows
- [ ] Backups set up and a restore tested once
- [ ] Usability sessions with at least 15 pilot reporters, timing each report
- [ ] Fix the top problems found
- [ ] Training guide (one page, with pictures) for reporters and for county officers

Done when: median report time is under 60 s across the 15 sessions, all tests pass, and the county officer signs off that the dashboard is ready for the pilot.

### Phase 8: Pilot (12 weeks, or 6 if option B)

Tasks:
- [ ] Onboard at least 20 reporters and 2 county authority users (aim for a mix of women, youth and displaced families, as the mission promises)
- [ ] Weekly check-in with county officers
- [ ] Weekly look at System Health and failed SMS
- [ ] Track every week: reports by voice and by text, time to first verification, SMS success rate, reports queued offline
- [ ] Mid-pilot feedback interviews with reporters and officers
- [ ] End-of-pilot interviews and survey
- [ ] Ask both the county authority and community representatives for sign-off (or not) before any wider rollout

Done when: the pilot period is over and the data needed for all four research questions has been collected.

### Phase 9: Analysis and write-up

Tasks:
- [ ] Compare time to first verification with the Phase 0 baseline (RQ2)
- [ ] Compare completed reports by voice and by text (RQ1)
- [ ] Summarise officer feedback on the dashboard (RQ3)
- [ ] List technical and connectivity problems seen during the pilot (RQ4)
- [ ] Report who took part and who was missed, honestly
- [ ] Write the results and discussion chapters

## 4. Milestones

| Milestone | End of | Evidence |
|-----------|--------|----------|
| M0 Co-design ready | Phase 0 | Session notes, decisions log, SMS test result |
| M1 Skeleton live | Phase 1 | Staging URLs, CI green |
| M2 Roles enforced | Phase 2 | Permission tests passing for auth |
| M3 Reporting works offline | Phase 3 | Airplane mode demo video |
| M4 SMS within 60 s | Phase 4 | Timing script output |
| M5 County dashboard | Phase 5 | Demo with a county officer |
| M6 Full system | Phase 6 | Full permission matrix passing |
| M7 Pilot ready | Phase 7 | Usability results, sign-off |
| M8 Pilot complete | Phase 8 | Pilot data set |

## 5. Risks during the build

| Risk | Early warning | What to do |
|------|---------------|------------|
| SMS provider doesn't cover +211 | Phase 0 test SMS fails | Switch to fallback provider before Phase 4 |
| Voice recording fails on some phones | Phase 3 device tests | Fall back to text, log the phone model, try `audio/mp4` |
| Behind schedule | A phase runs more than a week over | Cut from the Phase 6 extras first (CSV export, county editing), never from security or offline |
| Can't recruit enough pilot users | Fewer than 10 sign-ups by week 2 of the pilot | Work through a church, NGO or chief, and use the diaspora fallback described in Chapter One scope |
| Free hosting sleeps and delays SMS | SMS timing test fails after idle time | Move API to an always-on plan for the pilot |
| Data loss | Restore test fails | Fix backups before the pilot starts |

## 6. Definition of done (every feature)

- Works on a 360 px wide Android phone and on a laptop
- Server-side permission checks in place and covered by tests
- Input validated with the shared Zod schema
- English and Dinka text in place for anything a reporter sees
- No personal data in logs
- Tests written and passing in CI
- Deployed to staging and checked by hand
