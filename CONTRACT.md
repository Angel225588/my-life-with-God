# Ready to sign

What we need on the day the hotel says « d'accord, on y va pour 2 ans », and the
smaller agreement that comes first, before the pilot uses the shared server (D54).
Drafts are a starting point. **A French lawyer reviews the contract and the DPA
before anything is signed** (a few hundred euros, PRODUCTION.md §2).

## Who writes what

- **We write it.** The hotel is the *responsable de traitement* (controller);
  Alba is the *sous-traitant* (processor). The processor normally brings its
  contract and its DPA. The hotel can ask for changes.
- **A large group may send its own vendor papers instead:** its own data
  agreement and a security questionnaire. Then we answer theirs, using ours as
  the reference. Ask early (Q1).
- **Who signs** is the legal entity that runs the hotel (the *société
  d'exploitation*), which is not always the brand. Q1 finds out which.

## The pack (what the hotel signs or receives)

| # | Document | What's in it | Status |
|---|---|---|---|
| 0 | **Convention de pilote + accord de sous-traitance pilote** (2–3 pages) | Signed **before** the shared server holds real data (D54). What the pilot is, the data (team first names, the day's work, no guest names), the art. 28(3) clauses, the end date, deletion at pilot end + 30 days, free of charge | **To draft first.** It needs Q1 answered |
| 1 | **Contrat de service** (conditions générales + bon de commande) | Scope, price (Q6), term, what happens at the end, support and availability, liability cap, who owns what | To draft |
| 2 | **DPA — accord de sous-traitance** (RGPD art. 28) | The nature and purpose, the types of data and whose data it is (art. 28(3)). On their instructions only, and we warn them if an instruction breaks the RGPD (28(3)(h)). Confidentiality, security. Sub-processors under a general authorisation: we give notice of changes and they may object (28(2)); each one bound by the same duties (28(4)). Help with people's rights. **We notify a breach within 48 h**; the hotel then has 72 h to tell the CNIL. Return or deletion at the end, audits | To draft |
| 3 | **Annexe sécurité** | Hosting in Paris, encryption, access by role, isolation between hotels (RLS + test), backups (7 days), logs, who can access production (only the founder, with MFA on Supabase, Vercel and GitHub), no AI tool connected to production data | To draft, from `alba/docs/shared-server.md` |
| 4 | **Sous-traitants** (the list the DPA points to) | Supabase (data, Paris), Vercel (app, Paris), Mistral (reading BEOs, EU), Anthropic (Alba's answers), ElevenLabs (voice), the push services (Apple, Google, Mozilla), Google and Apple sign-in, and an email provider if we use email codes. **Paris hosting is not "no transfer":** Supabase, Vercel, Anthropic and ElevenLabs are US companies. For each one: where, what, and the safeguard (SCCs, or a Data Privacy Framework certification we have checked) | To draft. Each needs **our own DPA with them, on a company account**. Today Anthropic and ElevenLabs run on personal accounts (D49) |
| 5 | **Registre des traitements** (our side, art. 30.2) | Our record as a processor, dated and kept up to date | To draft |
| 6 | **Note pour le CSE et les équipes** | Alba records who ticked what and when, shows who is late (« +5 min »), and gives the director a live view. In a hotel with 50 or more employees, the CSE must be **informed and consulted before the decision** to use such a tool (Code du travail L2312-38). Staff must be told (L1222-4, RGPD art. 13), and the tool must stay proportionate (L1121-1). It applies **to the pilot too**. The note says the data is never used for discipline. This is the hotel's duty; we hand them a ready one-page note | To draft. **Can delay the start**, so raise it at the first meeting |
| 7 | **AIPD (DPIA) short form** | AI + staff data may need one. This is the hotel's duty; we help (art. 28.3.f) | Template, if they ask |
| 8 | **Attestation d'assurance** | RC Pro + cyber | To get |
| 9 | **Attestation de vigilance URSSAF** | The client must ask for it on any contract of €5,000 HT or more over its whole life, setup included (L8222-1, R8222-1), at signature and then every 6 months. 2 years at the Events price is over €5,000. With the founding discount it gets close, so provide it anyway | Download from URSSAF |
| 10 | **Proof the company exists** | The SIRENE notice plus the extract from the RNE (INPI). A micro-entreprise has a Kbis only if its activity is commercial (RCS). A software subscription may count as commercial: **confirm the activity type with the lawyer.** If purchasing insists on a Kbis and we don't have one, that is D20's signal to move to a SASU | Download |

## Before signing, on our side

- **Company accounts, each with its DPA accepted,** for all five sub-processors
  (item 4). This ends the pilot exceptions D45, D49 and D54.
- **Accounts and the shared server live**, with deletion and export per person
  and per hotel.
- **Price model decided** (Q6). The contract says one company, many users, one
  subscription (D52).
- **VAT:** prices in the contract are HT, with *TVA en sus si le seuil de
  franchise est dépassé*, because the franchise may end within 2 years. While
  under it, invoices say *TVA non applicable, art. 293 B du CGI*.
- **Invoices:**
  - « EI » after the name, and the SIREN.
  - Late-payment penalties and the €40 flat collection fee (Code de commerce
    L441-10).
  - E-invoicing: receiving is required from 1 Sep 2026; issuing, for a micro,
    from 1 Sep 2027, which falls inside a 2-year term.

## For a 2-year term (to discuss with the lawyer)

- A **pilot period** at the start (e.g. 3 months) that either side can end.
  This makes a 2-year yes easy to give.
- The **price fixed** for the term, or indexed once a year (Syntec).
- **At the end:** the data is returned (export) and then deleted, with a date
  written in the contract.
- **If Alba stops:** they keep an export of their data.

## Are we ready today?

**Not yet.**
- **The product:** close. The shared server and accounts come first (D54).
  They are built on demo data and go live with the hotel's data once the pilot
  agreement (row 0) is signed and the CSE is informed.
- **The papers:** about two weeks. Drafts take a few days, then the lawyer
  review, the company accounts, and the insurance.
- **What to say if they say yes tomorrow:** « Parfait. Je vous envoie le contrat
  et l'accord de sous-traitance cette semaine, et on présente Alba à votre CSE. »
