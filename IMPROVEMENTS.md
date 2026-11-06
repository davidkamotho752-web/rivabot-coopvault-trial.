# Blueprint v11 → suggested changes for v12

## A. Contradictions to fix before build
1. **Supervisory "zero write access" vs. duties.** They must peer-review access and sign attestations. Redefine as "no business writes; append-only sign/review actions". (Built that way.)
2. **Guarantor pledges "must equal" the loan** and "3× savings": fine, but also decide what happens when a guarantor declines (replace vs. restart). Trial: application is declined; member re-applies.
3. **Who can guarantee.** Blueprint bars Management + Supervisory only; your real practice says officials can never guarantee. Trial bars ALL officials (Manager, Credit, Executive, Supervisory, Education). Your practice also allows partial guarantees and up to 10 members per guarantor — v11 says exactly 5 guarantors. Pick one and write it into section 3.
4. **"Offline-capable" vs. two-way verification, WebAuthn, SMS OTP, double-entry.** All need the server. Offline should be read-only cache + queued *drafts* (applications, queries). Never queue money postings.
5. **Max loan 3× savings** ignores existing loans. Trial uses 3× savings minus outstanding principal, and one application in progress at a time.
6. **"Total deductions ≤ 2/3 of gross"**: state clearly that it includes existing deductions plus the new instalment (built that way).
7. **Executive "can access ANY data" + holds the signing keys.** Executives can also be borrowers/guarantors. Add: an Executive cannot sign or access anything about their own or a relative's loan.
8. **No-SASRA directive.** If Rivabot takes deposits (BOSA/FOSA) it is likely licensed/regulated by SASRA. Get your legal advisor to confirm in writing before dropping reporting; at minimum keep the data model able to produce returns later.

## B. Security gaps
- Watermark: v11 CSS puts it *under* content (z-index 0), so cards hide it. Moved *above*, click-through, and made it dynamic (user + timestamp) so a leaked screenshot is traceable. (Built.)
- Hash chain is only tamper-evident if the head hash is anchored outside the database (daily email to Supervisory / external timestamp).
- Token split (3+3 digits) is only ~1,000 guesses per half. Use a longer token, 10-minute expiry, 3 attempts, single use.
- Exec access needs a reason *category* + free text, a time-boxed view (e.g. 15 min), and auto-revoke.
- Break-glass / absent-officer rule: what if 2 of 4 executives are unavailable? Define alternates.
- SMS to feature phones can't do WebAuthn: fine for officials, but add a PIN + SIM-swap check for members.
- Replace localStorage with Supabase (RLS per role) before real data. Never ship real member data in this trial.

## C. Missing pieces worth adding
- Dividends/interest on savings, share capital, withdrawals/exit, and loan top-up/refinance.
- Arrears, penalties and default escalation (guarantor recovery order).
- Member statement export (PDF/CSV) and M-Pesa C2B auto-match.
- Versioned policy with effective dates (trial: Exec 3-of-4 policy proposals change live values).
- Full Kiswahili UI toggle (trial: EN UI, EN/SW materials).
- Data-protection (Kenya DPA 2019): consent record, retention, data-subject requests.
- Accessibility test on a real feature phone / 2G throttling.


---
# v12 status of the "missing pieces"
| Item | Status in trial |
|---|---|
| Dividends / interest on savings | Built: Manager prepares, 3 of 4 Executives sign, limited to distributable surplus. Simplification: rate applies to current balance, not month-weighted. |
| Share capital | Built: recorded by Manager, separate from savings, paid back on exit. |
| Withdrawals / exit | Built: limit = free savings, keeping minimum and 1/3 of loans; exit needs no loans or pledges; paid via 3-of-4 batch. |
| Loan top-up / refinance | Built: new loan clears old balance; cash paid = difference; same guarantors can re-pledge. |
| Arrears, penalties, default | Built: reminder → guarantors told → default → Executive calls guarantees pro rata. **Assumption to confirm:** 5% penalty, 30/60-day thresholds (not in the 2009 policy; Executive-editable). Borrower's own savings are NOT set off first — decide if they should be. |
| Statement export | Built: CSV + print/PDF. |
| M-Pesa C2B | Simulator built; real callback is a template in `backend-scaffolds/mpesa-c2b.ts` (needs Daraja registration + server). |
| Accessibility / 2G / feature phone | Lite view, labels/keyboard/contrast pass axe. 2G: 117 KB raw, ~37 KB gzipped on GitHub Pages ≈ 7 s on Slow 2G (cold), then offline via service worker. **Not tested on a real feature phone**; Java/Opera Mini phones cannot run this app. For those, USSD/SMS (e.g. Africa's Talking) is the realistic channel. |
| Not done | Versioned policy, Kiswahili UI, data-protection records (items 4-6 of the list). |

## AI assistant — what it is and is not
Built-in: answers from the member's own records and the policy rules (balance, limit, "can I borrow X", cost of a loan, next instalment, status, queue, guarantees, penalties, withdrawal/exit, dividends), English and Kiswahili. Swahili wording covers the main answers only and should be reviewed by the Education Committee.
Optional LLM: Executive sets an HTTPS endpoint; only the member's own figures (no phone/ID/other members) are sent; falls back to built-in answers if unreachable. The assistant cannot move money. For production, rebuild the context server-side from the database and get member consent (Kenya DPA 2019).
