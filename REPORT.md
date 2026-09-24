# The monthly report — what to actually measure

> **Correction to what I wrote in August.** `LIFE-PLAN.md` and `MARRIOTT.md`
> originally built the value case on *"covers served that weren't on the entitled
> list, at €18 each."* **Alba cannot see that.** Payments live in Micros, and
> anyone eating who was never on the list is invisible to us.
>
> Putting that number on a report would be inventing revenue in front of an
> operations director who knows exactly which system holds it. One fabricated
> figure and every other number on the page becomes suspect. Both files are now
> corrected.

---

## What Alba actually observes

| | Can Alba see it? |
|---|---|
| Who was on the entitled list | ✅ Yes |
| Who showed up at breakfast | ✅ Yes |
| **Mismatches between the two — the écarts** | ✅ **Yes. This is the asset.** |
| Who paid by card or cash | ❌ Micros |
| Revenue recovered from an écart | ❌ Micros |
| People who ate and were never on any list | ❌ Nobody sees this |

So the honest headline is not *"we recovered €X."* It's:

> **"We caught N discrepancies this month that were passed to reception the same
> morning."**

That's a real number, produced by a real system, and it is **verifiable in
Micros** — which makes it stronger than a bigger number they'd have to take on
faith.

---

## The reframe — operations, not savings (24 Sep 2026)

> **Leading with money was a mistake, and it's corrected below.**
>
> A cost report gets read by finance and squeezed at budget time. It also invites
> the exact comparison already lost once — *"the QR app costs €89."* An
> operations and guest-experience report gets read by the operations director and
> **acted on**, and it's weighed against the cost of one bad Wednesday rather
> than against another app.
>
> It also sidesteps the Micros problem entirely. Attendance, no-shows, peak load,
> service windows, staffing — all observable, or one tap from a manager. Nothing
> to hedge, no revenue to claim.

**The new headline is not a number, it's a decision:** was the team set up to
succeed, and which days will need more people next month.

| Measure | Source |
|---|---|
| Attended / no-show, count and rate | Observed |
| Écarts — as an *accuracy* number, not a revenue one | Observed |
| Actual service start and end vs planned | Observed |
| Peak: busiest 15 minutes, and when | Observed |
| **Staff on duty** | One tap per service, by the manager |
| **Covers per staff member at peak** ← the headline | Derived |
| Pressure blocks above a comfortable load | Derived |
| Arrival curve, 15-minute buckets | Derived |
| **Day-of-week pattern across months** ← the planning payload | Derived |

**Don't survey staff satisfaction.** Subjective, slow, and easy for a director to
wave away. Measure the *conditions* instead — how many 15-minute blocks ran above
a comfortable covers-per-staff load. Objective, free, and much harder to dismiss.

**The single graphic that carries the whole argument:** the arrival curve with the
staffing level drawn across it. Where the curve rises above the line is where
guests waited. Everything else on the page is supporting evidence.

**The most valuable section is the day-of-week pattern.** If Wednesdays run
consistently heavier than Mondays across six months, that's a rostering decision
for the rest of the year — and Alba is the only system in the building that can
say it. Always print how many weeks the pattern rests on, so he can judge how far
to trust it.

The paste-ready prompt is task 4 in [`PRODUCTION.md`](PRODUCTION.md).

The line for the meeting, in his own terms:

> *"We don't want to sacrifice our reputation because we didn't want to invest.
> This tells you which Wednesdays need more people — before the guests find out."*

---

## Audit result and decisions (24 Sep 2026)

The Step 1 audit came back against the code. Summary of what it found and what
was decided.

### The only urgent finding

**Production is collecting nothing.** The ledger branch was never merged; the live
tablet runs `main`, which deletes at 30 days and has no ledger. **Every day before
this ships is permanently gone from the day-of-week pattern.**

→ Merge the **day ledger and staff tap alone** — collection only, not the screen.
Additive, low risk. Deploy after service and never before a Wednesday (the
heaviest day, earliest peak). Old path stays as fallback.

→ **The ledger must backfill from the 30 retained days on first run**, or launch
day starts at zero instead of four weeks.

### The écart fix worth making

Reception's payment prompt is **pre-set to "Chambre"**, so nobody actively decides
and we can't see whether an écart was resolved.

→ For écart cases only, no pre-selection. Reception makes a real choice, and
"what reception decided" becomes observable. That's the difference between *"we
showed a prompt"* and *"reception resolved 47 discrepancies."* Leave the default
alone everywhere else.

### Decisions

| | Decision |
|---|---|
| **Comfortable load per staff** | **Ship unset.** The 12 on the mockup is an illustration, not a default. Let the director set it in the meeting — *"at what point does your team start falling behind?"* A number he chose is one he won't argue with, and it makes him an owner of the report. |
| **Planned window** | **Two — weekday and weekend.** Hotels run later at weekends and one window would flag every Saturday as a late opening. Confirm against the 30 days first. Store the window in force with each day; a setting change must never rewrite history. |
| **Filter by service** | **Drop it** — my sloppiness, written generically. One breakfast service here, so filters are by day and by weekday. Keep a `service` key on the record anyway: Events will bring seven a day, and a key now beats a migration later. |
| **Merge** | **Now.** Collection only, after service, not before a Wednesday. |

### What the audit corrected in its own earlier work

Worth recording, because both would have put a wrong number in front of the
director:

1. The no-show denominator counted walk-ins added during service, and offset
   extra guests against no-shows. Correct basis: per room, booked minus came,
   entitled rooms only.
2. A room corrected from 2 to 3 people made the third guest count as entitled.
   Entitlement is judged against the sheet's **original** count.

### What this means for October

The weekday pattern will be ~4 weeks deep. Thin, so it goes last.

**Lead with the arrival curve** — fully available from the 30 retained days, and
the strongest graphic on the board. Then the three-way split, then the peak.
Show the pattern labelled *"basé sur 4 semaines"* and say the true thing: it gets
stronger every week they keep using it. A report that improves the longer they
stay is a retention argument, not an apology.

---

## The earlier version — money-led

Kept because the écart wording is still right whenever money does come up, and
because the discipline about unobservable numbers applies to every version of
this report.

### The headline

> ## 47 écarts detected in September
> Guests who appeared at breakfast without a valid entitlement on file.
> Each was flagged to reception the same morning.

### The money line — carefully worded

> At an average breakfast rate of €18, these represent **up to €846 in billing
> corrections**. Settlement is recorded in Micros — your reception team can
> confirm how many were recovered.

Three things this does, and all three matter:

1. **It never claims revenue Alba didn't see.** Credibility intact.
2. **It invites verification instead of asking for trust.** An executive who
   checks and finds it accurate becomes an advocate.
3. **It makes the Micros integration obvious** — *"connect us to Micros and this
   stops being 'up to' and becomes exactly."* That's the natural upsell, and it's
   a strong thing to raise in October without asking for anything.

### The second number — no-shows

Available today, same data inverted: guests who **were** entitled and **didn't
come**. And in one way it's a stronger number than the écart, because it needs no
hedging at all. Écarts have to say *"up to"* because settlement lives in Micros.
No-shows are fully observable by Alba — we know precisely who was entitled and
precisely who didn't arrive. Nothing to qualify.

> ## 18% no-show rate
> 562 entitled guests did not attend. Highest on Tuesdays (23%), lowest at
> weekends (11%).

**The value isn't the count — it's the predictability.** A chef who learns that
Tuesdays run 23% no-show preps 23% less on Tuesdays. That's food cost, every
week, forever — and Alba is the only system in the building that could tell them.

Report the **count and the rate, broken down by day of week**. Do not convert it
to euros of food waste: we don't know what the kitchen prepares per head, and the
same discipline applies as everywhere else on this page. Give them the pattern;
let the chef price it. They'll do it more accurately than we could, and they'll
believe their own arithmetic.

### Then, small, underneath

- **Reliability** — 30 of 30 mornings covered, 0 fallbacks to paper
- **Volume** — 3,120 covers processed
- **Staff time** — ~17 hours not spent on lists and reconciliation
- **Peak handled** — 84 covers in the busiest 15 minutes

### And at the bottom, always

> *Based on: €18 average breakfast rate, 20 seconds saved per cover. Both
> editable in settings.*

Showing the assumptions makes the numbers **arguable instead of magic**. An
executive who can adjust your inputs trusts your outputs. One who can't, doesn't.

---

## Two things that would make this credible in a week

**1. Measure the seconds-per-cover once, with a stopwatch.**
Stand next to the breakfast list for one service and time the reconciliation.
Ten minutes of work replaces my assumed 20 seconds with a measured figure you can
defend when he pushes on it. He will push on it.

**2. Get the "before" quote from your manager.**
*"It used to take me forty minutes every morning to reorganise that list."*
One sentence from her is worth more than any calculation on the page, because
it's the one thing on the report he can't argue with.

---

## The manager's version is a different report

Don't send the director's report to the F&B manager. Different job entirely.

| | Executive — monthly | Manager — daily / weekly |
|---|---|---|
| Question | *Is this worth what we pay?* | *What do I do about it?* |
| Écarts | A count | **A list, with room numbers, to chase** |
| Covers | A total | By service, by day, for staffing |
| Peaks | One figure | When, so they can roster against it |
| Length | One page | A working screen, not a document |

The manager's version drives daily use. The executive's version renews the
contract. Build the executive one first — that's the one due next week.

---

## The rule for every number on this report

> **If Alba didn't observe it, it doesn't go on the page.**

Not estimated, not extrapolated, not "industry average." If it's worth showing
and we can't see it, that's a roadmap item — most likely the Micros integration —
not a number to approximate.

The report's entire power is that it's auditable. Protect that.
