# Daily Fuel — personal food tracker

A minimal, mobile-first food logging app with dark/light mode, editable calorie/protein estimates, date and time defaults, searchable food choices, repeat/edit/delete, dashboard, history and CSV export.

## Project files
- `index.html` — front end. Runs locally and can be hosted on GitHub Pages.
- `Code.gs` — Google Apps Script API for the Google Sheet.
- `food-tracker-template.xlsx` — optional starter workbook layout.

## Default goals
- Calories: 2,600 kcal/day
- Protein: 150 g/day

These are editable in the `Settings` sheet after setup. The app displays these defaults until the API loads.

## A. Create the Google Sheet and backend
1. Create a new Google Sheet, e.g. `Daily Fuel`.
2. Open **Extensions → Apps Script**.
3. Replace the editor contents with `Code.gs` from this folder and save.
4. In the Apps Script function dropdown, choose `setupSheets` and click **Run**. Approve the requested permissions. This creates:
   - `FoodLog`
   - `FoodDatabase`
   - `DailySummary`
   - `Settings`
5. In **Settings**, set `calorieGoal` to `2600` and `proteinGoal` to `150` if not already present.
6. In `FoodDatabase`, edit the starter foods and add your common foods. Columns are:
   `name | serving | calories | protein | meal | category`
   Calorie/protein values are per the serving described in the `serving` column.
7. Deploy: **Deploy → New deployment → Select type → Web app**.
   - Execute as: **Me**
   - Who has access: **Anyone** (only do this if you accept the risk of a publicly reachable endpoint; do not put sensitive data in this sheet)
   - Deploy and copy the Web app URL ending in `/exec`.

### Security note
This starter uses a public Apps Script web app endpoint so the static GitHub Pages app can call it without a login flow. Anyone who obtains that URL may be able to submit entries or read the data. Treat the URL like a secret, use a dedicated personal sheet, and do not store sensitive information. For stronger privacy, host the UI inside Apps Script HTML Service (using `google.script.run`) or add a proper authenticated backend before storing sensitive data.

## B. Connect the app
1. Open `index.html` in a browser, or publish it with GitHub Pages (steps below).
2. At the bottom of the app, tap **Local mode · tap this status line to configure Google Sheets sync**.
3. Paste the `/exec` URL and confirm. The app loads foods, entries and goals using a JSONP read. New/edited/deleted entries are submitted as form-encoded POSTs.
4. The UI saves locally immediately. The `no-cors` POST is fire-and-forget, so the browser cannot verify the server response; if a sync fails, check the Apps Script execution log and sheet. The app reads the server data on initial load. Do not assume an entry has reached the sheet until you verify it there.

## C. Publish on GitHub Pages
1. Create a GitHub repository (e.g. `daily-fuel`).
2. Upload `index.html` to the repository root.
3. Open **Settings → Pages**.
4. Under Build and deployment choose **Deploy from a branch**, branch `main`, folder `/(root)`, then Save.
5. Open the published Pages URL. Configure the Apps Script URL once in that browser.
6. On iPhone Safari, use **Share → Add to Home Screen** for quick access.

## Local-only mode
The app works without Google Sheets using browser local storage. Entries remain on that browser/device; export CSV periodically as a backup. Clearing browser site data can remove local entries.

## Sheet schema
- `FoodLog`: id, date, time, meal, food, quantity, servings, calories, protein, notes, createdAt, updatedAt
- `FoodDatabase`: name, serving, calories, protein, meal, category
- `DailySummary`: date, calories, protein, entries, calorieGoal, proteinGoal, calorieGoalHit, proteinGoalHit, bothGoalsHit
- `Settings`: key, value

## Limitations in this first version
- Nutrition estimates are approximate and depend on portions, cooking oil and recipes.
- The app supports editing stored calorie/protein values. The servings multiplier fills a suggested value from the database but does not silently overwrite manually edited values after you save.
- No automatic AI nutrition estimate for unknown foods; add a food to `FoodDatabase` or enter calories/protein manually.
- The API deployment is not configured by this project; you must create your own Sheet and deploy Apps Script.
