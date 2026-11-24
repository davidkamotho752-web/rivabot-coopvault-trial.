# CoopVault flow audit (v13.10, 21 Nov 2026)

## Built and tested
- Roles and windows: Member, Executive, Credit, Education, Supervisory, Manager/Accountant, System Administrator; page one lists them; back/forward/home; previous/next.
- Loan flow end to end (5 guarantors -> Manager -> Credit 2 of 3 -> Executive 3 of 4 -> payout), each stage visible in the right window with notifications.
- Groups, complaints (two-way, attachments), documents routing, training publishing, Books, month close/reconciliation, expenses with Executive approval, bus module, feedback with screenshots, AI assistant, footprints, 2-step login, tester sign-up.
- Editing: member details (with notice to the member), rules, notices, bus records, voids, developer editor (trial).

## Asked for earlier, NOT built yet (Stage 4 unless noted)
1. Withdrawal / exit: 90-working-day notice, KSh 5,000 deduction, blocked while owing or guaranteeing (rules text exists; no flow).
2. Top-up / second loan (needs Management + Supervisory approval per Loan Policy).
3. Member statement view and download/print.
4. Arrears penalties/default handling beyond the reminder steps.
5. Dividends and interest on shares, year-end close, opening balances.
6. Meetings, General Meeting, minutes, officer handover, declarations of interest, notices to all, dormant members, nominee payouts.
7. Public AI guide in English and Swahili, policy library, USSD/SMS simulator for analogue phones.
8. M-Pesa automatic matching (parked by you).
9. Supabase: real security, SMS verification of phone numbers, backups.

## Known limits
- Browser-only data; Chromium tested only; files in browser; feedback reaches the developer as a file until Supabase.
