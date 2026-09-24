# Prompts — copy and paste

Ready-to-paste prompts for the Alba codebase session. Newest at the top.
Each one records the decisions already taken, so nothing has to be re-argued.

---

## 0 · End of every session — **paste this before you close**

Keeps ClickUp a mirror of the work. See [`CLICKUP.md`](CLICKUP.md) for the rules.

```
Before we stop, close the session properly.

1. DEVLOG — add today's entry at the top of docs/DEVLOG.md: what was
   wrong, what changed, the number or test that proves it. Commit it.

2. CLICKUP JOURNAL — in Imarketin › 🌅 Alba › 📓 Journal de dev, create
   (or update, if one exists for today) the card
   "📓 Journal — YYYY-MM-DD · <gist>", status completed, due today, with
   these sections: Sessions du jour · Livré · Ce qui a cassé / leçons ·
   Décisions prises · Resté ouvert. Short, French, no guest names.

3. DECISIONS — for every decision taken today, one card in 📌 Décisions,
   status "accepted", due today: what was decided, why, what we rejected,
   where it is written, when to revisit. For every question we left open,
   one card with status "Open" and the date by which it must be decided.

4. TASKS — every new task you mentioned today gets a card in the right
   list WITH A DUE DATE (never a Sunday). Anything overdue gets a new date
   and a comment saying why it slipped.

5. Tell me: the journal card link, the decision cards, and anything you
   could not write to ClickUp (API limit, missing list) so I can fix it.
```

---

## R · For the report session — **paste this into that session** (25 Sep)

```
Status check, then two jobs, in this order. Read CLAUDE.md first.

WHERE WE ARE (verify, don't trust me):
- Production (main) stops at 31 Aug (#6, API security). Everything on
  claude/hotel-value-report-monthly-n4bmjv since then — the value
  report, the month ledger, the roster-expected ledger, /legal — is NOT
  in production.
- So the live tablet still deletes at 30 days and records no day
  ledger. Every day since 31 Aug is lost from the weekday pattern.

0. TELL ME FIRST, in a short table: what is on main vs this branch;
   how many days of data the production tablet holds right now; what
   would merge cleanly; what is risky.

1. SHIP COLLECTION ONLY — today if safe. Collection, not the screen:
   - day ledger, one record per service, NO names, NO room numbers
   - backfill from the days still on the tablet (say how many you got)
   - staff count asked at the START of service, a number only
   - two planned windows (weekday / weekend), window saved with each day
   - comfortable load per staff: a setting with NO default
   - écart payment prompt: no pre-selected "Chambre", écart cases only
   - feel check: optional, monthly average only, never per day/person
   - a `service` key on each record (Events will add several a day)
   Deploy after service, never before a Wednesday. Keep the old path as
   fallback. Do NOT ship the old money-led report screen with it.
   Stay local-only: no server store, no sync, no email with guest data
   until the DPA is signed.

2. THEN THE REPORT — operations, not savings:
   - headline: covers per staff member at peak (blank + explanation
     while comfortable load is unset)
   - arrival curve per 15 min with the staffing line in covers
   - servi / présent non servi / absent
   - day-of-week averages, always labelled "basé sur N semaines"
   - never print a number we did not observe; no euros in the headline;
     assumptions printed at the bottom, editable; one printed page
   Design reference: the "Rapport mensuel" board on the canvas
   https://claude.ai/artifact/VC8YJiF7AuMtEeNz5quTFB
   Keep other UI changes minimal: every check-in screen is being put on
   that canvas for a full UX redesign — don't redesign in parallel.

3. FOR THE OCTOBER MEETING: generate the report from the real
   collected data. Lead with the arrival curve, then the three-way
   split, then the peak. Show me the page before anyone else sees it.

CLOSE THE SESSION with the end-of-session routine: docs/DEVLOG.md
entry, then in ClickUp Imarketin › 🌅 Alba: a card in
"📓 Journal de dev", decisions in "📌 Décisions", new tasks with a due
date in "🍳 Petit-déjeuner — en production".
```

---

## 1 · Ship data collection to production — **paste this now**

> Urgent because nothing is collecting. The live tablet deletes at 30 days and
> the ledger was never merged, so every day this waits is a day permanently lost
> from the weekday pattern.

```
Ship data collection to production. Collection only — no report screen in
this change.

WHY NOW: the live tablet deletes data at 30 days and has no ledger, so
every day this waits is a day permanently gone from the weekday pattern.

1. THE DAY LEDGER — one record per service. No names, no room numbers.
   Local-only, like the monthly ledger.
     - date, weekday
     - booked / attended / no-shows (entitled rooms only)
     - écarts, split three ways: not on the list / on the list without
       breakfast / more people than booked
     - first arrival, last arrival
     - the 15-minute arrival counts
     - peak count and peak time
     - staff on duty
     - paper-service marker
     - the planned window in force that day
     - a `service` key — always "breakfast" today, but Events will bring
       several a day and a key now beats a migration later

2. BACKFILL ON FIRST RUN from the 30 days still on the tablet. Without
   this, launch day starts at zero instead of four weeks. Say how many
   days you actually recovered.

3. STAFF COUNT — ask at the START of service, not the end.
   Nobody closes the day; at the end they are leaving. At 06:52 they are
   standing there waiting for the first guest.
   One question, two big buttons, three seconds: "Combien êtes-vous ce
   matin ?" A NUMBER ONLY — never names. The rota already knows who.
   Skippable ("Plus tard"), and askable again later from the same place.

4. FEEL CHECK — at the end of service, one tap, five faces, optional.
   "Clôturer sans répondre" gets equal visual weight.
   STORE AND SHOW THE MONTHLY AVERAGE ONLY. With a team of three, a daily
   rating is not anonymous — never expose a single day or a single person,
   anywhere, including to the director.

5. TWO PLANNED WINDOWS — weekday and weekend, per hotel. Save the window
   in force with each day so changing the setting later never rewrites
   history. Before building, check first-arrival by weekday across the 30
   retained days and tell me whether the weekend shift is actually there.

6. COMFORTABLE LOAD PER STAFF — a hotel setting with NO DEFAULT. Ship it
   empty. Pressure blocks stay blank until the hotel sets a number.

7. THE ÉCART PAYMENT PROMPT — remove the pre-set "Chambre", for écart
   cases ONLY. Right now the default silently resolves it, so nobody
   actually decides and we cannot see what reception did. Leave the
   default alone everywhere else.

DEPLOY: after service, and never before a Wednesday — it is the heaviest
day with the earliest peak. Keep the old path as fallback.

Tell me the plan before you write code.
```

---

## 2 · The report screen — after collection is live

```
Build the report screen. It reads the day ledger; do not change the
ledger in this task.

FILTERS, one row above the content:
  - 30 days / 60 days / custom date range
  - by department (filter, not a separate screen)
Defaults to 30 days, all departments.

SECTIONS, in this order:
  1. Covers per staff member at peak — the headline. Blank with an
     explanation if the comfortable-load setting is unset.
  2. The arrival curve: arrivals per 15 minutes, with the staffing level
     drawn as a line IN COVERS (staff x comfortable load), so there is
     only one axis. Where bars cross the line is where guests waited.
  3. Prepared vs served: servi / présent non servi / absent, and the
     total prepared and not served.
  4. Day-of-week averages, ALWAYS labelled "basé sur N semaines".
  5. A compact table of the month's measures.

RULES:
  - Never print a number that was not observed. No estimates, no
    industry averages. If it matters and we cannot see it, say what to
    store and leave it off.
  - No euro figures in the headline.
  - Print the assumptions at the bottom, editable per hotel.
  - One page when printed.

Design reference — the canvas board "Rapport mensuel":
https://claude.ai/artifact/VC8YJiF7AuMtEeNz5quTFB
Chart palette, already validated for colour-blind separation and
contrast: #0B6FB5 servi · #C8781E présent non servi · #8A4FA8 absent.
```

---

## 3 · Alba Events — not yet

Waits on the paper test with Aymard. When it starts, the build order is in
[`ALBA-EVENTS.md`](ALBA-EVENTS.md) — **build the shared spine once, then add role
defaults.** Not department by department: that is four apps, four times the work,
and four inconsistent interfaces.

1. Upload → parse → **confirm screen** → timeline
2. The shared chronogram, defaulting to "my tasks", with a "tout" toggle
3. Task detail, with the fields that vary by role
4. Status and the activity log
5. Role defaults — which department a person lands in
6. The contradiction check
7. Only then: the "raconte ce qui s'est passé" agent, text before voice
