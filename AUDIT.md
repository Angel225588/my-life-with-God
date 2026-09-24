# Alba Events — design audit, 24 September

Every screen on the canvas walked as a first-time user, department by department.
Mechanical checks first (every link, every exit), then the judgement calls.

**Canvas:** https://claude.ai/artifact/VC8YJiF7AuMtEeNz5quTFB — 29 screens, now
**one row per person**, left to right in the order they meet the screens. Press
Play on the first screen of a row and follow the links.

---

## Verdict

**Before this pass: a first-time user would have got lost.** Seven screens could
not be reached in Play at all, Jafar's home could not open his own task, and the
"Profil" button sent everyone — cook, receptionist, director — to a screen that
said *"Bonjour Aymard"*.

**After this pass: every screen is reachable, none is a dead end, and each person
has the same bottom bar** (Événements · Mes tâches · Profil), a back arrow on every
page, and a first day that is not empty.

What still stands between this and "they will just love it" is listed under
**Still open** — the biggest is that the design now leans on Alba (the agent)
everywhere, and the agent is the hardest thing to build well.

---

## Bugs found and fixed

### Broken flows

| Problem | Fix |
|---|---|
| **Jafar's kitchen list could not open the Lunch Box task** — no link | The row now opens the task |
| Kitchen home still said **"Marquer prêt"** — the button you replaced with "Prêt à récupérer" weeks ago | Removed; the handover lives on the task. Kitchen gets the common bottom bar |
| **"Profil" went to Aymard's greeting splash** for every role | New **Profil** screen: photo, bio, notifications, what I see, sign out |
| Director's **"Rapport complet →" linked to itself** | Opens the monthly report |
| Director had **no way to reach the team settings** | Team button in the header → Équipe et départements |
| Director's Réception tile went nowhere | Opens Léa's Arrivées et départs |
| Réception's back arrow sent Léa to Aymard's splash | Goes to the calendar |
| Setup (room in U), Start-of-service, End-of-service, Report, KitchenTask: **unreachable** | All wired in: Timeline → Setup, Kitchen → KitchenTask, Start → End → Report |

### Contradictions between screens

| Problem | Fix |
|---|---|
| **Director said 2 847 covers, 18 % no-shows, 28 per person, 11 tense slots. The report said 2 561, 13 %, 14, 9.** | Director now matches the report |
| Timeline showed the room set-up as **"Fait" at 12:00 — in the future**, when "now" is 11:32 | Morning set-up moved to 10:30 |
| Setup screen said **Foyer -1, ready by 10:30** — the timeline and import said **Hall 78** | Setup is now the 13:00 re-set of Hall 78 for 24, ready by 12:45 — which is what the BEO actually asks |
| Aymard's "Mes tâches" **listed Marine's 16:45 coffee break as his**, and **missed his own room set-up** | List now matches the timeline; room set-up opens its task |
| **"v1 / v2 / v3" still on 5 screens** after you decided there are no versions, only "à jour" | Rewritten in plain language everywhere |
| Director avatar "OD", but the director is Sophie | Replaced by team + Alba buttons |
| KitchenTask said **"TOI"**, Restaurant said **"Dis ce qui se passe"** — everywhere else says *vous* | *Vous* everywhere |

### Privacy

| Problem | Fix |
|---|---|
| A canvas note named the hotel chain's **loyalty programme** — which identifies the chain you don't yet have permission to name | Removed |
| Import screen named the **real client company** | "société cliente" |

---

## Still open — decide or build next

1. **Alba always opens Marine's conversation**, from whichever screen you tap the
   star, and closing it returns to the restaurant. The canvas cannot remember
   where you came from; the product must. *Build note, not a design flaw.*
2. **"Accueil" (Bonjour Aymard) is one person's splash sitting in the shared
   row.** Recommendation: drop it as a separate screen. Its three numbers and the
   "2 incohérences" line become the top of the calendar. One less tap every
   morning.
3. **Kitchen and Restaurant don't have the Calendrier | Mes tâches tabs** the
   others share. Kitchen now has the bottom bar; Restaurant's bottom belongs to
   the composer. Decide whether those two roles need the calendar at all.
4. **Technique has no designed screen** ("Tâche technique"), and Réception's
   **"Plan des salles"** is promised but not drawn.
5. **The client is still called "WFC" on 18 screens.** Fine privately. Rename to a
   fictional client before showing this canvas to any other hotel.
6. **Two products on one director screen.** The top half is Events (decisions);
   the bottom half is the breakfast report. Worth a small switch — *Événements ·
   Petit-déjeuner* — so it's clear which product a number comes from.
7. Screens are snapshots of slightly different moments (KitchenTask says 10:52,
   the rest 11:32). Acceptable in a prototype; don't let it leak into the demo
   script.

---

## What is working — keep these, they are the product

- **Contradictions surfaced, not hidden.** The lunch box with three times is why
  a director says yes.
- **Three layers of information:** a *decision* stays until someone decides;
  *news* shows 3 seconds and files itself; *memory* lives in Activité.
- **"Je préviens : …" before you confirm.** Naming who gets told is what makes
  Alba trustworthy instead of magic.
- **Tagging interrupts; writing in the log is free.** The rule that keeps people
  from muting the app in week three.
- **A switch hides a screen, never a task.** Permissions can't break the work.
- **Kitchen sorted by ready-time, not event time** — the exact bug in the BEO.
- **Handover buttons** ("Prêt à récupérer", "En place en Hall 78") instead of
  status pickers.
- **"Où est quoi" for reception** — turn the phone to the guest.

## What I'd cut or delay for version 1

The canvas has 29 screens. That's a map, not a first release.

**Ship first (the loop that proves the value):** Calendrier · Chronologie ·
Tâche · Mes tâches · Dépôt du BEO + lecture · Inscription · Inviter.

**Delay:** month view, Stories, the one-minute call, fine-grained permission
switches (use role defaults only), the stay band. All good ideas; none needed to
prove that Alba catches the lunch-box mistake.

**The real risk:** the design now relies on Alba understanding free speech
("3 sont venus mais ils veulent juste un café"). If it misreads once in front of
a chef, trust goes. Ship v1 with **buttons first, voice second** — every voice
action already ends in "C'est ça / Corriger", which is the right safety net.

---

## Engagement — the Snapchat / Instagram question

**The honest version first:** for a work tool, *hours spent in the app* is the
wrong target. A waiter scrolling Alba for an hour is a waiter not serving. The
goal is **opened every shift, answered within minutes, trusted** — and a
pleasant, fast feeling every time.

So borrow the mechanics that make those apps **fast and pleasant**, and leave the
ones that make them **addictive**.

### Built in this pass

**Aujourd'hui en images** — Stories, made for work. A row of circles at the top
of the calendar. Each one is a **photo of a finished task**: the room dressed in
U, the 14 lunch boxes with the two vegetarian ones labelled, the coffee served.
Tap through like Instagram. Two reactions only — **Bravo, Merci** — and they land
in that task's activity.

Why this one: it's recognition people actually feel, and it's **proof** — the
director sees the room is set without walking to level -1.

**Rules it needs before it ships:** rooms and food only, **never a guest's
face**, and photos expire with the event.

### Worth building next

| From | Mechanic | In Alba |
|---|---|---|
| Mail, Tinder | **Swipe** | Right on a task = *Je le prends*; left = *plus tard* |
| Snapchat | **Voice first** | Already there: hold the mic, Alba writes it |
| Duolingo | **Streaks — team only** | *"12 services sans oubli d'allergie"*, shown to the team, never per person |
| iOS | **Haptics** | A small tap in the hand when a handover lands — the "done" moment should feel good |
| Spotify Wrapped | **Weekly recap** | Monday: *votre semaine* — services, moments, thanks received |
| Snapchat | **Open straight to what matters** | Calendar with today's alerts on top — never a menu first |

### Don't borrow

- **Infinite feeds** — there's always an end: "rien d'autre aujourd'hui".
- **Public counts and likes** — no "Aymard has 12 Bravos".
- **Individual leaderboards** — a kitchen is a team; ranking people breaks it.
- **Red badges for non-urgent things** — badge fatigue is how apps get muted.

### How we'll know it's working

| Signal | Target |
|---|---|
| Staff who open Alba at least once per shift | > 80 % |
| Median time for a question to get an answer | < 5 min |
| Handovers done in the app vs by phone | trending up month on month |
| Photos per event | ≥ 1 per service |
