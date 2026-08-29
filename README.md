# Dawn Mickens — Digital Business Card

Single-brand card for Timeless Connection Wedding Officiants, matching the
Officiant tab styling from the multi-brand card.

## 1. Host it on GitHub Pages

1. Create a new GitHub repo (e.g. `dawn-mickens-card`) — public repos get free Pages hosting.
2. Upload `index.html` and `dawn-mickens.vcf` to the repo root.
3. Go to **Settings → Pages**.
4. Under "Build and deployment," set **Source: Deploy from a branch**, branch `main`, folder `/ (root)`.
5. Save. GitHub will give you a live URL like `https://<username>.github.io/dawn-mickens-card/` within a minute or two.

Optional: if you own a domain, add a `CNAME` file with the domain name and point a DNS CNAME record at `<username>.github.io`.

## 2. Wire "Check Our Date" to Google Sheets

This uses a free Google Apps Script Web App — no backend needed.

1. Create a new Google Sheet (e.g. "Dawn Mickens — Leads"). Add a header row:
   `Name | Email | Phone | Wedding Date | Submitted At`
2. In the Sheet, go to **Extensions → Apps Script**.
3. Delete any starter code and paste this:

   ```javascript
   function doPost(e) {
     const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
     const data = JSON.parse(e.postData.contents);
     sheet.appendRow([
       data.name,
       data.email,
       data.phone,
       data.weddingDate,
       data.submittedAt
     ]);
     return ContentService.createTextOutput(JSON.stringify({status:"ok"}))
       .setMimeType(ContentService.MimeType.JSON);
   }
   ```

4. Click **Deploy → New deployment**.
5. Select type **Web app**.
6. Set "Execute as" to **Me**, and "Who has access" to **Anyone**.
7. Click **Deploy**, authorize it, and copy the **Web app URL** it gives you.
8. Open `index.html`, find this line near the bottom:

   ```javascript
   const SHEET_ENDPOINT = "PASTE_YOUR_GOOGLE_APPS_SCRIPT_URL_HERE";
   ```

   Replace the placeholder with the URL you copied. Re-upload the file to GitHub.

That's it — every "Check Our Date" submission will land as a new row in the Sheet, with no third-party lead-capture service involved.

## Notes
- Contact rows (phone, email, site, IG, FB) reuse the shared Timeless Connection details, since those are the same for both officiants.
- "Add to Contacts" downloads `dawn-mickens.vcf` directly — no third-party service.
- Colors/typography intentionally match the existing multi-brand card (dark ground, gold crest, rose accent, serif display name) for brand consistency.
