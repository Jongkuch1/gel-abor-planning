# Gel Abor: Product Requirements Document (PRD)

Version 1.0, 10 October 2026
Owner: Jongkuch Isaac Chol Anyar (BSE, Full Stack Web Development, African Leadership University)

## 1. Summary

Gel Abor ("prevent" in Dinka) is a flood reporting and monitoring web app for Jonglei State, South Sudan. A community member reports a flood by typing a short description or recording a voice note. The report goes by SMS to the county authority responsible for that area. County and state officials then check the report, track the response, and see what is happening across the state on a live dashboard.

## 2. Mission and Global Challenge

> My mission is to work with flood-affected communities in Jonglei State, especially women, youth, and displaced families, to co-design an SMS and voice-based flood reporting system that gets warnings to local authorities in time to act, using my full-stack development skills.

- Main GCGO: Climate Change (disaster risk reduction)
- Second GCGO: Governance

Because the mission says co-design, nothing in this PRD is final until it has been tested with the pilot community (see Section 11).

## 3. Problem

- In the 2024 flood season, about 1.4 million people in South Sudan were affected across 43 counties and the Abyei Administrative Area. More than 379,000 were displaced, and Jonglei and Northern Bahr el Ghazal held over half of everyone affected (UN OCHA, 8 November 2024).
- There is no ongoing channel for a person in a Jonglei community to report a flood directly to their own county authority. Most flood information comes from satellite maps and humanitarian situation reports.
- About 12.1% of South Sudan's population used the internet in January 2024 (DataReportal, 2024), and electricity access was 5.4% in 2023 (World Bank, 2023). Many adults are not comfortable reading and writing.
- Existing tools leave gaps. Ushahidi (Okolloh, 2009) is a public map that does not route a report to a named authority and has no voice option. Machine-learning forecasting (Nevo et al., 2022) predicts river levels but cannot take a report from the ground, and it needs river gauges Jonglei mostly lacks.

## 4. Goals and non-goals

### Goals for this phase (capstone MVP)

| ID | Goal | How we know |
|----|------|-------------|
| G1 | A reporter can send a flood report in under one minute, by voice or text | Median time under 60 s in usability tests with at least 15 pilot reporters |
| G2 | The responsible county authority hears about a new report fast | SMS accepted by the gateway within 60 s for at least 95% of 30 simulated reports |
| G3 | County authorities can check and track each report to the end | Every report moves through the status flow with a note and a name attached |
| G4 | The state administrator sees all nine Jonglei counties in one place | State overview shows counts per county that match a manual count |
| G5 | Each role can only do what it is allowed to do, enforced on the server | Automated API test matrix passes for every endpoint and every role |
| G6 | Reports survive bad connections | A report made offline is sent automatically when the phone reconnects, with no duplicates |

### Non-goals for this phase

- A native Android or iOS app (we build a mobile-friendly web app instead)
- USSD menus or reporting by inbound SMS from basic phones (planned for Phase 2, see Section 10)
- Flood forecasting or prediction
- Rollout beyond the pilot county
- Dispatching boats, food or other resources (Gel Abor informs the people who do this; it does not replace them)
- Payments of any kind

## 5. Users and roles

The system has exactly three roles. The server enforces the limits in the "Cannot" column, not just the interface.

| Role | Who they are | Can | Cannot |
|------|--------------|-----|--------|
| Community Reporter | People living in the pilot county, including women, youth and displaced families. Also trusted people such as chiefs or church leaders who report for others | Register with a phone number and PIN, send reports (voice, text, photo), see their own reports and statuses, get SMS updates | See other people's reports, verify, reject or resolve anything |
| County Authority | County officials who coordinate flood response (for example county relief staff) | See reports from their own county only, listen to voice notes, verify, reject, mark in progress or resolved, add notes, generate a situation report | Submit reports, see other counties |
| State Administrator (Jonglei) | A state-level official or the system owner | See all counties, use the same verification tools, manage users and county data, view System Health and the audit log | Submit reports |

### Example users (illustrative, to be replaced with what we learn in co-design)

- A mother in a displaced family who has a basic Android phone she shares with relatives. She reads a little and prefers to speak. She needs to report water entering the camp quickly and know someone heard her.
- A boma chief who many people go to first when water rises. He may report on behalf of several households.
- A county relief officer who has a smartphone, gets many calls during the season, and needs to know which reports are real and urgent.
- A state official who needs a weekly picture of which counties are worst hit to talk to partners.

## 6. User stories and acceptance criteria

### Community Reporter

| ID | Story | Acceptance criteria |
|----|-------|---------------------|
| R1 | As a reporter, I want to register with my phone number so I don't need an email | Phone in +211 format, 4-digit PIN, county and payam chosen from a list, language chosen. Consent screen shown and recorded |
| R2 | I want to record a voice note so I don't have to type | Big mic button, records up to 60 s, I can play it back and re-record before sending |
| R3 | I want to type instead if I prefer | Optional text box, up to 500 characters. A report needs voice or text, or both |
| R4 | I want to show how deep the water is with pictures | Four choices, each with a picture and a word: ankle, knee, waist, severe (above waist or houses flooded) |
| R5 | I want my location added without typing | GPS is tried for up to 10 s. If it fails, I pick my payam and boma from a list. The report is never blocked by GPS |
| R6 | I want to add a photo if I can | Optional. Photo is shrunk on the phone to about 300 KB before sending |
| R7 | If I have no network, I still want my report to go | Report is saved on the phone, shown as "waiting to send", and sent automatically when the network returns. No duplicate is created |
| R8 | I want to know what happened to my report | "My reports" list with status. SMS sent to me when the status changes |

### County Authority

| ID | Story | Acceptance criteria |
|----|-------|---------------------|
| C1 | As a county officer, I want an SMS when a new report comes in | SMS includes reference number, payam, severity, time and a link to the report |
| C2 | I want to see all reports in my county | List and map views, filter by status, severity, payam and date |
| C3 | I want to hear and see the evidence | Voice note player, photo, map pin, reporter phone number with a call button |
| C4 | I want to update a report | Verify, reject (reason required), mark in progress, resolve. Each change needs a short note. The reporter gets an SMS |
| C5 | I want a quick picture of my county | Stat cards (new, pending, in progress, resolved), severity trend chart, most affected payams, response status tracker |
| C6 | I want to share a situation report | Choose a date range and download a one-to-two-page PDF with counts, a table of reports and notes |

### State Administrator

| ID | Story | Acceptance criteria |
|----|-------|---------------------|
| S1 | I want to see all nine counties at once | Table and map with counts per county and status |
| S2 | I want to open one county | Same view a county officer has, for any county |
| S3 | I want to manage accounts | Create, edit and deactivate County Authority and State Administrator accounts. Reporters can be deactivated, not created |
| S4 | I want to know if the system is working | System Health page: database, API, job queue, SMS gateway, storage |
| S5 | I want to see who changed what | Audit log of logins, status changes and account changes |

## 7. Key product rules

1. A report must have a severity, a location (GPS or payam) and at least one of voice or text.
2. A report belongs to exactly one county. That decides who is alerted and who can see it.
3. Only County Authority and State Administrator can change a report's status. Only a Community Reporter can create a report.
4. Status flow: pending, then verified or rejected. A verified report can go to in progress and then resolved, or straight to resolved. A status never goes backwards. If a mistake is made, the officer adds a note explaining it.
5. Every status change records who did it, when, and a note.
6. The time from report received to first verification is stored. It is the main pilot measure.

## 8. Non-functional requirements

| Area | Requirement |
|------|-------------|
| Low bandwidth | Reporter screens load under about 200 KB of JavaScript (gzipped). A text-only report request is under 5 KB. App works on "Slow 3G" in browser throttling tests |
| Devices | Works on basic Android phones with Chrome, 360 px wide screens and up |
| Offline | App shell opens without network. Reports queue and send later |
| Reliability | SMS retried up to 3 times. Failed SMS shown on System Health |
| Security | PINs and passwords hashed with bcrypt. Short-lived access tokens. Rate limits on login and reporting. HTTPS only |
| Privacy | Collect the minimum. Reporter phone numbers visible only to that county's authority and the state administrator. Consent recorded. Retention period agreed with the county before the pilot |
| Accessibility | Core reporting flow usable without reading, through pictures and icons. Tap targets at least 48 px. WCAG 2.1 AA contrast |
| Language | English and Dinka at launch. Nuer added if the pilot county needs it. Translations done and checked by community members |
| Cost | Free or cheapest tiers for hosting. Main running cost is SMS |

## 9. Success metrics

From the Chapter One objectives and research questions:

1. Median time to submit a report under 60 s, tested with at least 15 pilot reporters in the first six weeks of deployment.
2. SMS to the county authority within 60 s for at least 95% of 30 simulated reports.
3. Permission tests: a Reporter token cannot call verify or resolve endpoints; Authority and Admin tokens cannot call the submit endpoint. 100% pass.
4. Main pilot measure: average time from report received to first verification, compared with the informal channels people use now (baseline collected in listening sessions).
5. Share of completed reports by voice versus text, especially among people with low literacy.
6. Pilot size: at least 20 reporters and 2 county authority users over 12 weeks.
7. Community-defined measures agreed in the first listening sessions, plus sign-off from both county authorities and community representatives before any wider rollout.

## 10. Release scope

### MVP (capstone)

- Reporter PWA: register, report by voice or text, severity pictures, GPS or manual location, photo, offline queue, my reports
- SMS alerts to county authorities and status updates to reporters
- County Authority dashboard: list, map, detail, status actions, notes, charts, situation report PDF
- State Administrator dashboard: state overview, county drill-down, user and county management, System Health, audit log
- Server-side role enforcement with automated tests

### Phase 2 (after the capstone, if the pilot supports it)

- Reporting by inbound SMS or USSD for people with no smartphone
- Reporting by phone call (voice line)
- Recorded audio instructions in Dinka and Nuer inside the app
- More counties, then the whole state
- Data export for partners such as OCHA clusters

## 11. Assumptions, risks and open questions

### Assumptions to test in co-design (not facts yet)

- People will trust and use a digital channel, or will use it through a trusted person like a chief.
- Some households have at least one Android smartphone they can use during a flood.
- County officials have phones, receive SMS and are willing to act on reports.
- Voice notes are easier for people than typing.

### Risks

| Risk | Mitigation |
|------|------------|
| Cannot reach communities during the term | Use Twic East contacts and phone calls for early sessions, partner with a church, NGO or county office |
| Only powerful voices heard (chiefs, officials, men) | Separate small sessions with women, youth and displaced families. Record who was and wasn't reached |
| SMS gateway does not deliver to South Sudan numbers | Confirm +211 coverage before building (open question 3). Provider is swappable in code |
| False or duplicate reports | County verification step. Duplicate check on the client ID |
| Authorities receive reports but don't act | Track response times and report them honestly as a finding |
| Location data puts people at risk | Show exact locations only to the right county and state users. Agree retention rules |
| Power and network go down during floods | Offline queue. Low data use. SMS as the alert channel |

### Open questions

1. Which county is the pilot site? (to confirm with the supervisor)
2. The mission says "SMS and voice-based", but Chapter One scope leaves out reporting by inbound SMS. Should inbound SMS reporting move into the MVP?
3. Does Africa's Talking deliver SMS to South Sudan (+211) networks? If not, which provider will we use?
4. Who translates and checks the Dinka (and possibly Nuer) text?
5. Who owns the data, and how long is it kept? This needs a written agreement with the county.
6. What does ALU ethics approval require before talking to community members?

## 12. Sources

- DataReportal. (2024). Digital 2024: South Sudan.
- Nevo, S., et al. (2022). Flood forecasting with machine learning models in an operational framework. Hydrology and Earth System Sciences, 26(15), 4013-4032.
- Okolloh, O. (2009). Ushahidi, or 'testimony': Web 2.0 tools for crowdsourcing crisis information. Participatory Learning and Action, 59(1), 65-70.
- UN OCHA. (2024, November 8). South Sudan: Floods snapshot (as of 8 November 2024).
- World Bank. (2023). Access to electricity (% of population), South Sudan.
