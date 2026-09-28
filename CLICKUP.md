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
| Tue 29 Sep | Open Google Play and Apple Developer accounts (moved from Sat 26 Sep) | 📱 |
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

## State of the migration (28 Sep 2026, 13:45 UTC / 15:45 Paris)

**The replay is finished.** Nothing is left in `clickup-sync/`.

ClickUp's MCP limit is **100 calls a day for the whole workspace**, shared by
every session. This MCP server enables **no bulk operators**, so every create,
update, comment and rename costs one call. On 28 Sep this replay used
**87 calls** (13:41–13:45 UTC), so at most 13 are left for other sessions
today. The quota resets about 24 h after the day's first call: next reset
**Tue 29 Sep, ~13:30–13:40 UTC**.

**Done:**
- 24 Sep: folder renamed, 6 lists created, 3 lists renamed, ~40 tasks moved in.
- 25 Sep replay: **37 decision cards** created in 📌 Décisions:
  - D1–D23 and D25–D32 are `accepted`, dated the day each was decided (D11
    goes on Mon 21 Sep because 20 Sep was a Sunday).
  - Q1, Q2, Q4, Q7 and Q8 are `Open`, dated the decide-by day.
  - Q3 is `Closed` (answered by D28).
- 26 Sep replay (13:21–13:22 UTC, 51 calls landed before the shared limit ran
  out):
  - **Q9** created in 📌 Décisions (`Open`, due Fri 9 Oct).
  - **All 30 moves** done. D24, Q5 and Q6 are now in 📌 Décisions and renamed
    "D24 · 🧭 Positionnement…", "Q5 · 🏷️ Décisions ouvertes…" and
    "Q6 · 💰 Pricing & add-ons…". D24 is `accepted`, due 10 Jun; Q5 is due
    17 Oct and Q6 31 Oct (both still `Open`). Their date/status updates went
    in the same call as the rename.
  - **14 of 26 creates**: the monthly-report screen (🍳), the six 🤝 Commercial
    tasks, and in 🎪 Events the paper test, BEO template, "WFC" rename, the
    two missing screens, V1·1, V1·2 and V1·3.
- 27 Sep replay (13:31–13:34 UTC, 58 calls landed: 2 reads, then 56 writes):
  - **The last 12 creates**: V1·4, V1·5, V1·6 and the kitchen voice test (🎪),
    the six 📱 App tasks, the logo and the demo video (🌐). The two
    store-account tasks were due Sat 26 Sep and landed re-dated to
    **Tue 29 Sep**.
  - **16 decision cards**, D33–D48, in 📌 Décisions: `accepted`, D33–D44 due
    25 Sep and D45–D48 due 26 Sep. Same format as D1–D32: name
    "D<n> · <the bold line>", description "**Decision (date):** … / **Why:** …
    / **Source:** … · DECISIONS.md".
  - **All 13 journal cards** in 📓 Journal de dev (`completed`, due the day;
    9 Aug on Mon 10 Aug). `devlog-days.json` is now empty.
  - **14 of 86 updates**: the Q5 and Q6 comments, then the first 12 entries
    in the file, `86ba484mh` through `86ba484nj` (among them the re-date to
    Tue 29 Sep, with its comment, of the task that now books the October
    meeting).
- 28 Sep replay (13:41–13:45 UTC, 87 calls, all succeeded):
  - **The last 72 updates** (`wdy2xgv8xr` through `wdy2xgxfj1`) and their
    **7 comments**: backlog re-dated to Fri 13 Nov, the rest to their
    calendar dates, 6 cards marked `completed` or `Closed`.
  - **The 3 folder renames**: the emptied folders are now "🗄️ (vide) …" /
    "🗄️ (doublon vide) …".
  - **Two journal cards** in 📓 Journal de dev (`completed`, due the day):
    26 Sep (D45–D48: real BEOs read with Mistral, the Moment screen,
    « Salle prête », swiping days, the « Parler à Alba » voice screens) and
    27 Sep (D49–D51: Claude brain with skills, dictation mic, compliance
    loop, Légal page, the ElevenLabs call, hotel rules by talking to Alba,
    7-day chat memory, the call as a chip with spoken « c'est ça », Savoir
    de l'hôtel, the acquisition plan).
  - **The 14 Aug journal card** now names the objection-handling file only
    as "le fichier du groupe hôtelier" (no brand name on ClickUp).

**Still pending:** nothing. `clickup-pending.json` and `devlog-days.json` are
empty. No journal card yet for 28 Sep (D52): add it at the end of today's
session.

**Check:**
- Another session created cards on 25 Sep that are **not** in this file:
  - In 📌 Décisions: 4 unnumbered decisions ("La collecte part avant l'écran",
    "Aucune valeur par défaut pour ce qui fonde le rapport", "Un paiement
    enregistré veut dire que quelqu'un l'a choisi", "L'effectif est un nombre,
    le ressenti une moyenne mensuelle").
  - In 📓 Journal de dev: a journal card "2026-09-25 — Collecte d'abord…".
  - In 🍳 Petit-déjeuner: 6 dated tasks.
  - Copy the 4 decisions into [`DECISIONS.md`](DECISIONS.md), or renumber
    them.
- The create "🚀 Livrer la collecte de données en production" was dropped on
  25 Sep: "Merger la collecte et déployer…" (`wdy2xh1d4y`) does the same job.

The emptied folders (the old pilot-hotel folder, *Plateforme & Acquisition*, and
the empty duplicate *Check-in — Breakfast PWA* in the POS space) get renamed
"🗄️ (vide) …", not deleted. They were renamed on 28 Sep. Delete them yourself
once you've checked.

## Still valid from the 14 Aug plan — not built yet

- **A separate `Life` space** (North Star · Daily · Rhythm · Ideas), so business
  urgency never outranks it visually.
- **One dashboard, two numbers:** `MRR €0 / €5,000` and `Conversations this
  week: 0 / 10`. Nothing else on it.
- **Three automations, no more:** task enters *Contacted* → follow-up at +3
  days · task enters *Won* → onboarding checklist · every Friday → the weekly
  review task.
