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

## State of the migration (25 Sep 2026, 14:20 UTC / 16:20 Paris)

ClickUp's MCP limit is **100 calls a day for the whole workspace**, shared by
every session. The next reset is **Sat 26 Sep, ~13:10 UTC**. This MCP server
enables **no bulk operators**, so every create, update and move costs one call.
The rest (≈190 items) takes about two more days of quota.

**Done:**
- 24 Sep: folder renamed, 6 lists created, 3 lists renamed, ~40 tasks moved in.
- 25 Sep replay: **37 decision cards** created in 📌 Décisions:
  - D1–D23 and D25–D32 are `accepted`, dated the day each was decided (D11
    goes on Mon 21 Sep because 20 Sep was a Sunday).
  - Q1, Q2, Q4, Q7 and Q8 are `Open`, dated the decide-by day.
  - Q3 is `Closed` (answered by D28).
- D24, Q5 and Q6 were **not created**. They already exist as cards
  (`86baby7av`, `86babzz36`, `86baby7c6`) that the pending moves bring into
  📌 Décisions, with their status and dates set in the updates. Rename them to
  "D24 · …", "Q5 · …" and "Q6 · …" once they have moved.

**Still pending**, recorded in [`clickup-sync/`](clickup-sync/):

- `clickup-pending.json`:
  - `decision_creates`: 1 item (Q9, which hit the limit).
  - `creates`: 27 new dated tasks.
  - `updates`: 87 date/status changes. Entries with a `comment` need an
    extra call for the comment.
  - `moves`: 30.
  - `folder_renames`: 3 empty folders.
- `devlog-days.json`: 13 journal cards (the 12 days plus 25 Sep, the UI/UX
  audit day).

**Check before replaying:**
- Another session created cards on 25 Sep that are **not** in this file:
  - In 📌 Décisions: 4 unnumbered decisions ("La collecte part avant l'écran",
    "Aucune valeur par défaut pour ce qui fonde le rapport", "Un paiement
    enregistré veut dire que quelqu'un l'a choisi", "L'effectif est un nombre,
    le ressenti une moyenne mensuelle").
  - In 📓 Journal de dev: a journal card "2026-09-25 — Collecte d'abord…".
  - In 🍳 Petit-déjeuner: 6 dated tasks.
  - Copy the 4 decisions into [`DECISIONS.md`](DECISIONS.md), or renumber
    them.
- Pending create "🚀 Livrer la collecte de données en production" does the
  same job as the new "Merger la collecte et déployer…" (`wdy2xh1d4y`, urgent,
  due 25 Sep). Drop the create rather than make a duplicate.

The emptied folders (the old pilot-hotel folder, *Plateforme & Acquisition*, and
the empty duplicate *Check-in — Breakfast PWA* in the POS space) get renamed
"🗄️ (vide) …", not deleted. Delete them yourself once you've checked. Their
renames are still pending.

## Still valid from the 14 Aug plan — not built yet

- **A separate `Life` space** (North Star · Daily · Rhythm · Ideas), so business
  urgency never outranks it visually.
- **One dashboard, two numbers:** `MRR €0 / €5,000` and `Conversations this
  week: 0 / 10`. Nothing else on it.
- **Three automations, no more:** task enters *Contacted* → follow-up at +3
  days · task enters *Won* → onboarding checklist · every Friday → the weekly
  review task.
