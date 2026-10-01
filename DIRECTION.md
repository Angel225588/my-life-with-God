# The payer's screen and the legal deck — the plan for the October meeting

Owner, 1 Oct: "it's time to work on the presentation for laws, regulations, and the
screen for the person who pays, meaning that we will merge Alba with check-in in
order for them to see it all and talk to Alba."

Two things to show the director at the October meeting (~13 Oct, Q4):
- **A.** The deck that answers the legal questions before they are asked.
- **B.** The screen he pays for: the whole hotel's day at a glance, breakfast and
  events, and Alba to ask.

---

## A. « Alba et vos données » — the legal deck

**Who reads it:** the director first. He forwards it to whoever signs (Q1), to
the hotel's legal person or DPO, and to the CSE. So it must stand on its own,
in French, with no jargon left unexplained.

**Format:** a deck made in Alba's identity, which downloads as PDF or PowerPoint.
The CSE gets a one-page note of its own (CONTRACT.md row 6).

**The ten slides:**
1. **Alba en une phrase.** Who uses it (breakfast tablet, team phones, the
   director) and what it changes.
2. **Qui est responsable.** The hotel is the *responsable de traitement*; Alba is
   the *sous-traitant*. What gets signed, in order: the pilot agreement, then
   the contract and the DPA.
3. **Quelles données, module par module.**
   - **Petit-déjeuner:** room, name, package, the time of arrival. That is what
     the team needs to serve.
   - **Événements:** team first names and the day's work. No guest names.
     Allergies are counts per dish.
4. **Où elles vivent** (one drawing):
   - the tablet and the phones, encrypted
   - the server in Paris (from the pilot agreement on)
   - each sub-processor, with its country and its safeguard
5. **L'IA: ce qu'elle fait, ce qu'elle ne fait pas.**
   - Guest data never goes to the US AI services; breakfast stays with Mistral
     (EU). Alba's assistant sees only counts.
   - It says it is an AI (AI Act art. 50), and the team gets a short AI
     training (art. 4).
   - **It never scores, ranks or judges a person.** Doing that would make it a
     "high-risk" AI under the AI Act (staff evaluation, Annex III).
6. **Les équipes.**
   - What the director sees: moments, delays, decisions.
   - What he never sees: who opened what, rankings, « vu par la direction ».
   - The data is never used for discipline.
   - The CSE is informed and consulted.
7. **La sécurité.**
   - Hosted in Paris, encrypted, each hotel walled off from the others (with a
     test proving it).
   - Founder-only access, with two-factor sign-in.
   - Breach notice within 48 h.
8. **Les droits et les durées.**
   - Export and erase, for one guest or for the whole hotel.
   - Breakfast data kept 90 days by default (30 recommended); events kept N
     days after the event ends.
9. **Ce qui reste à faire, dit honnêtement.** Company accounts and their DPAs,
   the lawyer's review. Saying it first builds trust.
10. **Prochaines étapes.** Sign the pilot agreement, present the CSE note, and
    the contact person.

**Sources:**
- `CONTRACT.md`
- `alba/docs/shared-server.md`
- the breakfast app's drafts: `legal/DPA.md`, `REGISTRE-DES-TRAITEMENTS.md`,
  `SECURITY-SUMMARY.md`

**Checked by:** the compliance auditor, then the owner, then the lawyer, before
it leaves our hands.

**One code change it implies:** Alba's brain refuses to rank or judge a person
(« Qui est le plus lent ? » gets « Je ne compare pas les personnes. Je peux vous
montrer ce qui était en retard aujourd'hui. »). This becomes a rule in the
system prompt, with a test.

---

## B. The payer's screen: « Direction »

**The one question:** « Mon hôtel va bien aujourd'hui, et qu'est-ce qui a besoin
de moi ? »
**Where:** the computer at the desk, the phone in the corridor.

**On top, the hotel in one line** (the owner's three numbers):

> **Aujourd'hui · 87 % d'occupation · 180 petits-déjeuners attendus (64 servis) · 2 événements · 72 couverts**

- **Occupation:** read from the hotel's own morning brief, which the breakfast
  app already reads (occupied rooms ÷ rooms sellable). Shown with its source,
  « d'après le brief du matin ». It is a percentage, but it is the hotel's own
  number, not one we invent. The panel's « pas de pourcentages » was about
  scores we make up.
- **Petit-déjeuner:** expected (from the list), served (live during service),
  the peak quarter-hour. After service: « Terminé · 176 sur 180 ».
- **Événements:** today's events and their covers.

**Under it, the panel's design** (`alba/docs/ux/direction-panel.md`, already
decided), in this order: À décider, Allergies du jour, En retard, Petit-déjeuner,
Demain, then Les moments du jour, and the Semaine tab.

**Alba, always there:** the same chat and call. It answers director questions
across both modules: « Comment s'est passé le petit-déjeuner cette semaine ? »,
« Qu'est-ce qui peut coincer demain ? ». Every answer ends with « D'après : … ».

**The trick that makes the merge safe:** the director needs **numbers**, not
guests. So the breakfast tablet sends only counts to the shared server: per day
expected, served, no-shows, covers per quarter-hour, and occupancy. **No guest
names or rooms leave the tablet.** This keeps the legal risk at the level of the
events pilot, it fits the pilot agreement, and it can go live with it.

### Build order

1. **Today, owner:** make the breakfast repo and this repo private.
2. **Me:**
   - scrub the brand from the breakfast code (wordmark, gold colours, names in
     code and docs)
   - merge the value-report branch
3. **Me:** copy the breakfast numbers logic and its tests into Alba
   (`src/modules/petit-dejeuner/`: snapshot, quarter-hours, peak, morning brief,
   value report). They must pass before any screen moves.
4. **Me:** `/direction` on demo data, desktop and phone, with the line on top
   and the five blocks. Reviewed by the « directeur » agent, the UI audit and
   the compliance audit.
5. **October meeting:** show `/direction` and the deck. Ask for the pilot
   agreement and Q1.
6. **After the pilot agreement:**
   - the shared server live
   - the breakfast tablet sends its counts
   - `/direction` shows the real hotel
7. **Later:** the breakfast screens move into Alba under `/petit-dejeuner`. One
   app, one sign-in. The old app stays as the fallback until the team prefers
   the new one.

---

## Before any of this: repositories private

The breakfast repo and this plan repo are public. Both go private before the
merge starts: the owner does it in GitHub, under Settings → Danger Zone →
Change visibility. Step 2 of the build order then cleans the breakfast code.
