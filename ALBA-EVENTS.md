# Alba Events — the BEO problem

Captured 24 Sep 2026, from a problem I watched happen at work.

---

## The problem, as I saw it

Sales closes an event with a group or a company. They send out one PDF containing
everything: room setup, microphones, AV, a pastry breakfast with coffee and juice,
timings, headcount. It goes to every department at once.

Then:

- Nobody can tell which parts of a twelve-page document are **theirs**
- Everyone phones everyone else to confirm their piece is being done
- The details change constantly, and each change means another round of calls
- Information gets lost on the way

My own manager at the restaurant spent her own time **reorganising that PDF into
categories** so her team could read it. That's a person doing manual labour to
compensate for missing software — the strongest product signal there is, and I
watched it happen in front of me rather than imagining it.

## It has a name

This is the **BEO — Banquet Event Order**. Every hotel with an events business
has this exact document and this exact problem. The pain is universal, expensive,
and well known inside the industry.

---

## Why my angle is the right one

Tools exist: Tripleseat, Event Temple, Cvent, Amadeus Delphi, Planning Pod.

**They are all sales-side.** They help the sales team *build* the BEO and manage
the pipeline — proposals, contracts, invoicing. They're sold to sales directors.

What I'm building is **the receiving end**: the kitchen, AV, housekeeping and
front desk who get the finished PDF and have to execute it. Almost nobody serves
that side, because the buyer is less obvious.

That's a far better wedge than competing with Tripleseat:

| | Competing with Tripleseat | Alba Events |
|---|---|---|
| Replaces the sales system? | Yes — rip and replace, data migration, a fight | **No** — sits downstream of whatever makes the PDF |
| Scope | Pipeline, proposals, contracts, invoicing | Distribution and execution |
| Buyer | Sales director | **Operations director** |
| Build size | Years | Weeks |

The operations director is exactly who I'm meeting in October. This module is the
reason he cares about Alba at all.

---

## The insight worth building around

> **The pain isn't the first PDF. It's version five.**

Everyone can read a document once. Nobody has a system for *"the pastry order
changed at 16h yesterday and the kitchen doesn't know."* That's where the phone
calls actually come from, and it's where events actually go wrong.

**So the moat feature is: re-upload the revised BEO → diff it against the previous
version → notify only the people whose tasks actually changed.**

Small. Buildable. Nothing on the market does it well. And once a team relies on
it, going back to a PDF is unthinkable.

---

## Scope — what NOT to build

I described: upload, auto-assignment, a calendar, per-department dashboards, an
overall dashboard, icons and imagery, event messaging, private messaging.

That's six months, and it's exactly how this becomes project #17.

### ❌ No messaging

They have WhatsApp. I will not beat an entrenched group chat, and trying costs
months. Build **comments on a specific task** instead — contextual, attached to
the thing being discussed, and actually better than a chat thread. Push
notifications out to what they already use.

### ❌ No dashboards first

The dashboards are the **director's** product. Amar's product is task clarity.
A dashboard with no underlying usage is an empty dashboard — it has to be earned
with real task data first. Build Amar's version, let it run for a month, and the
director's dashboard fills itself.

### ⚠️ Extraction must be confirmed by a human

The AI will get things wrong. **A wrong setup time sent to the kitchen is worse
than the messy PDF** — it's confidently wrong instead of merely unclear.

So: extraction proposes, a human confirms, and confirming is fast. In operations
software, being wrong destroys trust far faster than being absent. This is not an
optional polish step; it's the difference between a tool they trust and one they
quietly stop opening.

---

## The screen pattern — same skeleton for everyone

Four screens, identical for every role. What changes is **what fills them**, never
the shape.

| # | Screen | Same for everyone | Differs by role |
|---|---|---|---|
| 1 | **Accueil** | Date, greeting, today in three numbers, one primary action | Which numbers matter; the action (Aymard: *Déposer un BEO*) |
| 2 | **Calendrier** | Day strip, calendar icon for any date, events listed per day | Which events are shown first |
| 3 | **Chronologie** (l'événement) | The full day, every service, in time order | **Mine highlighted, others greyed** — same list, different emphasis |
| 4 | **Tâche** | Status, details, requirements, allergies, activity, comments | Which requirements appear: quantities for the kitchen, equipment for AV, covers for the restaurant |

**Nobody gets a screen someone else doesn't have.** One layout to design, one to
build, one to explain to a new starter — and when two people are on the phone,
they are looking at the same thing.

## The shared components

The pieces that repeat, so they're built once and never redrawn:

| Component | Where | Rule |
|---|---|---|
| **Allergy card** | Tâche · Cuisine · Restaurant | Red, at the top, content always visible. The button only **records** that someone read it, and gates the next action |
| **Mine / not mine row** | Chronologie, Mes tâches | Mine: white, solid left bar, full contrast. Theirs: flat grey, department named. Never colour alone |
| **Status** | Everywhere | Dot **and** word. Green = Fait, amber = En cours, grey = À faire. Green means *only* that — never "mine" |
| **Menu block** | Restaurant · Cuisine | Course, plate count, and the constraint attached to **the dish it affects** |
| **Quantity tile** | Cuisine · Tâche | Icon, number, item, and its constraint underneath |
| **Say-what-happened bar** | Restaurant, later everywhere | Dictate or type; Alba proposes; a human confirms |
| **Activity log** | Every screen | Top-right clock button. Reading at the top, acting at the bottom |

### Two rules that decide the rest

**Safety information is never behind a tap.** The allergy card shows everything
immediately. The button exists to record the read, not to reveal it. If the one
time someone doesn't tap is the time a guest is harmed, the design failed.

**Alba raises conflicts as questions, never resolves them silently.** It knows a
constraint and it knows a dish — so when the Caesar has parmesan and a guest is
*sans fromage*, it says so. Where it isn't certain (anchovies in the dressing?) it
prints **à vérifier** rather than guessing. Right nine times and wrong once is
worse than asking every time.

## The build order

### V0 — this week, no code

Take one real BEO. Split it by hand into per-department one-pagers — exactly what
my manager already does manually. Give them to Amar for one event.

- He says *"can I have this next week too"* → build it
- He shrugs → I just saved three months

Either way I know by Friday. **Do this before writing a line of code.**

### V1 — the only thing that matters (2–3 weeks)

> Upload the messy PDF → each department sees only their part, as a timed checklist.

- PDF → structured event (same Mistral document pipeline as the breakfast lists)
- **Human review and correction screen** — mandatory, see above
- Auto-split by department
- Each person sees: my tasks, timed, today and this week
- Mark done

### V2 — the moat

- Re-upload → diff → notify only affected people
- Comments on a task
- The director's overview, now that there's real data in it

### V3 — everything else

Calendar sync, icons and imagery, per-department dashboards, the rest.

---

## The 10-cover story — why "came / didn't come" is too crude

Real service, told 24 Sep: 10 expected, kitchen prepped 10, starters plated.
5 arrived early and started. 2 came later and ate. 3 came later and **only wanted
a coffee**.

That last group breaks the model. They are not no-shows — they walked in. But a
cover was prepared for them and thrown away. A binary counter records them as
"arrived" and the waste vanishes.

**So every expected cover has three outcomes, not two:**

| | What it means | Who it matters to |
|---|---|---|
| **Servi** | Ate the meal | Billing, satisfaction |
| **Présent, non servi** | Showed up, took nothing (coffee only, arrived too late, changed their mind) | **Kitchen — this is waste that looks like attendance** |
| **Absent** | Never came | Kitchen, and the billing conversation |

Waste = *présents non servis* + *absents*. In the report that's one sentence:
**"559 couverts préparés sans être servis."** The chef can act on it; a no-show
percentage alone he cannot.

It also fixes the billing question. 10 prepared and billed, 6 eaten, is a
conversation with the client — and Emeline needs it the same day, not at month
end.

**Arrivals come in waves**, so each one is timestamped. That's not extra work for
Marine — it's the same button press — and it's where the arrival curve comes from.

---

## The "raconte ce qui s'est passé" agent

Recording that story structurally takes six taps across three screens, during
service, with her hands full. Saying it takes eight seconds:

> *"On attendait 10, 5 sont arrivés à 12h05, 2 à 12h30, et 3 sont venus juste
> pour un café."*

That's how staff already talk to each other. So the button exists on Marine's
screen: **Dicter** or **Écrire**.

### What happens after she speaks — the flow

1. **She talks.** Free text or voice. No form, no fields.
2. **Alba proposes a structured change** and shows it: `7 servis · 3 présents non
   servis · 0 absent · lancement 12h15`. Nothing is saved yet.
3. **She confirms with one tap.** This step is not optional. An agent that writes
   to an operational record unsupervised will eventually be confidently wrong, and
   a wrong record is worse than no record — the same rule as the BEO extraction.
4. **The numbers update** — service, daily, monthly, the report.
5. **One entry lands in the event log**, visible to everyone on the event. Not a
   message, a fact with a timestamp and a name on it.
6. **Only affected roles are notified**, and each in their own terms:
   - Cuisine — *"3 couverts préparés non servis"*
   - Commercial — *"10 facturés, 7 servis — à vérifier avec le client"*
   - Direction — nothing, unless it crosses a threshold
7. **Questions go back on that log entry**, as a reply. Not a new phone call, not
   a new WhatsApp thread. The question and the answer stay attached to the thing
   they're about, and the next person to look sees both.

### What it must not be

- **Not a chat.** One structured entry per event, not a conversation to scroll.
- **Not autonomous.** It proposes, a human confirms. Always.
- **Not V1.** Text first, voice later — they're the same pipeline, and the text
  version is a fraction of the work. Voice is the right end state for a restaurant
  floor, but it isn't where to start.

### Why it fits

Mistral is already the provider, already EU-hosted, already under the DPA being
written for the BEOs. Transcription and structured extraction are the same two
capabilities that parse the breakfast lists and the event orders. **This is a
third use of one pipeline, not a new system.**

---

## Amar

A named user with the problem **today**, who I already work with. That's worth
more than any market research, and it's the thing that makes this different from
Raizane or Vox — those had no user, this has one before it has a line of code.

Give him the V0 paper version first. Then V1 access as the responsible for his
area. Watch him use it without helping. Write down every complaint verbatim.

---

## What it does to the business

| | Alba breakfast only | Alba + Events |
|---|---|---|
| Departments touched | 1 | 4 |
| Price | €99–179 | **€349–499** |
| Properties needed for €5,000 | 28 | **15** |
| If they cancel | Stop a subscription | Retrain four departments |

It changes what Alba *is*: from a tool the F&B manager uses, into the system the
hotel runs its mornings and its events on.

**And this is the Raizane thesis working exactly as written** — win a beachhead,
then widen inside the vertical. Events is module two. Not a deviation from the
plan; the plan doing what it was supposed to do. The PDF-to-structured-data
pipeline is even the same one already being built for breakfast lists.

---

## The October meeting — the part that decides everything

> ## ⚠️ DO NOT OPEN WITH EVENT SCREENS.

He said Alba was too expensive for its value. If I walk in with a **new product**,
what he hears is: *"he's building more features instead of proving the first
one."* The €149 objection comes straight back, now attached to something bigger.

**The order:**

1. **Open with the value report.** *"Here's what Alba did for you in September —
   X covers, Y staff hours, Z covers served that weren't on the list, €N."*
2. **Then** the events concept. *"And here's where this goes next."*

Same meeting, same screens, opposite outcome:

- Report first → a vendor proving value and expanding. Events makes the platform
  look inevitable.
- Screens first → a developer with a new idea. Events makes the price look worse.

**Which means the monthly value report is still the single most important thing
to build before that meeting.** Task 4 in [`PRODUCTION.md`](PRODUCTION.md).
Nothing in this file changes that — it raises the stakes on it.

---

## One practical caution

I work at this hotel. Amar is a colleague. My manager is a colleague. That's an
enormous advantage — real access, real problems, a free test user — and it's also
worth keeping clean:

- **Build on my own time and my own equipment.** French law on employee
  inventions is not nothing, and an employer can have a claim on work created
  with their resources or on their time.
- **Ask before using real BEO data**, even for testing. Guest and client names in
  event orders are personal data, and the same GDPR logic as the breakfast lists
  applies.
- Be straight with them about the vendor relationship as it forms. A quiet
  conflict discovered later is far more expensive than an awkward conversation
  now.

None of this is a reason not to build it. It's a reason to build it cleanly.
