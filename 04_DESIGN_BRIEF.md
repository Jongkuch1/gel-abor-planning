# Gel Abor: Design Brief

Version 1.0, 10 October 2026
Read with: 03_APP_FLOW.md (screens), 01_PRD.md (users)

## 1. What we are designing

Two connected surfaces for one flood reporting system in Jonglei State, South Sudan:

1. **Reporter app**: used on a basic Android phone, often outdoors, sometimes in the rain, by people who may not read well. It has to work with almost no explanation.
2. **Dashboards**: used by county and state officials on a laptop, tablet or phone to check reports and plan a response. They need to see a lot of information quickly.

## 2. Who we are designing for

- A mother in a displaced family sharing one phone with relatives. She reads a little and would rather speak than type.
- Youth who are comfortable with phones and may report for older relatives.
- A boma chief or church leader who reports for several households.
- A county relief officer with many calls to handle in flood season who needs to know which reports are real and urgent.
- A state official preparing a weekly picture of the nine counties.

These are starting points. The pilot community will change them during co-design sessions.

## 3. Design goals (in priority order)

1. **Works without reading.** Every step of the reporting flow has a picture or icon and a few words. Voice is as easy as typing.
2. **Works on a cheap phone with bad network.** Small pages, no heavy images, clear feedback when something is waiting to send.
3. **Fast for officials.** The most urgent reports are the easiest to find. Key actions are one click from the report.
4. **Calm and trustworthy.** People use this during a frightening time. No flashing, no alarm sounds, plain and serious tone, clear confirmation that the report arrived.
5. **Fair to everyone.** Do not rely on colour alone. Large text and tap areas. Readable in sunlight.

## 4. Brand

- **Name:** Gel Abor, which means "prevent" in Dinka.
- **Logo:** navy rounded-square badge, a blue water droplet with a light highlight, and amber signal arcs at the top right (the alert going out). Wordmark in a bold serif, tagline "Flood reporting and monitoring" in small caps sans-serif.
- **To do:** the current logo files still say "Tïït Abor". They need a new wordmark reading "Gel Abor". The icon itself can stay.

## 5. Colour tokens

Blue is the main colour (water), navy for structure, amber only for alerts and the logo arcs.

| Token | Hex | Use |
|-------|-----|-----|
| `navy-900` | #0B1F3A | Dashboard sidebar, logo badge |
| `navy-700` | #16325C | Sidebar hover, headings on light backgrounds |
| `blue-600` | #1E6FD9 | Primary buttons, links, active states |
| `blue-100` | #DCEAFB | Selected rows, light highlights |
| `blue-50` | #EEF5FD | Page background on reporter app |
| `amber-500` | #F2A900 | New-report badges, alert accents, logo arcs |
| `slate-900` | #0F172A | Main text |
| `slate-600` | #475569 | Secondary text |
| `slate-200` | #E2E8F0 | Borders, dividers |
| `white` | #FFFFFF | Cards, dashboard background |

### Severity (water depth)

Always shown with the picture and the word, never colour alone.

| Severity | Hex | Picture |
|----------|-----|---------|
| Ankle | #FACC15 | Person with water at the ankles |
| Knee | #F97316 | Water at the knees |
| Waist | #DC2626 | Water at the waist |
| Severe | #7F1D1D | Water above the waist or a flooded house roof |

### Status

| Status | Hex | Chip label (dashboard) | Reporter label |
|--------|-----|------------------------|----------------|
| pending | #64748B | Pending | Sent, waiting for the county |
| verified | #1E6FD9 | Verified | Checked by the county |
| in_progress | #7C3AED | In progress | Help is being organised |
| resolved | #16A34A | Resolved | Closed by the county |
| rejected | #94A3B8 (with strike icon) | Rejected | Not confirmed |

Check every text and background pair against WCAG 2.1 AA (4.5:1 for normal text). Use dark text on yellow and orange chips, white text on the darker ones.

## 6. Typography

- **Font:** Noto Sans for everything in the app. It supports the Dinka letters ɛ, ɔ, ŋ and the vowels with two dots (ä, ë, ï, ö). Test these letters early: the old logo had a rendering bug with "ï".
- **Reporter app:** body 18 px, buttons 20 px semibold, step titles 24 px bold.
- **Dashboards:** body 14 to 16 px, table text 14 px, stat numbers 28 to 32 px bold, page titles 22 px.
- Line height about 1.5 for body text.

## 7. Icons and pictures

- Dashboard: Lucide line icons (consistent, light).
- Reporter app: simple filled pictograms for the four water depths, mic, camera, location pin, tick, clock (waiting), and phone.
- The depth pictures are the most important artwork in the product. Draft 2 or 3 versions and test them with pilot community members before the build is final. Ask: "Which picture shows the water at your house today?"
- No photos of real people in the interface.

## 8. Reporter app layout rules

- One task per screen. Progress dots at the top (5 steps).
- Main action button full width, at least 56 px high, placed in the bottom third of the screen where thumbs reach.
- Tap targets at least 48 x 48 px with 8 px gaps.
- Mic button 96 px round, blue, turns red while recording with a countdown ring.
- Plain backgrounds (`blue-50` or white), no background images.
- A clear banner when offline or when reports are waiting to send.
- Works in portrait from 360 px wide. No horizontal scrolling.
- Nice to have (Phase 2): a speaker icon on each step that plays a short recorded instruction in Dinka or Nuer.

## 9. Dashboard layout rules

Follow the earlier Gel Abor dashboard mockup (blue version):

- Dark navy sidebar on the left with logo, nav items and the signed-in user at the bottom. Collapses to icons on tablets and a top menu on phones.
- Top bar: page title, date range picker, county name (for county users).
- Row of 4 stat cards: big number, label, small change indicator.
- Main area: map or chart on the left, recent reports list on the right on wide screens. Stacks on narrow screens.
- Tables: sticky header, severity and status chips, row click opens the report.
- Report detail: evidence (voice, text, photo, map) on the left, status timeline and action buttons on the right.
- Destructive or important actions (Reject, Resolve) ask for a note and a confirm.

## 10. Writing style in the product

- Short, plain words. Say "Checked by the county", not "Verified by authority".
- Speak to the reporter as "you". No blame in error messages.
- Numbers and times in a familiar format: "12 Oct, 14:40".
- Every English string has a Dinka version from a community translator. Keep strings short because some phones show little text and Dinka SMS messages have a 70-character limit per part.

### Sample text

| Place | English |
|-------|---------|
| Home button | Report a flood |
| Step 1 title | How deep is the water? |
| Step 2 title | Tell us what is happening |
| Mic hint | Tap to speak. Tap again to stop. |
| Step 3 title | Where are you? |
| Sent | Sent. Your county has been told. Reference R-1042. |
| Saved offline | No network now. We will send it as soon as you are connected. |
| Consent | We keep your phone number and your report. Only county and state flood officers can see them. |

## 11. Accessibility checklist

- WCAG 2.1 AA contrast for all text.
- Severity and status never shown by colour alone.
- All icons have text labels (visible or for screen readers).
- Usable at 200% zoom and with large system fonts.
- Focus outlines visible on dashboards for keyboard users.
- No time limits on reporter screens, except the 60 s voice recording, which shows a countdown.

## 12. Deliverables

1. Figma file with:
   - Reporter screens at 360 x 800 (all screens in App Flow section 9)
   - Dashboard screens at 1440 x 900 and 1024 x 768
   - Component library: buttons, chips, stat cards, table row, step header, mic button, depth cards, toasts, dialogs
   - Colour and type styles matching sections 5 and 6
2. Four depth pictograms (SVG), tested with the community.
3. Updated Gel Abor logo files (full, horizontal, icon only; transparent and white backgrounds).
4. Tailwind config with the tokens above.

## 13. Co-design checkpoints

| When | What we test with the community | Change we expect |
|------|--------------------------------|------------------|
| Before high-fidelity design | Depth pictures, words for statuses, voice versus typing preference | Pictures and words |
| After first clickable prototype | Full report flow, timed | Fewer steps or different order |
| First week of pilot | Real use, feedback interviews | Fixes to text, Dinka wording, button placement |
