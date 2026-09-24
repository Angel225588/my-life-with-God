# Alba — production readiness

Five tasks, in order. Each has a prompt to paste into Claude Code in the Alba
repo. **Do them one at a time**, in this order, and let each one finish before
starting the next.

Order matters: 1–3 are what let me sell to anyone at all without lying about
compliance. 4 is what answers the price objection. 5 is what the director
actually asked for.

| # | Task | Why now | Rough size |
|---|---|---|---|
| 1 | Gemini → Mistral | EU hosting kills most of the GDPR problem at the root | Half a day |
| 2 | GDPR: RLS, DPA, retention, minimisation | Hotels' compliance teams *will* ask | One day |
| 3 | Lock down `/api/*` | Open endpoint billing an AI provider per call | Half a day |
| 4 | The monthly value report + email | The answer to "too expensive for its value" | One to two days |
| 5 | Customisation | Exactly what the director said he values | Half a day |

> A note on the order: 4 is the one that makes money. If it were safe to do it
> first, it would be first. It isn't — an open API and unisolated guest data are
> real liabilities and they get closed before I put the product in front of more
> hotels. Three days. Then sell.

---

## 1 — Gemini → Mistral

Good call, and better than you may realise. Mistral is French, EU-hosted, and
GDPR-native — which means the Article 28 conversation with a hotel goes from
*"my AI provider is American, here's the transfer mechanism…"* to *"it's
processed in the EU by a French company."* That's not a small difference when
you're selling to French hotels. It removes the objection rather than answering
it.

Also: they have a **document/OCR model** built for exactly what Alba does —
parsing uploaded lists. Check whether it fits better than a general chat model
for the extraction step.

```
Migrate this codebase from Google Gemini to Mistral AI.

Context: this app lets hotels upload breakfast-attendance lists (PDF and
images) and extracts structured data from them. We're moving to Mistral
because it is EU-hosted and simplifies our GDPR position for French hotel
customers.

Before writing code, check the current Mistral API docs for model names,
the SDK package name, and the request/response shapes — do not rely on
memory, and tell me what you found before you start.

Do this:
1. Find every place Gemini is called. List them for me before changing
   anything, with what each call does and which model it uses.
2. Evaluate Mistral's document/OCR model against their general chat model
   for our extraction step specifically. Recommend one and say why.
3. Put all AI calls behind a single provider module with one interface, so
   we never have to do this migration in scattered places again.
4. Migrate. Keep the exact same output shape so nothing downstream breaks.
5. Add: timeouts, retry with backoff on 429 and 5xx, and a hard per-request
   token cap.
6. Move keys to env vars. Confirm no key is in client code or in git
   history — check the history, not just the working tree.
7. Write a test for the extraction step using a realistic sample list, and
   verify parity with the Gemini output before we cut over.
8. Remove the Gemini SDK and its env vars once tests pass.

Do not change the UI or the database schema.
```

**After it ships:** sign Mistral's DPA, and delete the Google Cloud project so
there's no dormant billing or dormant data.

---

## 2 — GDPR

The thing to understand about my position: **the hotel is the data controller
(responsable de traitement). Alba is the processor (sous-traitant).** Under
Article 28 the hotel is legally required to have a written DPA with me. So this
isn't optional paperwork — it's a document a hotel will eventually *ask me for*,
and the ones with real compliance functions will ask before signing.

Having it ready makes me look like a serious vendor. Not having it stalls deals
at the worst possible moment.

```
Make this app GDPR-compliant as a data processor. We handle hotel guest
data (names, room numbers, meal entitlements) for hotels in France.

Audit first, then fix. Show me the audit before changing anything.

1. DATA MINIMISATION — the highest-value item, do this analysis first.
   List every piece of personal data we store and what it's actually used
   for. For each one ask: could this feature work with a room number and an
   entitlement status instead of a guest name? Anything we can stop
   collecting is a whole class of risk deleted rather than managed. Give me
   a specific recommendation per field.

2. MULTI-TENANT ISOLATION — verify Row Level Security is enabled on every
   Supabase table holding hotel or guest data, with policies scoped by
   hotel/tenant id. Write a test that proves hotel A cannot read hotel B's
   rows, including through any API route, view, or RPC. This is the one
   that ends the company if it's wrong.

3. RETENTION — guest data should not live forever. Add a configurable
   retention window (default 90 days) and an automatic purge job. Log the
   purges.

4. ERASURE AND EXPORT — an endpoint for a hotel to export or delete all
   data for one guest, and all data for their whole property.

5. ACCESS LOGGING — who accessed what guest data, when. Retained
   separately from the data itself.

6. ENCRYPTION — confirm at rest and in transit everywhere, including
   uploaded files in storage.

Then produce, as markdown files in a /legal folder:
   - A DPA (accord de sous-traitance) I can offer hotels, Article 28
     compliant, listing sub-processors: Mistral, Supabase, Vercel.
   - A register of processing activities (registre des traitements).
   - A privacy policy.
   - A one-page "security and compliance" summary I can hand to a hotel's
     compliance contact. Plain language, not legalese.

Flag anything you are not confident about rather than guessing. I will
have a lawyer review the legal documents — draft them as a starting point
and mark clearly where professional review is needed.
```

**These drafts are a starting point, not legal advice.** Have a French lawyer
review the DPA and privacy policy before a hotel signs. It's a few hundred euros
and it's the cheapest insurance in the business.

---

## 3 — Lock down `/api/*`

Already in the ClickUp backlog as *"#1 pre-sale"*, and it was right.

```
Security-harden all API routes.

Audit every route under /api and tell me, for each: is it authenticated,
is it rate limited, does it validate input, can it cost us money if
someone hits it in a loop? Show me the table before fixing anything.

Then:
1. Require authentication on every route that isn't deliberately public.
   List anything you're leaving public and justify each one.
2. Rate limit per authenticated user and per IP. Stricter limits on any
   route that calls the AI provider.
3. Hard body-size limit on uploads.
4. Validate uploaded files by magic bytes, not by extension or by the
   client-supplied content type. Accept only PDF and the image types we
   actually need.
5. A hard monthly spend cap on AI calls, per hotel and globally, that
   fails closed with a clear error rather than silently spending.
6. Make sure errors don't leak stack traces, table names, or provider
   details to the client.
7. Check git history for committed secrets. If any are found, tell me
   which and I'll rotate them.

Write a test for each fix.
```

---

## 4 — The monthly report

Reframed 24 Sep 2026 around guest experience and operations rather than cost
savings — see [`REPORT.md`](REPORT.md) for why, and what each number rests on.

```
Rebuild the monthly report around guest experience and operations —
NOT cost savings.

WHY THIS CHANGED
The first version led with money saved. That was wrong: it invites a
price comparison, it gets squeezed at budget time, and half of it isn't
observable by us anyway (payments live in Micros). This report is for a
hotel operations director. Its job is to help him STAFF AND PREPARE
CORRECTLY, and to show him where guest experience is at risk. Money is
his conclusion to draw, not our claim to make.

STEP 1 — AUDIT BEFORE YOU BUILD. Show me this before writing any code.
For each metric below, tell me: can we derive it from what we store
today, yes or no? If no, exactly what would we need to start storing?
I need this list before anything else — it decides what ships this week.

STEP 2 — THE METRICS, grouped by where they come from

A. Observed automatically (we should already have these)
   - Attended: guests on the entitled list who showed up
   - No-shows: entitled, did not show. Count AND rate
   - Ecarts: showed up with no valid entitlement, flagged to reception.
     Report as an OPERATIONAL accuracy number, not a revenue number
   - Actual service start (first arrival) and end (last arrival),
     against the planned window
   - Peak: busiest 15-minute block, how many, and at what time
   - Services covered, and any fallback to paper

B. Entered by the manager — one number, one tap, per service
   - How many staff were on duty
   Build the tap. Without it, half of section C is impossible, so make
   it the fastest interaction in the app.

C. Derived — this is where the value is
   - COVERS PER STAFF MEMBER AT PEAK. The headline. This is the number
     that shows whether the team was set up to succeed or to survive
   - Pressure blocks: how many 15-minute blocks exceeded a configurable
     comfortable load per staff member. Use this INSTEAD of a staff
     satisfaction survey — it is objective and needs no one to fill
     anything in
   - Planned vs actual service window: did we open late, run long
   - Arrival curve: arrivals per 15 minutes across the service
   - DAY-OF-WEEK AVERAGES, tracked across months

STEP 3 — THE ARRIVAL CURVE
A simple curve of arrivals in 15-minute buckets across the service,
with the staff-on-duty level drawn across it. Where the curve goes
above the staffing line is where guests waited. That single graphic is
the whole argument — it shows the SHAPE of the rush, not just a total.
Keep it plain: no 3D, no gradients, no decoration. It must be readable
in five seconds on a printed page.

STEP 4 — THE DAY-OF-WEEK PATTERN. The most valuable section.
Average attendance and peak load by day of week, across every month we
have. The point is predictability: if Wednesdays consistently run
heavier than Mondays, that is a rostering decision the director can
make for the rest of the year, and we are the only system in the
building that can tell him.
Show the trend across months so he can see it is a stable pattern and
not one odd week. State the number of weeks the pattern is based on,
so he can judge how much to trust it.

STEP 5 — BUILD BOTH
   - A screen in the app: filter by month and by service
   - A one-page PDF for the monthly email, same numbers, print-ready

RULES
- Never print a number we did not observe. No estimates, no industry
  averages, no extrapolation. If it matters and we cannot see it, tell
  me what to store and leave it off the page
- No euro figures in the headline. Money appears only if the director
  asks, and then only as something he can verify in Micros
- Print every assumption (comfortable load per staff member, the peak
  window length) at the bottom of the page, and make them editable per
  hotel. Numbers he can adjust are numbers he trusts
- One page. If it does not fit, cut. Two pages is a report nobody reads

Start with Step 1 and wait for me before building.
```

### How to send it — email integration

**Use Resend.** Best fit for the stack: clean API, good deliverability, works
with Vercel in about twenty minutes, generous free tier, EU region available
(pick it). Alternatives are Postmark (excellent, pricier) and SendGrid (fine,
clunkier). Don't build SMTP yourself.

```
Set up transactional email with Resend.

1. Add the Resend SDK. Use their EU region for GDPR consistency.
2. Verify our sending domain properly: SPF, DKIM and DMARC records. Tell
   me exactly which DNS records to add. Getting this wrong means our
   reports land in spam, which defeats the entire point.
3. Build a React Email template for the monthly report. Short body — the
   headline number and one sentence — with the PDF attached. Nobody reads
   a long email; they open the PDF or they read the one number.
4. A Vercel Cron job on the 1st of each month that generates and sends
   the report to every active hotel.
5. Log every send. Retry failures. Alert me if a send fails twice.
6. A test mode that sends to me instead of to the hotel.

Send me a real one first with our live hotel's actual data, so I can
check it before any customer sees it.
```

---

## 5 — Customisation

The director said he liked that the competing app let him *"customize many
things."* That's a direct instruction about what he values, and it's cheap.

```
Add per-hotel customisation. Keep this simple — settings, not a theme
engine.

  - Hotel logo on the dashboard and on the monthly report
  - One accent colour
  - Custom meal-period names and times (breakfast, brunch, late
    breakfast — hotels name these differently)
  - Choose which columns show on the main dashboard, and their order
  - Export format preference: PDF, CSV, Excel
  - Language: French and Spanish, with French as default

All of it editable by the hotel themselves, in a settings page. No
support ticket, no email to me. The feeling of control is the point —
if they have to ask me to change their logo, they don't feel ownership.
```

---

## The order again, because it will be tempting to skip ahead

1. Mistral → 2. GDPR → 3. API lockdown → 4. Report → 5. Customisation

Three days of foundation, then the thing that makes money, then the thing that
delights. Skipping 1–3 to get to 4 faster means selling to hotels while holding
their guests' data in a way I couldn't defend if asked. Not worth it — and the
compliance one-pager from task 2 is itself a sales asset with hotels that take
data seriously.
