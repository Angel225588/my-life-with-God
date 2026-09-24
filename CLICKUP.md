# ClickUp — how Alba is organised

Rebuilt 24–25 Sep 2026. Everything about Alba lives in **one folder**:
**Imarketin › 🌅 Alba** (the old "Check-in" folder, renamed).

## The lists

| List | What goes there |
|---|---|
| 📌 **Décisions** | One card per decision. `accepted` = decided; `Open` = to decide, with the date by which it gets decided. Mirrors [`DECISIONS.md`](DECISIONS.md). |
| 📓 **Journal de dev** | One card per working day: sessions, shipped, what broke + the lesson, decisions, still open. Mirrors `docs/DEVLOG.md` in the Alba code repo, plus the design/strategy sessions from this repo. |
| 🤝 **Commercial — pilote, signature, pipeline** | The pilot hotel (legal file, October meeting, signature, invoicing), then every other hotel. |
| 🍳 **Petit-déjeuner — en production** | Module 1 as it runs every morning: field bugs, data collection, the monthly report. |
| 🎪 **Alba Events — V1** | Module 2: the paper test, the 11-screen V1 build, the voice test. |
| 📱 **App mobile & stores** | PWA, Apple/Google accounts, Capacitor. |
| 🌐 **Site, démo & supports** | Landing, one-pager, deck, logo, demo video. |
| 🔒 **Sécurité, RGPD & plateforme** | Guest-data protection, and the server/sync work that waits for a signed DPA. |
| 🧭 **Roadmap — après signature** | The backlog. A date here is the day we decide whether it enters the plan — not a delivery promise. |
| 📖 **User stories — Check-in** | Unchanged (the Alba repo's devlog points to it by name). |

## The rules

1. **Every task has a date.** Real deadline, or — for backlog — the review date
   (the post-signature review is **Fri 13 Nov**). Never a Sunday.
2. **Every decision gets a card** in 📌 Décisions the day it's taken: what,
   why, what we rejected, where it's written, when to revisit it.
3. **Every working session ends with a journal card** in 📓 Journal de dev
   (the end-of-session prompt is in [`PROMPTS.md`](PROMPTS.md) §0).
4. **The repo wins.** If ClickUp and a file disagree, the file is right and the
   card gets fixed — say so in a comment, don't fix silently.
5. **Friday review** (task in 🤝 Commercial, re-dated each week): 10
   conversations? Decisions copied? Journal up to date? Anything overdue gets a
   new date *and a comment saying why*.

## The calendar — what's due, in order

| Date | What | List |
|---|---|---|
| **Sat 26 Sep** | Ship data collection to production (PROMPTS §1) | 🍳 |
| Sat 26 Sep | Mistral retention answer + hotel privacy contact | 🤝 |
| Sat 26 Sep | Open Google Play and Apple Developer accounts | 📱 |
| Tue 29 Sep | Book the October meeting date | 🤝 |
| **Wed 30 Sep** | Legal file complete · decide who signs the DPA (Q1) | 🤝 📌 |
| Fri 2 Oct | Paper test V0 with Aymard · template on the BEO task · security checks (API, first-visit allergy access) · re-check the two open field bugs | 🎪 🔒 🍳 |
| Sat 3 Oct | Go/no-go Events V1 (Q2) · do kitchen/restaurant need the calendar (Q3) | 📌 |
| Tue 6 Oct | Talk to the F&B manager and breakfast supervisor | 🤝 |
| Wed 7 Oct | Report screen ready for the meeting | 🍳 |
| Fri 9 Oct | Lawyer review · service agreement · one-pager + deck · logo · rename "WFC" · V1·1 | 🤝 🌐 🎪 |
| **Tue 13 Oct** | **Meeting with the director — report first, then terms** (date to confirm) | 🤝 |
| 14–28 Oct | V1·2 → V1·6 (if the paper test says go) | 🎪 |
| Sat 17 Oct | Landing page · demo video · domain + price shown? (Q5) | 🌐 📌 |
| Fri 23 Oct | Billing (SEPA/Stripe) · installable PWA | 🤝 📱 |
| Fri 30 Oct | First invoice | 🤝 |
| Sat 31 Oct | 20 hotel visits · Events price (Q6) · site pages | 🤝 📌 🌐 |
| Fri 6 Nov | Kitchen voice test → Whisper or Voxtral (Q7) | 🎪 |
| **Fri 13 Nov** | **Post-signature review** — every backlog card gets scheduled or re-dated | 🧭 🔒 |

## State of the migration (25 Sep, 00:30)

ClickUp's **daily API limit** stopped the reorganisation halfway. Done:
folder renamed, 6 lists created, 3 lists renamed, ~40 tasks moved in. Waiting
for the limit to reset, all recorded in [`clickup-sync/`](clickup-sync/):

- `clickup-pending.json` — 30 moves, 87 date/status updates, 27 new dated tasks,
  3 empty-folder renames
- `devlog-days.json` — 12 journal cards ready to create
- the 32 decision cards come from [`DECISIONS.md`](DECISIONS.md)

The emptied folders (the old pilot-hotel folder, *Plateforme & Acquisition*, and
the empty duplicate *Check-in — Breakfast PWA* in the POS space) get renamed
"🗄️ (vide) …", not deleted. Delete them yourself once you've checked.

## Still valid from the 14 Aug plan — not built yet

- **A separate `Life` space** (North Star · Daily · Rhythm · Ideas), so business
  urgency never outranks it visually.
- **One dashboard, two numbers:** `MRR €0 / €5,000` and `Conversations this
  week: 0 / 10`. Nothing else on it.
- **Three automations, no more:** task enters *Contacted* → follow-up at +3
  days · task enters *Won* → onboarding checklist · every Friday → the weekly
  review task.
