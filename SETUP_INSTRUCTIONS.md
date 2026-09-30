# Home Loan Document Checklist — Setup Guide

This package has three files:

| File | What it's for |
|---|---|
| `Home_Loan_Document_Checklist.html` | The tool itself. Rename to `index.html` before uploading to GitHub. |
| `Code.gs` | Backend script that writes every submission into a Google Sheet. |
| `SETUP_INSTRUCTIONS.md` | This file. |

Do **Part 1** first (it gives you a URL to paste into the HTML file), then **Part 2**.

---

## Part 1 — Create the Google Sheet backend (~5 minutes)

1. Go to [sheets.google.com](https://sheets.google.com) and create a new blank spreadsheet. Name it something like `Home Loan Checklist Submissions`.
2. In the sheet, click **Extensions → Apps Script**. This opens a script editor tied to this sheet.
3. Delete any placeholder code in the editor, then paste in the entire contents of `Code.gs` (the file next to this guide).
4. Click **Save** (the floppy disk icon), give the project any name (e.g. "Checklist Logger").
5. Click **Deploy → New deployment**.
6. Click the gear icon next to "Select type" and choose **Web app**.
7. Set:
   - **Description**: anything, e.g. "v1"
   - **Execute as**: **Me** (your account)
   - **Who has access**: **Anyone** *(this has to be "Anyone" so the phones running the tool — which aren't logged into your Google account — can send data to it. It does NOT make your spreadsheet public; only this one script endpoint accepts incoming writes, and it only writes, it can't be used to read your sheet.)*
8. Click **Deploy**. The first time, Google will ask you to authorize the script — click through the "Advanced" warning (this warning shows up because it's your own unpublished script, not because anything is wrong) and allow access.
9. Copy the **Web app URL** it gives you. It looks like:
   `https://script.google.com/macros/s/AKfycb.../exec`
10. Keep this tab open — you'll need this URL in Part 2, Step 4.

**Test it:** paste the URL into a browser tab. You should see "Home Loan Checklist logging endpoint is live." If instead you see a sign-in or permission error, go back to step 7 and confirm "Who has access" is set to **Anyone**.

> **Updating the script later:** if you edit `Code.gs` again, you must do **Deploy → Manage deployments → Edit (pencil icon) → New version → Deploy** for the changes to go live. Saving alone isn't enough.

---

## Part 2 — Host the tool on GitHub Pages (~5 minutes)

1. Go to [github.com](https://github.com) and create a new **public** repository (e.g. `home-loan-checklist`). Public is required for free GitHub Pages hosting.
2. Rename `Home_Loan_Document_Checklist.html` to **`index.html`** on your computer (GitHub Pages serves `index.html` as the homepage automatically — this saves your Sales team from typing a filename).
3. Open `index.html` in any text editor and find this line near the top of the `<script>` section:
   ```js
   var SUBMIT_WEBHOOK_URL = 'PASTE_YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL_HERE';
   ```
   Replace the placeholder with the URL you copied in Part 1, Step 9, keeping the quotes:
   ```js
   var SUBMIT_WEBHOOK_URL = 'https://script.google.com/macros/s/AKfycb.../exec';
   ```
   Save the file.
4. On your new GitHub repo page, click **Add file → Upload files**, drag in your edited `index.html`, and click **Commit changes**.
5. Go to the repo's **Settings → Pages** (left sidebar).
6. Under "Build and deployment" → **Source**, choose **Deploy from a branch**.
7. Under **Branch**, choose `main` and folder `/ (root)`, then click **Save**.
8. Wait 1–2 minutes, then refresh the page. GitHub will show your live URL, typically:
   `https://<your-username>.github.io/home-loan-checklist/`
9. Open that URL on your phone to confirm it loads and looks right.

**That's your link to share with the 300–500 Sales users** — via QR code, WhatsApp, Teams, or email. Bookmark it on the home screen for one-tap access on mobile.

---

## Updating the tool later

Whenever you want to change the checklist content or fix something:
1. Edit `index.html` locally.
2. On GitHub, go to the file, click the pencil (edit) icon, paste in the new content, and commit — or just re-upload via **Add file → Upload files** (it will overwrite the existing one).
3. The live URL updates automatically within a minute or two — no need to resend the link to anyone.

## Viewing submissions

Every time someone taps **Submit checklist** (on the primary applicant screen, the co-applicant identity screen, or the co-applicant financial screen), a new row is appended to the **Submissions** tab of your Google Sheet, including:
- Application number and loan amount entered
- The customer/co-applicant profile selected (relationship, loan type, employment, residency, assessment program)
- Which documents were still missing at the time of submission (if any)
- A timestamp and overall status (Complete / Missing docs)

You can filter, pivot, or export this sheet like any other — e.g. to track which branches/RMs are submitting incomplete checklists.

## A note on data sensitivity

This checklist doesn't collect customer PII directly (no name, PAN, phone number fields) — just profile categories, loan amount, and document-completion status. If you later add customer-identifying fields, restrict who can view the Google Sheet (Share settings) accordingly, since the Apps Script deployment itself only accepts writes and can't be used to read data back out.
