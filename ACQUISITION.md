# Getting clients — the site, the videos, one Alba, the director

> Started 27 Sep 2026, owner's call: *"I've been spending so much time building,
> time to start getting clients."* The real client is the **operations director**:
> the one who sees what is happening. Our promise: **guest satisfaction through
> staff satisfaction and simple tools.**

First draft of the site (private link, open on phone and computer):
**https://claude.ai/artifact/6waFNTQirKcK8ecxGyVV5V**

---

## 1. The site

**Job of the site:** a director who hears about Alba (a visit, a video, a
colleague) understands in 60 seconds what it does, sees it working, and asks for
a demo *in his hotel*.

**Look:** Alba's own identity, so the site and the app feel like one product:
Fraunces for titles, Plus Jakarta Sans for text, amber on warm paper, the sunrise
glyph. The page follows a hotel's day from dawn (05:52, the breakfast opens in 38
minutes) to the director's evening point. Inspired by ElevenLabs and Claude in
the way it *shows* instead of tells: a live-looking call, real screens, founder
videos. Not a copy of either.

**Sections (in the draft):**
1. Hero: *Des équipes sereines. Des clients qui le sentent.* Two buttons: demo in
   your hotel, watch Alba in 90 seconds.
2. Proof strip: in service every morning since March in a Paris hotel · a BEO read
   in under a minute · nothing changes without « C'est ça ».
3. A hotel's day: 06:30 breakfast, 09:10 the BEO becomes tasks, 11:15 banquet,
   17:45 the director.
4. Talk to Alba: the call, allergies written per dish, the page that opens.
5. One Alba, three modules: Petit-déjeuner (en service), Événements (pilote),
   Direction (bientôt).
6. For the operations director: his desk view and a question to Alba.
7. Founder videos (three episodes, below).
8. Trust: you decide, allergies first, GDPR and AI Act, simple from day one.
9. How to get it: demo on site → 30-day pilot on one service → the whole team.
   Demo form.

**Build (after the draft is approved):**
- Its own site, not a page of imarketin.com (that stays the studio). New repo
  `alba-site`, Next.js 16, static, French first, then English and Spanish.
- Reuse from `imarketin-landing`: the i18n and metadata pattern, the
  video-with-still fallback, the motion gate (nothing hidden from crawlers or
  reduced-motion), JSON-LD, `llms.txt`, the screenshot script.
- The form must really arrive: Resend (EU region) to the founder's inbox, plus an
  SMS-style confirmation to the director. Name, hotel, rooms, contact. Nothing else.
- Screens on the site are always demo data. Never a real guest, never the partner
  hotel's name or colours (`imarketin-landing/docs/art-direction.md`: never fake a
  client screenshot either).
- Vercel Analytics, no cookies banner needed without trackers.

**Decided (D52):** no price on the site. One contract per company (a hotel or a
group), many users under one subscription; the model (per property, per group,
per user) is a calculation to do with finance. The site says *« un hôtel
parisien »*. Domain picked on 28 Sep.

## 2. The videos — the founder, like a model launch

Format: you to camera, calm, one idea per video, the product on a real phone in a
real back-of-house. Like the model introductions: short, confident, no music bed
fighting the voice. Subtitles always (people watch muted).

| # | Title | Length | What we see |
|---|---|---|---|
| 1 | Alba en 90 secondes | 90 s | You, then the breakfast list, a BEO photo, a call to Alba, the director view |
| 2 | Un BEO lu en 17 secondes | 2 min | Photo → events → tasks per team → allergies in red |
| 3 | Appeler Alba pendant le service | 2 min | A call: allergies, open the moment, « oui, c'est ça » |

**Episode 1 — script (French, ~220 words):**

> *(Face caméra, arrière-salle d'un hôtel, 6 h du matin.)*
> Je m'appelle Angel. Depuis mars, chaque matin, une équipe de petit-déjeuner
> travaille avec Alba.
> Dans un hôtel, l'information vit sur papier, dans des messages, et dans la tête
> de trois personnes. Quand elle arrive en retard, c'est l'équipe qui court, et
> le client qui attend.
> *(Écran : la liste du petit-déjeuner.)* Alba met la bonne information au bon
> endroit. La salle sait qui a le petit-déjeuner inclus.
> *(Écran : photo d'un BEO.)* Le commercial prend une photo du bon de commande.
> Alba le lit et prévient chaque équipe. Les allergies, en rouge, plat par plat.
> *(Écran : l'appel.)* Et pendant le service, on n'a pas les mains libres. Alors
> on lui parle. « Il y a des allergies pour midi ? » Elle répond, et elle l'écrit.
> *(Écran : la vue direction.)* Le directeur voit comment tourne l'hôtel, sans
> faire le tour.
> Alba ne décide rien à votre place. Elle propose, votre équipe confirme.
> Des équipes sereines, des clients qui le sentent. Je viens vous la montrer
> dans votre hôtel.

**Shooting kit:** your phone on a small tripod, a clip-on microphone, window
light. Demo data on every screen. Written OK from the hotel before filming
anywhere in it; no guest, no badge, no logo in frame.

## 3. One Alba — merging the breakfast app and the events app

What the two codebases look like today (research, 27 Sep):
- **Breakfast** (`Check-in-`, live from `main`): Next 16.1, data only on the
  tablet (encrypted, 30-day retention), no login, Mistral for reading lists,
  French and English, ~1,000 tests. Uses the partner hotel brand's gold, which
  must go. The newest branch (monthly value report, `/legal`, no staff data) is
  not merged yet.
- **Events** (`Alba`): Next 16.3, data only on the phone, roles, the AI brain and
  call, the compliance and UI audit loops, French only.

**Path (8 steps):**
1. Merge the value-report branch into breakfast `main` and deploy it. Then freeze
   breakfast features.
2. The `Alba` repo is the host (newer stack, roles, store build, audit loops).
3. Copy breakfast's pure logic and its tests into `src/modules/petit-dejeuner/`
   unchanged; green before any screen moves.
4. One shared model: one hotel, one team, one set of roles; staff phone numbers
   off by default (breakfast's stance wins).
5. One security layer: breakfast's signed device cookie and AI spend cap on every
   route, one encrypted store for both modules.
6. Breakfast screens move under `/petit-dejeuner/*` in Alba's design, keeping the
   tablet keypads and layouts that work.
7. Test the one app at the pilot hotel on its own address; the old breakfast app
   stays as the rollback until the team says it's better.
8. After the DPA: real login and a shared, EU-hosted database (schema already drafted).

**Rule:** no guest data (names, rooms, allergies of guests) ever goes to the US
services (Claude, the call). Breakfast data stays with Mistral (EU) as today.

## 4. The director view

**Who:** the hotel's operations director. Our real client: he buys, he sees, he
renews. **Where:** his computer at the desk, his phone in the corridor.

**Home, in one page:**
- *Now:* today's moments with their state (served, ready, late, waiting for a
  decision), breakfast covers live, the busiest quarter-hour.
- *What could go wrong:* late lines, open decisions, allergies of the day.
- *Tomorrow:* events, covers, what still waits.
- *The month:* the one-page report (attendance, peaks, covers per staff at peak;
  no money invented).
- *Ask Alba:* the same chat and call, with director questions: « Qu'est-ce qui peut
  coincer aujourd'hui ? », « Comment s'est passée la semaine ? ».

**The real blocker:** today every phone keeps its own data. A director on his
computer cannot see what the team's phones did without a shared, server-side
store. That needs the DPA (D9, Q1). So:
- **Now:** the director view as a working demo on demo data (for the site, the
  videos and the October meeting), plus the director's own phone at the pilot.
- **After the DPA:** the shared EU database; the director view shows the real hotel.

## 5. Order

| When | What |
|---|---|
| This week | Approve the site draft · pick the domain · film episode 1 · director view prototype (screens on a canvas) |
| Before 13 Oct | Build and ship the site with the real form · episodes 2–3 · merge step 1 |
| October meeting | Show the director view and the monthly report · ask for the DPA and the price |
| After the DPA | Shared database, real director view, merge steps 3–8 |

## 6. App Store and Play Store (D52: required)

Same code, wrapped with Capacitor (`SHIPPING.md`): reliable notifications and a
listing a director can find. **Start the paperwork now**, the code is ready for it:
- Apple Developer, **company** account (99 €/year). Needs a D-U-N-S number for the
  company: free, but days to two weeks. Google Play: 25 € once.
- Apple review needs: a privacy policy URL (the site's Légal page), in-app account
  deletion, a demo login for the reviewer, and the AI disclosure already in the app.
- Store listings go live after the site; push notifications need the shared
  server, so after the DPA.

## Decisions still open

1. Domain (28 Sep).
2. Film where? (The partner hotel with written OK, or a neutral room.)
3. Staff phone numbers in the merged app: off by default (recommended).
4. The price model, with finance (Q6).
