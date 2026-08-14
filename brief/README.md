# The morning brief

A one-page PDF that lands before the day starts, so the first thing I see is the
number — not my inbox, and not a build task.

`template.html` is the layout. It is **already filled in with 14 Aug 2026 as a
worked example** — the daily job copies it and replaces the marked values. Keep
the design stable; only the values change. A brief that looks different every
morning stops being glanceable.

## What gets replaced each day

| In the template | Replaced with |
|---|---|
| `Friday 14 August 2026` / `Day 1 of the plan` | Today, and days since 14 Aug 2026 |
| `—` in the big number | Conversations this week, from `SCOREBOARD.md`. Leave as `—` if the row is blank — never invent it |
| `€0 / €5,000` and the bar width | Current MRR |
| The two ruled lines | Left blank. **He writes today's one outcome by hand.** That is the point of them |
| Follow-ups section | Items due from ClickUp, or the standing line about second contacts |
| The current priority box | Top unfinished item from the 90-day arc in `LIFE-PLAN.md` |
| Verse + reference | A new one daily — wisdom, work, patience, trust |

Never changes: the Alba wordmark, the three lines in the footer.

## Rules

- **One A4 page.** If it doesn't fit, cut something. Two pages is a report, and
  nobody reads a report at 4am.
- **Never name the flagship hotel account.** No written permission — see
  [`../MARRIOTT.md`](../MARRIOTT.md). Write "the flagship account".
- **Never fill in today's one outcome.** He writes it. A brief that decides the
  day for him is a brief he stops reading.
- **No charts, no streaks, no motivational quotes.** One number, one priority,
  one verse.

## Rendering

```sh
CHROME=$(ls -d /opt/pw-browsers/chromium-*/chrome-linux/chrome | head -1)
"$CHROME" --headless --disable-gpu --no-sandbox --no-pdf-header-footer \
  --print-to-pdf=brief.pdf "file://$PWD/brief/template.html"
```

Verify it's one page and over 1KB before sending.

## Delivery

Routine `trig_01SyPConA54ui9Sy4T5Rc9MV` — fires 01:52 UTC daily (03:52 Paris in
summer, 02:52 in winter; adjust at the DST change if it drifts too early).

**Known gap:** Routines created through the MCP tool can't carry connector
grants, so the fired session has no Gmail and can't send the email. Until that's
fixed it falls back to committing the PDF to `briefs/` on the branch.

**The fix:** recreate the Routine from the Routines UI on claude.ai with Gmail
attached, then delete `trig_01SyPConA54ui9Sy4T5Rc9MV`. Same prompt, working
email.
