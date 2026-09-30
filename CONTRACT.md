# Ready to sign

What we need on the day the hotel says « d'accord, on y va pour 2 ans ».
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
| 1 | **Contrat de service** (conditions générales + bon de commande) | Scope, price (Q6), term, what happens at the end, support and availability, liability cap, who owns what | To draft |
| 2 | **DPA — accord de sous-traitance** (RGPD art. 28) | What we process and why, for how long, on their instructions only, confidentiality, security, sub-processors, help with people's rights, breach notice without delay (they have 72 h to tell the CNIL), return or deletion at the end, audits | To draft |
| 3 | **Annexe sécurité** | Hosting in Paris, encryption, access by role, isolation between hotels (RLS + test), backups, logs, who can access production (only the founder) | To draft, from `alba/docs/shared-server.md` |
| 4 | **Sous-traitants** (the list the DPA points to) | Supabase (data, Paris), Vercel (app, Paris), Mistral (reading BEOs, EU), Anthropic (Alba's answers), ElevenLabs (voice). For each one: where, what, and the transfer safeguard | To draft. Each needs **our own DPA with them, on a company account**. Today Anthropic and ElevenLabs run on personal accounts (D49) |
| 5 | **Registre des traitements** (our side, art. 30.2) | Our record as a processor | To draft |
| 6 | **Note pour le CSE et les équipes** | Alba records who did what and when. In France, a tool that can follow staff activity must be presented to the CSE before it is used (Code du travail L2312-38), and staff must be told (L1222-4). This is the hotel's duty; we hand them a ready one-page note | To draft. **Can delay the start**, so raise it at the first meeting |
| 7 | **AIPD (DPIA) short form** | AI + staff data may need one. This is the hotel's duty; we help (art. 28.3.f) | Template, if they ask |
| 8 | **Attestation d'assurance** | RC Pro + cyber | To get |
| 9 | **Attestation de vigilance URSSAF** | Required by the client for any contract of €5,000 or more, renewed every 6 months. 2 years at the Events price is over €5,000 | Download from URSSAF |
| 10 | **Avis de situation SIRENE** | Proof the company exists. A micro-entreprise has no Kbis; if purchasing insists on a Kbis, that is D20's signal to move to a SASU | Download |

## Before signing, on our side

- **Company accounts, each with its DPA accepted,** for all five sub-processors
  (item 4). This ends the pilot exceptions D45, D49 and D54.
- **Accounts and the shared server live**, with deletion and export per person
  and per hotel.
- **Price model decided** (Q6). The contract says one company, many users, one
  subscription (D52).
- **VAT:** under the micro threshold, invoices say *TVA non applicable, art.
  293 B du CGI*.

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
- **The papers:** about two weeks. Drafts take a few days, then the lawyer
  review, the company accounts, and the insurance.
- **What to say if they say yes tomorrow:** « Parfait. Je vous envoie le contrat
  et l'accord de sous-traitance cette semaine, et on présente Alba à votre CSE. »
