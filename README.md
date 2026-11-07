# CoopVault · Rivabot SACCO — Trial Build v12

Single-file PWA (`index.html`) with all 7 role windows. Demo data lives in the browser (localStorage). **OTP for every login: `123456`.**

## Put it on GitHub Pages (5 steps)
1. Create a NEW repo, e.g. `rivabot-coopvault` (don't overwrite your existing `coop-vault` repo).
2. Upload everything in this folder, including the hidden `.github` and `.nojekyll` (or `git init && git add -A && git commit -m "trial" && git push`).
3. Repo → Settings → Pages → Source: **GitHub Actions**.
4. Push to `main` (or Actions → "Deploy CoopVault to GitHub Pages" → Run workflow).
5. Open `https://<your-username>.github.io/rivabot-coopvault/`.

## Trial script (one full loan, ~5 min)
Member **Faith Njeri** → Loans: amount 100000, pick 5 guarantors, Auto-split, attach any file, Submit →
each guarantor (Guarantees tab) Accept → Credit **c1,c2,c3**: Verify payslip, Approve → Executive: Loans → Sign off →
Manager: Batches → tick loan, Prepare → Executives **e1,e2,e3**: Pending → Sign → loan goes Active; schedule appears under the member's Loans.
Log out / switch user from the header at any time. "Reset demo data" is reachable from the error screen or via `CV.ACT.resetdemo()` in the console.

## Windows
Member · External Member · Manager · Credit · Executive · Supervisory · Education (login → role → person → OTP → security-key step for Manager/Executive).

## New in v12
- **Complete simulation** (login screen → "Run complete simulation"): starts from an EMPTY SACCO, registers 9 members, runs 8 months of M-Pesa C2B contributions, loans (approve / decline / guarantor refusal), arrears → penalty → default → guarantees called, top-up refinance, withdrawal, exit, dividend run, journals, period close + attestation, Executive access with split token, policy change, education, assistant — 40 checked steps, then renders every screen of every role and reconciles the books. Afterwards log in as any role to inspect the result.
- **Start with an empty SACCO**: nothing pre-loaded; the Manager registers members and everything else flows from entered transactions.
- **⏩ +30 days** button (header): moves the system date forward so arrears, penalties, default and period close can be seen without waiting.
- **System health panel** (Manager Today, Supervisory Ledger): 12 live reconciliation checks (Dr=Cr, hash chain, savings/shares/loans vs ledger, statements vs balances, pledges, bank, batches…).
- **Assistant**: reads the member's own live records (EN + Kiswahili); optional LLM endpoint set by Executive (Policy tab) — see `backend-scaffolds/`.
- **Lite view** (button in header / login): bigger text, higher contrast, flatter layout. Accessibility scan (axe, WCAG 2 A/AA): 0 violations on 9 screens.
- Shares, interest/dividend runs, withdrawals, exit, top-up/refinance, arrears + penalties + default escalation, CSV/print statements, M-Pesa C2B simulator.

`backend-scaffolds/*.ts` are UNTESTED templates for the real server side (AI proxy, Daraja C2B).

## v12.1 (7 Nov 2026)
- First page: **Quick open** (one tap into any window, no SMS code) and **Run self-check** (tests every window on your device and says what is wrong). Build number shown at the bottom of the first page — if it does not say v12.1, your browser is showing an old copy.
- New functions: member cancels an application / withdrawal; Executive rejects a payment batch or dividend run; Manager reverses a wrong manual journal.
