# ClickUp — what's there, and what's missing

Read on 14 Aug 2026. The API went down partway through, so this is based on one
successful read of the Imarketin space — good enough to see the shape.

---

## What's there

**Space: `Imarketin`** (`90143235266`)

| Folder / List | Contents |
|---|---|
| 📁 **Check-in** | `Live · Marriott`, `Security · Backlog`, `Sale version · Roadmap`, `📖 User stories` |
| 📁 **Imarketin Hôtellerie — Plateforme & Acquisition** | `🔒 Fondations — Sécurité & Sync`, `🗺️ Produit — Vision, Rôles & Roadmap`, `🗄️ Supabase — Migration & Sync` |
| 📋 **🔥 Landing — Ember Build** | G-011 home reframe, G-012 languages, G-013 product site |

The organisation is genuinely good. Epics, user stories, a live customer list,
a security backlog. This is not a messy workspace — it's a well-run engineering
project.

## What's missing

**Every single list is a build list.**

Epics, user stories, sync architecture, Supabase migration, UX research, landing
rebuilds. Out of everything I could see, the closest thing to commercial work was
a security task labelled *"#1 pre-sale."*

There is no list of hotels to contact. No pipeline. No record of conversations.
No pricing. No follow-ups. **Nothing that turns a built thing into a paid thing.**

This is the same finding as the Vercel account, in a different tool: 16 deployed
projects, 0 customers; a beautifully organised backlog, 0 prospects. The tools
aren't the problem. The tools are faithfully reflecting where the attention goes.

> The folder is called **Plateforme & Acquisition**. Everything under it is
> platform. Nothing under it is acquisition.

---

## The exception — three build tasks that really are sales prerequisites

I've been saying "sell before you build." Here is the honest exception, because
this is France and this is guest data:

- `Lock down /api/* (Gemini cost exposure) — #1 pre-sale`
- `Rate-limit + body-size + PDF magic-byte hardening; Gemini DPA + Supabase RLS`

**These are not busywork and they are not procrastination.** An open API endpoint
that bills Gemini per call is a real financial hole. And selling software that
processes hotel guests' personal data in the EU **without a DPA in place and RLS
switched on** is a GDPR exposure — for me *and* for the hotel that trusts me.
Marriott's own compliance people will eventually ask, and the answer has to
already exist.

**So: these get done in week 1, alongside the Marriott commercial terms.** They
are the one legitimate "build first." Everything else in the backlog waits behind
a paying customer asking for it.

---

## The restructure — small, not a rebuild

Keep everything that exists. Add what's missing. Two changes only:

### 1. A pipeline list — `💰 Acquisition · Pipeline`

Inside the existing **Imarketin** space. One task per prospect, not per feature.

Statuses: `To contact → Contacted → Demo booked → Demo done → Proposal → Won / Lost`

Custom fields: property name, contact, covers per day, tier (Small/Standard/
Branded), next step, next step date.

**This becomes the list I open first every morning**, and every walk-in creates
a task in it. If a conversation isn't in here, it didn't happen.

### 2. A separate Space — `Life`

Not a folder inside Imarketin. A **Space**, so business urgency can never
outrank it visually.

| List | What lives there |
|---|---|
| `🎯 North Star` | One pinned task: the goal, the number, the current MRR. Nothing else. |
| `🙏 Daily` | Recurring: Word & prayer, the one line, the one outcome |
| `📅 Rhythm` | Recurring: Monday film batch, Friday review, Sunday off |
| `💡 Ideas` | Mirrors `IDEAS.md` — where ideas go so they don't hijack the day |

Yes to a personal one. The whole reason for the split is that a life goal filed
under a business space quietly becomes a business task.

---

## Making the goal visible

Visibility is what makes a plan change behaviour. Three layers, weakest to
strongest:

1. **A ClickUp Dashboard** — one card, huge number: `MRR: €0 / €5,000`.
   Second card: `Conversations this week: 0 / 10`. Nothing else on it. A
   dashboard with twelve widgets is a dashboard nobody reads.
2. **The pinned North Star task**, updated every Friday during the review.
3. **On paper, on the wall, where I actually sit.** Genuinely the most effective
   of the three. Two lines, handwritten:
   > **10 conversations this week.**
   > **€0 → €5,000.**

## The automations

Three, and deliberately no more. Automations that fire constantly get muted, and
a muted automation is worse than none.

| # | Trigger | Action |
|---|---|---|
| 1 | Task enters `Contacted` | Set next-step date to +3 days, create a follow-up. *Most sales are in the second contact, and I have never once made a second contact.* |
| 2 | Task enters `Won` | Auto-create the onboarding checklist: config, staff training, first invoice, testimonial ask, referral ask |
| 3 | Friday | Create the weekly review task with the four questions from `DAILY.md` |

Plus one outside ClickUp: **a scheduled check-in with Claude** — every weekday
morning with the number and today's one outcome, and every Friday to run the
review. That's the automation that actually guides toward the goal, because it
can ask *"how many conversations yesterday?"* and notice when the answer has been
zero for four days.

**Nothing here is built yet** — ClickUp's API was erroring at the time of
writing, and the daily/weekly schedule needs my timezone and preferred times.
