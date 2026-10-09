# Halal Nikah · ሀላል ኒካህ

Amharic + English app for Muslims seeking halal marriage. No npm dependencies: only Node.js 22.5+.

## Run
1. `cp .env.example .env` and fill in YOUR phone, password, bank details and a random SECRET.
2. `node --no-warnings server.js` then open http://localhost:3000
3. Admin (you): log in with ADMIN_PHONE / ADMIN_PASSWORD; you land on `/admin.html`.

## How it works
- New user registers and can browse for **60 seconds free** (TRIAL_SECONDS in `.env`). A countdown shows at the top.
- When the time ends the app asks for the one-time **20 ETB** fee, enforced on the server.
- User transfers 20 ETB to your bank account (shown from `.env`), types the sender name, presses **I have paid**.
- CBE sends *you* an SMS "credited with ETB 20.00 ...". An SMS-forwarder app on your Android phone POSTs each SMS to your server; the server reads the amount, matches it to the waiting user (by sender name, or when only one payment is waiting) and unlocks the account automatically. The user's screen updates by itself.
- If a payment can't be matched, it stays pending: use `/admin.html` (shows pending payments + received bank SMS) and press Approve manually.
- Sending interest needs payment even during the free minute.

## SMS forwarder setup (Android, on the phone that receives CBE SMS)
1. Put `WEBHOOK_SECRET=<random 16+ chars>` in `.env`.
2. Install any SMS-forwarder app that can send an HTTP POST with JSON.
3. Rule: only SMS from the CBE sender -> POST `https://YOUR-DOMAIN/api/sms-webhook?key=YOUR_SECRET`, JSON body `{"text":"<message text>"}`.
4. Send yourself a test payment and check the "Bank SMS received" table in `/admin.html`.
Only incoming ("credited") SMS are used; debits are ignored; the same SMS twice counts once.

## Putting it on phones
- Host on any HTTPS server (set NODE_ENV=production). Keep `data.db` on a persistent disk and back it up.
- Users open the site in Chrome and choose "Install app / Add to Home screen" (PWA).
- For Play Store: wrap the HTTPS URL with Bubblewrap or PWABuilder (TWA).

## Privacy & safety
Phone numbers are never public. Contact is revealed only on mutual interest; for women it is the wali's contact. 18+ enforced. Login rate-limited. Admin can block users.
Before launch add terms and a privacy policy.
