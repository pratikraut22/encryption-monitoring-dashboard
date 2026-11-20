# Encryption Monitoring Dashboard

## Included
- `index.html` — dashboard interface
- `style.css` — responsive visual design
- `app.js` — filters, charts, Excel import, local history and CSV export
- `data.js` — starter data extracted from the uploaded October 2026 workbook

## Run locally
You can open `index.html` directly in a modern browser. For best results, use a local static server:
- VS Code: install Live Server, then choose **Open with Live Server**
- Or run `python -m http.server 8000` in this folder and visit `http://localhost:8000`

Internet access is needed to load the SheetJS and Chart.js libraries from their CDNs.

## Deploy to Cloudflare Pages (free)
1. Create a GitHub repository, for example `encryption-monitoring-dashboard`.
2. Upload all four files (`index.html`, `style.css`, `app.js`, `data.js`) and this README.
3. In Cloudflare, open **Workers & Pages → Create → Pages → Connect to Git**.
4. Select your repository.
5. Framework preset: **None**. Build command: leave blank. Build output directory: `/` (or `.` if requested).
6. Deploy. Cloudflare will provide a `*.pages.dev` address.

## Monthly upload workflow
1. Open your deployed dashboard.
2. Click **Upload monthly Excel**.
3. Select the next month's workbook.
4. The dashboard reads `WEEKLY REPORT`, `Main - Star` and (if present) `SUMMARY`.
5. The month is saved in this browser's IndexedDB history and can be selected from the Reporting Period dropdown.

Important: Browser history is local to the browser/device where the file was uploaded. It is not shared automatically with other users or other devices. For shared central history, add a backend/database in a later phase.

## Workbook expectations
The import parser expects:
- `WEEKLY REPORT`: headers on row 2, network rows beginning row 3, with the fields OP ID, STATE, CITY, MACHINE NO, OPERATOR NAME, Count, CATEGORY, Operation Name, Week 40–44, Status, INC, As Per Billing, BUDGET and FINAL AMOUNT TO PAY.
- `Main - Star`: network rows beginning row 3; daily status columns start in column I and are spaced every three columns, with dates in row 2.
- `SUMMARY`: optional daily summary with Date in column C and the status totals in columns D–I.

## Notes
- The starter dashboard uses cached/calculated values from the supplied workbook.
- New uploads are parsed in the browser; no file is sent to a server by this static dashboard.
- Filters apply to network-level and operator/billing/weekly analytics. Daily summary charts reflect the full uploaded workbook because the workbook's daily summary is not split by operator/state in the source structure.
- KPI labels and calculations follow the workbook's own field names. Weekly counts are summed from the five week columns in `WEEKLY REPORT`.
