# EA LUBHO

Platform for managing and hosting MT5 Expert Advisors. It is not a trading bot.

Folders: `public/` site + PWA files · `functions/api/` Cloudflare Pages Function entry · `src/` API code ·
`schema.sql`, `migrations/` database · `test/` automated tests · `agent/` Windows host agent (pending) ·
`backup/original/` your untouched original index.html · `docs/` status and deploy notes.

**Read docs/STATUS.md first — it says what is finished and what is not.**

## Run the tests (needs Node 22+, not your Windows 8.1 PC)
    npm test

## Deploy (free) — Cloudflare dashboard, no command line
1. Put this folder in a PRIVATE GitHub repo (never upload secrets).
2. Cloudflare → Workers & Pages → Create → Pages → Connect to Git → pick the repo.
   Build command: (empty). Build output directory: `public`. Deploy.
3. Cloudflare → Storage & Databases → D1 → Create database `lubho`. Open its Console and paste the whole
   contents of `migrations/0001_init.sql`, run it.
4. Pages project → Settings → Bindings → Add → D1 database → variable name `DB` → pick `lubho`.
5. (Optional, needs a payment method) R2 → create bucket `lubho-files` → Pages Bindings → R2 bucket → variable `FILES`.
   Skip it and files are stored in D1 (smaller limits).
6. Pages → Settings → Variables and Secrets → add as **Secret** (encrypted):
   - `SETUP_TOKEN`: any long random phrase you invent.
   - `CREDENTIAL_KEY`: 32 random bytes, base64. Generate in PowerShell:
     `$b=New-Object byte[] 32; (New-Object Security.Cryptography.RNGCryptoServiceProvider).GetBytes($b); [Convert]::ToBase64String($b)`
     Back it up somewhere safe: if you lose it, saved MT5 passwords cannot be decrypted.
7. Redeploy. The first-time Super Admin setup uses SETUP_TOKEN and a password YOU choose. No password is built in.

Note: until the front end is connected (pending), the deployed page is still the demo page.
