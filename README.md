# TijaPass V7 — Bilingual Intelligent Accounting & Super Accountant Review

**Browser demonstration (not a live payment or regulated financial system).** This upgrade preserves the V4–V6 customer, TFL, FI, supplier, wallets and financing user journeys while improving the accounting experience. Source code remains fully editable React/Vite. Designed for GitHub → Netlify as a separate V7 site.

## Major V7 features

- Language dropdown English / Kiswahili. Core V7 customer/accountant flows have explicit English–Kiswahili strings; a broad presentation translation catalogue handles many inherited V4–V6 dashboards, financial statement labels, partner screens and forms. Any untranslated custom/system-generated text should be corrected during professional i18n QA before public customer rollout.
- Only **two capture methods**: simple guided transaction entry, or bank/mobile-money CSV/TSV/basic XLSX statement import. The standalone *describe in your own words* entry screen was removed.
- Speech-to-text button under mandatory **What was this money for?** field (subject to device/browser support). Customer must read back/correct the recognised text before submitting.
- Category appears immediately after counterparty and before purpose, filtered according to money received/paid; rule engine suggests the likely accounting category. Low-confidence descriptions show a compact category list with a **More types** option.
- Rules-assisted congruence checks consider direction, purpose, counterparty, amount, and category; incomplete, contradictory or semantically inconsistent records cannot be posted automatically. The rules engine cannot infer an arbitrary customer's identity or prove the true source of funds.
- Clear and ordinary guided entries can automatically enter the browser's **provisional/self-declared** accounting journal; **they are not marked as externally verified** and are withheld from the reviewed-data credit metric. Imports, possible duplicates, unusual amounts and financing-related categories require review.
- **Super AI Accountant demo (explainable deterministic rules, not an external LLM):** customer corrections, exception routing, TFL alert and reviewer notes. In this browser demo the TFL role can return an entry for customer correction, review, reject or approve. Posted journals may be reversed by a separate, balanced contra entry, retaining the source original and justification.
- Legacy V4 wallet/finance/scoring use fictional seed data. The V7 accounting journal and bank/mobile imports are **not yet reconciled to real FI APIs or a permanent multi-user ledger**. TFL users can switch roles in the demo; production requires genuine identity and authorization.

## Demo to show a developer / BOT

1. Open new V7 Netlify site (do not overwrite V6), switch to **Customer** and select **Kiswahili**. Inspect Overview and customer finance dashboard.
2. Select **Uhasibu Rahisi → Jaza maelezo**, choose **Pesa niliyopokea**, enter `Juma`, `20,000`, choose cash, and enter `Nimeuza mchele kwa Juma`. Review and confirm to see an **auto-cleared self-declared** posting. Do not call it verified bank income.
3. Try changing the customer name to Asha while leaving the purpose `kwa Juma`; the system should block a contradictory entry until corrected.
4. Import a demonstration CSV statement and review an entry with missing payer/purpose; it must be clarified and marked for TFL review (not posted as authenticated banking data).
5. Attempt a `TZS 20,000,000` receipt against much smaller transaction history. The review queue flags unusual activity, asks for evidence and requests TFL review.
6. Switch to **TFL Admin → Smart Accounting → Review & verify**. Show exception notes, return-to-customer action and correction resubmission.
7. Show **Financial statements** and **Audit & controls → posted journals**. Demonstrate a reasoned reversing journal entry, confirming debit-credit balances stay equal.

## Deployment (GitHub + Netlify)

1. Extract ZIP and upload the **files INSIDE** `TijaPass_V7_AI_Accounting/` at the ROOT of a new GitHub repository, e.g. `Tijapass-V7`.
2. Netlify → Add new project → Import from GitHub → select this repository.
3. Build command `npm run build` and publish directory `dist` (already declared in `netlify.toml`).
4. Wait until Netlify displays **Published**. Open the V7 URL and test both languages.
5. Use demonstration data only. The local browser data key is `tijapass-v7-ai-accounting`; it does not migrate V6 data and is not shared between devices.

### Local developer workflow

Node 20+: `npm install`, `npm run dev`, `npm test`, `npm run build`.

**Do not enter actual NIDA information, genuine customer bank statements, passwords, payment API secrets or real funds into this demo.**

## Developer handoff

- `src/AccountingStudio.jsx` — customer and TFL accounting UI, TFL queue, returns/reversal action.
- `src/superAccountant.js` — tested, explainable classification/congruence/duplicate/anomaly routing and reversal primitives; NOT an LLM.
- `src/v7FullLocale.js`, `src/v6i18n.js` — presentation translations + dedicated accounting strings.
- `src/accounting.js` — deterministic double-entry allocation/reporting. Existing tests in `tests/`.
- `src/importers.js` — statement CSV / TSV / basic XLSX parsing; no secure provider verification or PDF/OCR.
- `supabase/accounting_v7_upgrade.sql` — conceptual database migration for a future secure implementation; **NOT executed by Netlify**.
- `PRODUCTION_ROADMAP_V7.md` — sequencing and constraints.

The demo shows the business architecture and customer workflow. Production needs independently tested accounting rules, explicit data provenance, authentication, database transaction isolation, server-owned journal posting, approvals, FI-level authorization, external APIs/consent, reversals, regulatory/privacy review and reconciliation.
