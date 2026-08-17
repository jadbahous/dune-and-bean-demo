# Lead sheet + email notifier (Google Apps Script)

This turns a Google Sheet you own into the storage + notification backend for
the lead widget. It's free, needs no API key, and runs entirely under your
own Google account — nothing to install.

## One-time setup (about 5 minutes)

1. Go to **sheets.google.com** and create a new blank spreadsheet. Name it
   something like `Lead Widget — Leads`.
2. In the sheet, go to **Extensions → Apps Script**. A new tab opens with a
   code editor.
3. Delete the placeholder code in `Code.gs` and paste in the contents of
   `Code.gs` from this folder.
4. On line with `var ownerEmail = ...`, replace the email address with
   whichever inbox should get lead notifications (defaults to
   jad.bahous@gmail.com).
5. Click **Deploy → New deployment**. Click the gear icon next to
   "Select type" and choose **Web app**.
   - Description: `lead widget endpoint`
   - Execute as: **Me**
   - Who has access: **Anyone**
6. Click **Deploy**. The first time, Google will ask you to authorize the
   script — click through the "Google hasn't verified this app" warning
   (it's your own script, this is expected) and allow it.
7. Copy the **Web app URL** it gives you (ends in `/exec`). That's your
   `LEAD_WEBHOOK_URL`.
8. Add that URL as an environment variable in Vercel: Project → Settings →
   Environment Variables → new variable named `LEAD_WEBHOOK_URL`, value is
   the URL you copied. Redeploy after adding it.

## Reusing this for a new client

Duplicate the Google Sheet (or make a new one), redeploy the same script as
a new Web app, and set that client's Vercel project to a different
`LEAD_WEBHOOK_URL`. Each client gets their own sheet and inbox — nothing in
the widget or `/api` code changes.

## Testing it

Once deployed, every submitted lead adds a row to a `Leads` tab in the sheet
(created automatically on first use) and sends an email to the owner
address. If a lead doesn't show up, check the Apps Script "Executions" log
(left sidebar in the Apps Script editor) for errors.
