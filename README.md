# Minimal Food Log

A lightweight, mobile-friendly food diary for GitHub Pages. No build step, dependencies, account, or backend.

## Features
- Log food, approximate amount, time, meal category, and optional notes
- Browse entries by date; edit and delete entries
- Saves to the current browser/device using `localStorage`
- Export all entries as CSV for Google Sheets

## Publish with GitHub Pages
1. Create a GitHub repository, for example `food-log`.
2. Upload `index.html` and this README to the repository root.
3. In the repository, open **Settings → Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/(root)`, then Save.
5. Wait for the Pages deployment. The site URL will be shown under Settings → Pages.

## Use with Google Sheets
1. Open the app and choose **Export CSV**.
2. In Google Sheets, choose **File → Import → Upload** and upload the CSV.
3. Choose a new sheet or replace the current sheet as desired.

## Important storage note
Entries stay in the browser and device where you entered them. They do not automatically sync between devices or browsers. Export CSV backups periodically. Clearing browser site data may remove the saved entries.
