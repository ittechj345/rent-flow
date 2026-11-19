# RentFlow – Review, Fixes & Roadmap

## Bugs fixed in this version
1. **Login screen never appeared** – app waited for cloud-sync init, which only ran after login. Now local-only mode works.
2. **Cloud sync was completely broken** – wrong action name (`syncAll`), no `userRole`, `no-cors` (unreadable response), response shape mismatch, local keys mismatch. Replaced by one token-protected `sync` action with last-write-wins + deletion tombstones (client `lib/sync-merge.ts`, server `code.gs`).
3. **Data loss for managers** – saving wrote only the *visible* subset to localStorage, wiping other managers' records. Now `commitCollection()` only replaces what the user can see.
4. **Manager saw nothing** – access check compared room/bill ids with user ids. Records now carry `ownerId`.
5. **Security** – plain-text passwords, hardcoded admin password + fallback password, password printed to console, reset token in activity log, self-signup could become admin. Now SHA-256 salted hashes (old passwords auto-upgrade on login).
6. **PWA** – service worker registered as `/sw.ts` (never works), manifest icons mismatched, pinch-zoom disabled. Now `/sw.js`, PNG icons, zoom allowed.
7. **Billing** – overdue never computed, revenue counted unpaid bills, duplicate bill for same room+month allowed, negative units allowed, advance auto-subtracted every month, previous reading typed by hand.
8. Type errors hidden by `ignoreBuildErrors` – fixed and flag removed.

## New features
- Previous meter reading auto-filled, unit rate remembered
- Water / maintenance / other charges on bills
- Duplicate-bill and overpayment validation
- Derived status (paid / pending / **overdue**)
- Dashboard: collected vs billed chart (6 months), collection rate, outstanding, overdue list
- WhatsApp reminder link (pre-written message) on bills
- Cloud settings: sync token field + real connection test

## Still to do (recommended order)
1. **Real backend** (Supabase/Firebase). localStorage + Apps Script is fine for 1 landlord, not for many users; passwords/roles are still enforced only in the browser.
2. Partial payments history (payments[] per bill) + printable PDF receipt
3. Tenant portal (tenant sees own bills, pays via UPI link / QR)
4. Security-deposit & advance ledger, lease expiry alerts
5. Electricity auto-split for shared rooms, meter photo upload
6. Monthly auto-generate bills (copy last month) with one click
7. Backup/restore (JSON export/import) and CSV/Excel export
8. Bengali/Hindi language toggle
9. Split the 1100-line `advanced-room-agreement.tsx`, `support-chat.tsx` into smaller components; remove duplicate `admin-panel`/`enhanced-admin-panel`

## Apps Script setup (new)
1. Paste `google-apps-script/code.gs`, edit `SECRET` in `setSyncToken()`, run it once, then delete the secret from the code.
2. Deploy as Web app (Execute as: Me, Access: Anyone) → copy URL.
3. In RentFlow: Settings → Cloud Sync → paste URL + the same token → Test.
Demo logins: admin@rentflow.com / admin123 (change immediately).


---
# Version 2 – what was added (Google login, PIN/biometric, collections)

## Sign-in & lock
- **Google sign-in** on the login screen. The Google ID token's signature, issuer, audience and expiry are verified in the browser against Google's public keys.
  1. Google Cloud Console → APIs & Services → Credentials → *OAuth client ID* (Web). Add your site URL under **Authorized JavaScript origins**.
  2. Paste the Client ID: login screen → "Set up Google sign-in" (or set `NEXT_PUBLIC_GOOGLE_CLIENT_ID`).
  3. A Google email that matches an existing user logs into that account; a new email becomes a *manager* (never admin).
- **PIN lock** (More → Security): PBKDF2-hashed, 5 wrong tries = 30 s wait (doubling), 15 wrong tries = full sign-out. Locks when the app is in the background (30 s / 1 min / 5 min / immediately) and after 5 min idle.
- **Fingerprint / Face unlock** via WebAuthn (platform authenticator). Needs HTTPS + a screen lock on the phone. The PIN always works as backup.
- Honest limit: PIN/biometric protect the *device session*. The data still lives in browser storage + your Google Sheet, so real multi-user security needs a backend (Supabase/Firebase).

## Rent collection ("Collect" tab)
- Bill statuses: Pending · Partly paid · Paid · Overdue · Disputed · Waived (always derived, never stale).
- Payment ledger per bill: many payments, date, mode (UPI/cash/bank/cheque), UTR, receiver, **printable receipt** (Print → Save as PDF).
- **Tenant "I have paid" → landlord Confirm/Reject.** Only confirmed money counts.
- If rent is not received: reminder ladder (3 days before → due day → +1 → +3 → +7 → +15 final notice) with ready WhatsApp messages in **English or Bengali**; **promise-to-pay date** pauses reminders; call/note log; **late fee** (grace days, flat or per-day, cap; waivable); mark disputed; waive remaining.
- Tenant portal ("My Rent"): a tenant whose email is on their renter profile sees only their own bills and receipts.

## More tab
Profit & expenses (+ vacancy loss) · Security-deposit ledger · One-click monthly bills + meter-reading entry · Lease-expiry alerts · Fees & receipt settings · JSON backup/restore + CSV for Excel · Activity log (who changed what).

## Apps Script
Re-deploy `google-apps-script/code.gs` (new collections expenses / deposit_txns / audit_log sync automatically).

## Tested
Unit tests: status, partial/full payment, claims, late fee, reminder ladder, merge/tombstones, PIN lockout, Google token verification (valid + forged/expired/wrong-app tokens). Browser test (Playwright): login → overdue bill → payment + receipt → PIN set → lock on reload → wrong/right PIN → tenant claim → landlord confirm → PAID. Production build passes with type-checking on.
Not tested: real Google sign-in popup and real fingerprint/Face prompt (need your Client ID and a phone).

---

## v3 additions (no-backend items)

- **A− / A+ text size** in the header (80–150%), saved on the device; **Light / Dark / Auto** theme; **English / বাংলা / हिन्दी** for menus, rent collection, tenant screen, repairs, notices, lock screen and reminders.
- **Install guide** (Android, iPhone "Add to Home Screen", desktop), **offline banner**, **error boundary** (a crash shows a recovery screen, not a white page), **Terms / Privacy** pages, **erase all data**.
- **Repairs (maintenance)**: tenant or landlord logs a request with photo + urgency → assign a mistri (saved contractor list, WhatsApp button) → status → mark done with cost and who pays → landlord-paid cost becomes an expense automatically. Tenant can reopen if not fixed.
- **Notice board** (landlord posts, tenants read, WhatsApp share), **"Needs your attention today"** panel on Home, red badge on Repairs.
- **Branding**: your app name and logo in the header.
- **UPI pay**: set your UPI ID in More → Fees & Receipt. "Pay by UPI" shows a QR + "Open UPI app" with the amount filled in; the UPI ID is added to WhatsApp reminders. Tenant records the UTR, landlord confirms.

### Still needs a backend (Supabase recommended)
1. Tenant login on their **own phone** (today the shared sync token = full-sheet access, so tenants should use the landlord's device or wait for the backend).
2. Real push / SMS / WhatsApp notifications.
3. Automatic payment confirmation (payment gateway webhook).
4. Server-side security, per-user access rules, proper audit trail.

### Not tested here
Real Google sign-in popup (needs your Client ID), real fingerprint/face prompt (needs a phone), and the hand-off into GPay/PhonePe (needs a phone).

---

## v4: Cloud account + AI agent
- Supabase cloud account (email/password or Google) with sync through `/api/cloud/sync`. Old Google Sheets sync still works if no cloud account is signed in.
- `lib/agent/*`: rules engine (reminder ladder, tenant claims, monthly bills, repair triage, stuck repairs, lease expiry, duplicate payments, daily digest), optional Claude layer, de-duplicating action queue.
- `/api/agent/run` (daily Vercel cron + "Check now"), `/api/agent/state`.
- **More → AI Agent**: daily digest, approval queue, send-next-reminder, edit message, autonomy settings, history.
- See `SETUP_SUPABASE_AGENT.md`.

## v5: smarter agent
- `lib/agent/insights.ts`: tenant payment profiles, pattern alerts, read-only query tools.
- `lib/agent/llm.ts`: validated Claude helpers (reminder tone, mistri message, reply reading, tool-using Q&A).
- `/api/agent/ask`, `/api/agent/reply` with a daily limit. UI: Ask the agent, Paste a tenant's reply, "Why" lines on actions.
