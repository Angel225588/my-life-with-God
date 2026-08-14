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

## 4 — The monthly value report

The one that makes money. See [`MARRIOTT.md`](MARRIOTT.md) for why this is the
product's missing half rather than a feature.

```
Build an automated monthly value report for each hotel.

WHY: our software saves hotels money invisibly, and invisible value gets
priced at zero. A director just told us we're "too expensive for the
value" while using the product daily. This report is the answer, and it
needs to work for every future customer too.

THE REPORT — one page, per hotel, per month:
  - Covers processed this month, and total since they started
  - Staff hours saved: covers x a configurable seconds-per-cover
    (default 20s), shown as hours and as euros at a configurable
    hourly rate
  - THE KEY NUMBER: covers served that were NOT on the entitled list —
    count, and value at a configurable average breakfast price.
    This is money they were losing before us. Make it the biggest thing
    on the page.
  - Busiest service, and peak throughput in the busiest 15 minutes
  - One line at the bottom: total estimated value delivered this month
    vs what they pay us

First, before building anything: query the existing production data and
tell me what these numbers actually are for our live hotel, for every
month we have. I want to see real figures before we design around them.
If the entitled-vs-attended delta isn't currently derivable from what we
store, say so immediately — that changes what we build first and it is
the most important thing in this task.

THEN:
  - Generate it as a PDF, and as a page in the app
  - Email it automatically on the 1st of each month
  - Let me trigger one manually for any hotel and any month
  - Make every assumption (seconds per cover, hourly rate, breakfast
    price) editable per hotel, and show them on the report so the numbers
    are transparent and arguable rather than magic

Do not invent or estimate any number we cannot derive from real data.
If something isn't measurable yet, leave it out and tell me what we'd
need to store to measure it.
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
