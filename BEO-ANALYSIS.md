# What the real BEO taught us

From the World Freight BEO, lundi 21 septembre 2026 — two printed versions of the
same document, photographed 24 Sep 2026.

---

## The finding

**The document contradicts itself.** Not between departments, not over the phone —
on the page, before anyone does anything wrong.

The lunch box for 14 people appears three times:

| Where | What it says |
|---|---|
| Event table | `11h15 – 13h` (v2) · `11h30` (v1) |
| Restaurant / Salon section | ready **11h30**, sent to Hall 78 before **11h45** |
| Kitchen section | ready **11h15**, sent to Hall 78 for **11h15** |

The kitchen and the restaurant are working from different instructions **off the
same sheet of paper.** Everyone follows their section correctly and the delivery
still lands in the wrong place at the wrong time. That's not a discipline problem
and no amount of phoning fixes it.

### And the versions drift silently

Between the two printouts, both dated for the same event:

- Club sandwiches: **11 chicken + 3 vegetarian → 12 chicken + 2 vegetarian**
- The named vegetarian list lost a name: **3 names → 2 names**

Fourteen either way, so the total looks right and nothing flags. If the kitchen
is holding the earlier sheet, one guest gets a meal they can't eat — and the
first anyone knows is at the table.

### The handwriting is the most valuable thing on the page

> *"12h05 = arrivée de 5 pax en attente"*
> *"12h15 = demande de lancement"*
> *"2 pax 12h30 ils sont pas venus."*

Someone is **already recording no-shows and service timing, by hand, on paper
that gets thrown away.** That's the number we spent a day looking for in the
database. It exists — it's just in biro.

And it has a billing edge: the Micros facturation bills **24 covers**. Two didn't
come. Nobody's system connects those two facts.

---

## The feature this creates

> **Alba shouldn't just distribute the BEO. It should find the contradictions in it.**

Extraction already has to read every section to split tasks by department. Once
it has, *"this delivery time appears three times with three values"* falls out for
free — and *"this quantity changed since the version you last saw"* comes with the
diff we were already planning.

Nobody else does this. Every BEO tool on the market helps sales **write** the
document. None of them read it back and ask whether it agrees with itself.

It's also the perfect demo, because it runs on **their own paperwork** and the
answer is undeniable. You aren't claiming their process is sloppy — you're
showing them a page that disagrees with itself, and no one in the room will
defend it.

**Positioning shift worth noting:** this makes Alba a *safety* tool, not just an
efficiency tool. Efficiency gets compared on price. Safety doesn't.

---

## The structure that came out of it

The BEO is messier to read than it is to model. Underneath, it's regular:

```
Event            client, purpose, nationality, dates, commercial in charge,
                 on-site contact, billing reference
└─ Day           21 septembre 2026
   └─ Service    time · type · place · setup · pax          ← the event table
      ├─ Requirements   per department, with quantities
      ├─ Constraints    allergies, intolerances, named guests
      └─ Billing line   Micros: unit price, qty, VAT
```

**The event table is already a clean schema** — Time / Type / Place / Set Up /
Number of people. That's the spine, and it's the same on every BEO this hotel
issues. Everything else hangs off a service.

### Departments fall out of the section headings

The document is already organised by who does what — it just repeats itself
across the sections instead of routing:

| Section in the BEO | Department | Example from this event |
|---|---|---|
| Front Office | Reception | digital logo + "Meeting", key to the on-site contact, 2 parking spaces |
| Restaurant / Salon | Service | U-shape ×2, coffee break setups, existing covers |
| Meeting | AV / Technique | paperboard, digital flipchart, HDMI + ClickShare, 2 mics, no visio |
| Kitchen | Cuisine | 14 Caesar, 12+2 club, 14 chocolate cake without ice cream, menu Issy |
| Facturation Micros | Finance | 6 lines, 10% VAT, €3 000 total |

**Routing is not a new idea to be invented — it's already in the document.** Alba
just has to stop everyone reading everyone else's section.

### Fields worth extracting first

Highest value per unit of effort, in order:

1. **Service rows** — time, type, place, setup, pax. The spine.
2. **Quantities with items** — "14 Caesar Salad", "2 micros". These are what get
   miscounted, and they're what the icon tiles show at a glance.
3. **Constraints** — allergies and intolerances. Getting one wrong is the worst
   outcome in the whole document, so it's the one that most deserves a screen of
   its own rather than a line in a paragraph.
4. **Time references anywhere in prose** — because that's where the contradictions
   hide. Extract every one, with the section it came from, and compare.
5. **Named-guest requirements** — "put the name on the vegetarian sandwich."
   Small, easy to drop, and highly visible when dropped.

---

## Data-protection note

This document is dense with personal data: client contact name, mobile, email,
the on-site contact, two named guests for parking, three named guests with
**dietary requirements** — which under GDPR sits close to health data.

**None of it goes in the prototype, the canvas, or any demo that leaves the
hotel.** The screens keep the operational content — times, places, setups,
quantities, allergy *counts* — and drop every identifier. The company is shown as
"WFC".

For a demo inside the hotel, with their own document, the real names are fine —
it's their data. Put them back locally if it helps the room. They should not live
on a hosted URL.

This also sharpens task 2 of [`PRODUCTION.md`](PRODUCTION.md): if Alba ingests
BEOs, it processes special-category data, and the DPA and retention rules have to
cover event documents, not just breakfast lists.

---

## What to do with this

1. **Run the extraction against this exact PDF** as the first test. If it finds
   the three lunch-box times, the feature is real.
2. **Hand Aymard the paper version this week** — his day, split by section, with
   the contradiction flagged. That's the V0 test in
   [`ALBA-EVENTS.md`](ALBA-EVENTS.md), and this document is the input.
3. **In October, show the contradiction before showing the app.** Put the two
   printouts side by side, point at the three times, then open the screen that
   catches it. The product sells itself from there.
