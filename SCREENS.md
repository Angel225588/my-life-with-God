# Why each screen exists

Every screen has to earn its place by answering **one question faster than
anything else in the building** — faster than a phone call, a walk to the
office, the paper BEO, or asking Alba.

That last one is the new test. The Alba chat will answer most questions: typed,
by voice message, or in a call. So a screen only survives if it is **faster than
asking** — because the answer is visible at a glance with full hands, or because
it is the place where something is *recorded* and checked.

> **Screens are for glancing and tapping. The Alba chat is for asking, telling,
> and changing.**

---

## Voice: only inside the Alba chat

**Decided 25 Sep (D25).** Voice is not a microphone on every screen. It lives in
one place, the Alba chat, which takes three inputs:

| Input | When | Example |
|---|---|---|
| **Type** | Quiet, precise | "À quelle heure la Lunch Box ?" |
| **Voice message** (hold to talk) | Hands busy, noisy room | "Trois sont venus mais ils veulent juste un café" |
| **Call** | Two or more decisions blocking people | Alba asks first: "Je peux vous appeler ? Une minute suffit." |

Voice by **ElevenLabs** (Alba's spoken voice, and the call). Reading documents
stays on Mistral.

### What makes it trustworthy — four rules

1. **Alba answers questions right away; it never changes anything silently.**
   Every change ends in a card: what changes, **who gets told** ("Je préviens :
   Cuisine, Emeline"), and *C'est ça / Corriger*. Nothing moves until a human
   taps.
2. **Every change Alba makes shows up in the task's Activité, with who
   confirmed it and a way to undo it.** Trust is being able to check.
3. **Screens work without Alba.** A chef with gloves on glances at the task; he
   doesn't talk to a chatbot. The chat is the fast lane for exceptions, not the
   only road.
4. **Allergies are never changed by voice alone.** Alba can take the message,
   but an allergy change is always shown as text and confirmed with a tap.

### Before it goes live

- **Listening is a separate choice from speaking.** ElevenLabs has its own
  speech-to-text (Scribe), so the kitchen test in [`V1.md`](V1.md) now compares
  **three**: Scribe, Whisper, Voxtral. Same 30 phrases, same bar (numbers, times,
  allergens 30/30).
- **ElevenLabs becomes a sub-processor.** Staff voices are personal data, and a
  voice message can contain a guest's allergy (health data). It must be named in
  the DPA, and its EU hosting and retention terms confirmed in writing — the
  same two questions already asked of Mistral. Until the DPA is signed (D9),
  test with made-up events only.

---

## The screens

**Time** = how long it takes to get what you came for. **Replaces** = what
people do today instead.

### V1 — what we build first

| Screen | Who | The one question | Time | Replaces | Why not just ask Alba? | Verdict |
|---|---|---|---|---|---|---|
| **Mise en route** | Director, once | "Is my hotel set up?" | 1 min | — | It happens once, before Alba knows anything | **Add to V1**: nothing else works without it |
| **Équipe** | Director | "Who's in, who's missing?" | 5 s | Asking around | Alba can *flag* "Technique hasn't opened the invite", but the list is the record | Keep |
| **Inviter** | Director / manager | "Get this person in now" | 20 s | Handing out logins | A phone number is safer typed than spoken; Alba can prefill it | Keep |
| **Inscription** | New staff | "Am I in?" | 30 s, no password | Account creation | First contact; has to work without explanation | Keep |
| **Déposer un BEO** | Commercial | "Hand over the document as it is" | 10 s | Printing and walking it round | Also possible *in* the chat (send the PDF) — same flow after | Keep |
| **Ce qu'Alba a lu** | Commercial | "Did Alba understand it right?" | 60 s | Everyone re-reading the PDF | **The first trust screen.** Checking 7 services is faster in a table than a paragraph. Mandatory (D16) | Keep |
| **Calendrier** | Everyone | "What's on today, and is anything wrong?" | 3 s | The printed week | Looking is faster than asking. "2 à trancher" at the top | Keep |
| **Chronologie** | Everyone | "The day in order — which parts are mine?" | 5 s | The BEO, pages 1–4 | A timeline is read, not asked for | Keep |
| **Tâche** (one template) | Whoever does it | "When, where, what, who helps — done?" | 3 s for time and place | Calling the commercial | Hands full; the hand-off button has to be one tap | Keep — **one template, every task is data** (D23) |
| **Mes tâches** | Everyone | "What do I do next?" | 2 s | Memory, WhatsApp | The glance at the start of a shift | Keep |
| **Profil** | Everyone | "Mute me off shift; sign out" | Rare | — | Settings aren't a conversation | Keep, minimal |
| **Alba** (chat) | Everyone | "Ask, tell, change" | 10 s to an answer | Phone calls, the office | — | **Add to V1**, see below |

### After V1 — the full map

| Screen | Who | The one question | Verdict and why |
|---|---|---|---|
| Alba · l'agent · dire ce qui change · l'appel | Everyone | Ask / change / decide | **Merge into one Alba chat** with type, voice message, call. Three screens for one conversation was the design talking, not the user |
| Cuisine (Jafar) | Kitchen | "What leaves when, with which allergy?" | **Merge**: it's *Mes tâches* sorted by ready-time, the kitchen default. Same screen, different sort |
| Commercial (Emeline) | Commercial | "Questions waiting for me?" | **Merge**: it's *Calendrier* with the questions card on top — already the same skeleton |
| Mes tâches · questions | Commercial | "What's waiting on me?" | **Merge** into *Mes tâches* |
| Tâche — le BEO | Commercial | "Is everyone up to date, who's seen the change?" | Keep for V2 (re-upload and diff). Redo it on the task template |
| Restaurant (Marine) | Restaurant | "How's the service going?" | Later. Its "say what happened" bar becomes a button that opens the Alba chat, **no microphone on the screen** |
| Réception (Léa) | Reception | "Which groups arrive, where do they go?" | Later. "Où est quoi" (turn the phone to the guest) is the part to keep |
| Direction | Director | "What needs my decision?" | Later: a dashboard has to be earned with real data. Split Événements / Petit-déjeuner (Q8) |
| Qui voit quoi | Director | "Who sees what?" | Later: V1 uses role defaults (D21) |
| Aujourd'hui en images | Everyone | "Show me the work is done" | Later: recognition, not operations |
| Rapport mensuel · Effectif · Fin de service | Breakfast | "Was the team set up to succeed?" | **Keep, module 1**: this is the October argument (D12, D13) |
| Month view, stay band | — | — | Later (D21) |

---

## What this changes on the canvas

- **Remove the microphone** from the Activité composer on every task screen.
  Typing and tagging stay; talking goes through Alba.
- **One Alba chat screen** replaces three, with the three inputs in its bottom
  bar.
- **Mise en route** and **Alba** join the V1 row.
- V1 rule D22 becomes: **buttons on screens, voice only in the Alba chat, and
  voice goes live after the kitchen test.**
