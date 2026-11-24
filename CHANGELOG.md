# CoopVault v13.2 — Stage 2 (trial)
Browser-only trial. **Use test data only.** Data lives in the browser it is entered in.

## Open it
Upload `index.html`, `sw.js`, `manifest.webmanifest`, `icon.svg`, `.nojekyll` to the GitHub repo (replace the old files). The first screen is a setup wizard: the app starts empty.

## What Stage 1 has
- Empty start; first-run setup creates the Administrator. No hard-coded members, loans or sample data.
- Seven windows: Member, Manager/Accountant, Credit, Management, Education, Supervisory, Administrator.
- Login: PIN (PBKDF2 hashed) + second step (real TOTP, RFC 6238). Trial mode shows the code on screen; switch it off for real use.
- Lockout after 5 wrong tries, idle logout, step-up code for sensitive actions.
- Privacy by role (masked vs full), Supervisory read-only, Administrator masked with recorded temporary access.
- Footprints: append-only SHA-256 chained audit trail; members see who opened or changed their data; any change notifies the member.
- Membership applications and admission (Management), General Meeting confirmation, activation codes.
- Loan Policy and By-laws as clause library and editable rules (changes need a resolution reference).
- Feedback loop to the Administrator, notices from Education, encrypted backup/restore.

## Added in Stage 2
- Loans: normal, school fees, emergency. Live policy check with clause numbers; 5 guarantors (accept, decline, pledge less, replace, remind); Manager check, Credit (2 of 3), Executive (3 of 4), ratification list, payout, repayments with receipts, arrears steps (1, 2, 3 months), guarantees released on clearing.
- Returns, appeals, questions and ratings: every request ends with an answer.
- Groups (3 to 10): proposal, member agreement, Management ratification, group loan lines (several liability), joint payout, every member signs off as received.
- Member to Supervisory complaints: two-way, confidential, officers named are excluded.
- Administrator: fake test-data generator (made at run time, editable, erasable) and a test clock. No data is hard-coded.

## Honest limits
Anyone with browser developer tools can alter local data; the chain detects casual edits only. Real protection arrives with the server (Stage 6). Documents are small files (under 200 KB) kept in the browser. Stage 3 (payroll and bank file loop, accountant books, other windows' document vaults, staff bus), Stage 4 (meetings) and Stage 5 (public AI guide, USSD simulator) are not built yet. Tested in Chromium only.

## v13.3 — navigation, assistant, feedback loop
- Browser Back/Forward work across all windows (loan, group, member and complaint pages included). After log-out, Back shows no private screen.
- 🤖 Ask: an assistant on every screen (also for visitors). English and basic Swahili. Answers come from the Loan Policy and By-laws settings and from how the app works; for members it works out their own borrowing limit on the device. It cannot approve or change anything. Unanswered questions go to the Administrator.
- 💬 Feedback on every screen: records the screen and build version, goes to the Administrator's inbox, and the Administrator replies and sets a status (seen, working, fixed, not planned). The sender is notified and confirms “fixed” or reopens it.
- Badges on tabs and a “What do you want to do?” panel on every Home screen.
- System page: a 12-point data-consistency check across receipts, loans, guarantees, groups and the audit chain.

## Deploy and test (free)
**Option A, on your laptop (safest):** in this folder run `python3 -m http.server 8000`, then open http://localhost:8000 in Chrome. Data stays in that browser only.
**Option B, GitHub Pages:** upload every file here (including the hidden `.nojekyll`) to the repo, then Settings → Pages → Deploy from branch → main / root. Free Pages needs a public repo; that is fine because the code holds no secrets and no data. Do not enter real member data and share the link with nobody until the Supabase version.
**Phone:** open the Pages link in Chrome, menu → Add to Home screen (installs as “CoopVault”). Phone data is separate from the laptop's.
**First run:** the setup screen asks for your society name and creates the Administrator. Leave “Fill the app with editable sample people and activity” ticked: you get sample people for every role, loans at every stage, complaints, a group, an application, expenses and data requests, all made at run time and all editable. The login page then shows “Try a window” buttons (no PIN) and every screen has a Window switcher. Turn that off in Rules before real use. Normal PIN login still works with the test PIN shown in System.
**Report problems:** 💬 Feedback on any screen; read them in the Administrator's Feedback inbox.

## v13.4 — Stage 3, drop 1: data files, books, vaults
- **Data files (Manager):** request a payroll, bank or M-Pesa file, upload it when it arrives by email (PDF, Excel .xlsx, CSV, photo; under 400 KB). Payroll files are matched to members by payroll number, differences are flagged, unknown/repeated numbers are never posted, and posting puts loan instalments first then savings, with receipts. Posting twice is blocked.
- **Books (Manager writes; Management and Supervisory read-only):** cash book, trial balance, income and expenditure, balance sheet, expenses with payment vouchers (above the limit in Rules an Executive officer approves), bank/M-Pesa/cash reconciliation, month closing (closed months reject back-dated entries; an Executive officer can reopen with a reason), monthly statement (due within 10 days of month end).
- **Document vaults:** Manager, Credit, Management, Education and Supervisory each have a vault; the Supervisory Committee can open all and every opening by someone else is logged.
- System page: 17 consistency checks (5 new for the books).
- Not yet: M-Pesa auto-matching (parked at your request), the staff bus, opening balances, year-end close and dividends. The bus comes in drop 2.

## v13.4b — see every window
Sample society and “Try a window” (trial only). The app has no hard-coded society name; you type it at setup. Only the Loan Policy and By-laws are built into the code.

## v13.5 — documents routed by role, training, attachments
- **Members → My documents:** send ID copy, payslip, nominee form or proof of payment to the Accountant or the Credit Committee. The receiving office verifies or rejects with a reason; the member is told. Loan documents still go on the loan page (Accountant verifies, Credit and Supervisory can read).
- **Applicants** can attach an ID copy and payslip to the application; the Accountant, Credit and Executive can open them, and on admission they move to the new member.
- **Management, Credit, Education → Vault → “Send to the Accountant for filing”:** receipts, vouchers, invoices, resolutions. The Accountant marks them filed with a reference and the sender is told.
- **Education → Vault → “Publish to all members”:** members are notified and find it under **Training**.
- **Complaints:** a member (and the Supervisory Committee in replies) can attach a document; only the member and Supervisory can open it, and not an officer the complaint names.
- **Supervisory:** “Every document” list, read-only, every opening logged.
- **Screenshots:** 💬 Feedback accepts a screenshot file and, where the browser allows it, “Capture this screen”. Images are shrunk to about 200 KB and go to the Administrator only. Until there is a shared server, testers use “Download my feedback” and send the file to the developer, who adds it under Feedback → “Bring in feedback from testers”.
- Group constitution upload removed. Files stay under 400 KB (200 KB for loan documents) during the browser trial.

## v13.6 — Staff bus (Accountant)
- New **Bus** window: Manager writes; Management and Supervisory read-only; other roles do not see it.
- Daily log (trips, odometer checks), bookings with partial payments, money received (M-Pesa code, bank transfer, cash, cheque) with numbered receipts posting to Books, expenses with vouchers (above the limit they wait for an Executive officer), garage records, driver details, expiry alerts (insurance, inspection, licence, driver licence, PSV, service due), monthly summary.
- Attachments (slips, receipts) open for Manager, Executive officers and Supervisory only.
- Test tools: "Add sample bus records". Consistency checks now 21.

## v13.7 — Page one shows every window
- First page lists all seven windows (Member, Executive, Credit, Education, Supervisory, Manager/Accountant, System Administrator). In trial mode a tap opens that window; with one-tap off it goes to Log in, naming the chosen window.
- Back, Forward and Home buttons at the top of every page; "All windows" when logged in.

## v13.8 — Own login, visible PIN, change PIN, Previous/Next
- Testers can "Create my own login" (full name, phone number, own PIN; no email at this stage). Login works with the ID (TV-0001) or the phone number. Testers get six windows (not System Administrator) and switch with "All windows" without logging in again. Everything they do is recorded under their name.
- Every PIN box has a Show/Hide button. "My login" (top bar) changes PIN and contact details; changes are in the audit trail.
- Previous / Next buttons at the bottom of every page step through that window's tabs (or the public pages) in order.
- A failed form keeps what you typed (PINs are never kept).
- Phone numbers are NOT verified by SMS yet: that needs the secure server (Supabase stage).

## v13.9 — Back/Home fix, working tester windows, flow audit
- Browser Back/Forward no longer stall: stale history entries (other windows, before login) are skipped; at the start of a window a real user stays put with a message; sample/tester users return to page one.
- Tester logins start with a sample savings history and an Executive seat (choose at sign-up) so Member, Executive, Credit, Manager, Supervisory and Education windows all work.
- Audited a full loan from a tester: guarantors, Manager, Credit, Executive, payout. Each stage shows in the right window's badge and notifies the right person.
- Consistency check no longer flags tester accounts for holding many roles.

## v13.10 — Editing and flow audit
- Bus: Edit on trips, bookings, drivers, garage records; Void on receipts and vouchers (kept on file, out of the Books). Reasons are mandatory and audited. Closed months block voids.
- Education: edit or withdraw a notice; take a training file down.
- Administrator (trial): developer data editor in System for any sample record (secrets hidden, personal details masked, reason + code required, recorded as a developer edit). Switch off in Settings before real use.
- Consistency now has 22 checks.

## v13.11 — Sample figures everywhere (trial mode)
- New loan form opens pre-filled: KSh 30,000, 12 months, a purpose, and five guarantors already chosen. Everything can be changed.
- If there are not enough members to guarantee, the form says so and offers "Add sample members, officers and activity".
- "Trial helper" card on every window's Home adds sample members, officers, activity and bus records when missing.
- Every form (except login, PIN and security forms) has "Fill sample values" in trial mode; edit before sending.

## v13.12 — Change the figures behind the loan rules (trial)
- Members: "Trial: change my figures" on Home and on the loan form: savings, share capital, gross pay, months in the society, society reserves. Manager and Administrator can do the same for any member from the member's profile. Savings/shares change through a recorded entry (Books stay balanced) and everything is audited.

## v13.13 — The Accountant corrects balances
- Manager/Accountant: "Correct a balance" on every member's profile and in Money: set the right total for savings or shares with a reason. A numbered correcting entry is posted, the member and the Supervisory Committee are told, it is audited, and closed months are protected. Works with or without trial mode.
- "Fill sample values" now fills required dates, odometer readings, distinct members in group forms, and rule values.
- Live check of every form in every window (submit with sample values): all work; the only refusals are real rules.


## v13.14 (22 Nov 2026) step 1 of the agreed ledger design: planning data
- New records, all generated at run time, marked as test data, editable in System > Developer data editor, with an "Add or restore planning samples" button: budget lines (one per expense category), change requests, a supplementary budget, introductions of new members.
- Every sample member now has full next of kin (name, relationship, ID or passport, phone, alternative phone, residence, share %), shown on the member profile to officers who may see personal details.
- New expense categories: Tax and statutory, Insurance, Referral tokens. Sample tax, insurance and salary vouchers.
- New rules: introducer token (KSh 500) and bus referral token (5%).
- Fix found while building: a version-label edit had removed the storage key, so nothing saved. Restored and tested.

## v13.15 (22 Nov 2026) step 2: change requests and month close
- New Changes tab (Executive, Accountant, Credit, Supervisory). An Executive officer starts a change (correct a balance, payroll variance limit with recovery plan, change a rule, other), all four officers sign with the second step, the Accountant carries it out. One refusal closes it. Credit and Supervisory read only.
- Month close: the Accountant prepares and sends; the Treasurer and the Chairman both sign to close; the Accountant, Treasurer and Chairman can add notes.
- No member alert when the Accountant works on a member's account (balance corrections). The audit trail and the Supervisory notice stay.
- New rule: payroll variance tolerance (stored; applied to payroll checks in step 4).
- Sample data now includes a request waiting for the Accountant.

## v13.16 (22 Nov 2026) payout queue and member notices
- A plain member now gets only two kinds of notice: their number in the payout queue as it moves until they are next, and the prompt to confirm the payout account. All other notices to members are off. Officers and staff are unaffected.
- Payout queue: approved loans are ordered by approval time. At number 1 the member confirms M-Pesa (checked number) or bank details with their code. The Accountant cannot pay out until it is confirmed; the Loan desk shows the queue and each destination.
- Sample data fills a sample destination so the demo still runs.

## v13.17 (22 Nov 2026) sample payslip and bank slips
- Sample documents are drawn at run time from the member's own record (name, payroll number, gross pay, active loan instalments, savings) and from the loan (amount, reference, confirmed payout account). Nothing is stored in the code. Percentages for deductions are in Rules. Every page is stamped SAMPLE and uses "Sample Bank Ltd" (change it with S.sacco.sampleBank in the developer editor).
- Member loan page: "Attach a sample payslip". My documents: "Attach a sample bank deposit slip" and "Attach a sample payslip". Accountant payout card: "Download a sample transfer advice" (reference = loan number).
- Administrator, System, Trial tools: "Sample documents" makes and downloads any of the three for any member.
- (v13.17 addition) Loan page: "Attach sample payslips for me and my N guarantors" adds the applicant's payslip plus one payslip per guarantor, each from that person's own record. Guarantor payslips show as "Guarantor payslip" for the Accountant to open and verify; they are not required by the rules.

## v13.18 (24 Nov 2026) payslip goes to Credit
- Guarantors attach no payslip (the optional sample guarantor payslip button from v13.17 is removed).
- The applicant's payslip is forwarded to the Credit Committee for review: Credit marks it Verified or Rejected while the loan is with them. The Executive and the Accountant can open it. The Accountant no longer has to verify it before sending the loan on, but it must be attached and the gross pay recorded.
