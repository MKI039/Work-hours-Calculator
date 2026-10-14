# Clock-in Balance

A small, single-file web app that tracks your **extra or deficit work time** against a 9-hour day. Enter hours (or in/out times) by hand, or import your company's biometric `.xlsx` report. Everything stays in your browser. There is no server and no account.

## What it does

- Compares each day with **9h 00m** and keeps a running balance. Extra time cancels out deficit time, so 9h 32m on Monday and 8h 45m on Thursday gives **17m extra**.
- Shows the latest 3 days by default, newest first, with an expand bar for the full list. The **Buffer** column is the cumulative balance up to each day.
- Weekends are greyed out in the calendar and are never counted.
- Light and dark mode button (starts in light mode on a new device and remembers your choice).
- Works on phones and desktops.

## Day types

| Type | Counts toward balance? | Notes |
|---|---|---|
| Work | Yes | Entered as hours and minutes, or as In and Out times (AM/PM button, colon added automatically). |
| In progress | No | Saved when you enter only an In time. Edit it later to add the Out time. |
| Leave | No | Optional note (for example "ML") when imported. |
| Holiday | No | |
| LOP | No | Loss of pay day, tracked and shown separately. |

A date can hold either one work entry or one leave/holiday/LOP mark, not both.

## Importing the biometric report (.xlsx)

Use **Import report** and choose the company sheet. The app reads it itself, with no third-party spreadsheet library.

- The report month and year come from the title (for example "Biometric October - 2026 Report"), because dates like `21-Sep` have no year.
- Columns are found by header name: Date, In Time, Out Time, Worked Hours, Status.
- The name and registration number row is ignored and never stored.
- The file itself is not stored, only the days from it.

| Status in the sheet | Imported as |
|---|---|
| Present with worked hours | Work (uses Worked Hours, keeps in/out times) |
| Present or blank, with the same in and out time | In progress (not counted yet) |
| ML and other leave codes | Leave |
| Holiday | Holiday |
| ULOP and other LOP codes | LOP |
| Week-Off | Skipped |
| No punches | Skipped |
| Unrecognised status | Skipped and listed in the message |

Days after the in-progress day (or after today, if there is none) are left out until you import again. Re-importing an updated report replaces the days from the earlier import. Days you entered by hand are never overwritten.

**Deleting imported files:** each import appears in the Biometric report card with its date range and day count. **Delete** (confirmed with a second tap) removes the file record and all its days, and leaves your manual entries alone.

Limits: `.xlsx` only, up to 5 MB and 2,000 rows. Dates and times in the sheet must be stored as text, like the sample.

## Backup and restore

- **Export data** saves a JSON backup. If a download isn't available, the text is shown so you can copy it.
- **Import data** or **Paste data instead** restores a backup. Importing adds to your current days and skips duplicates. To replace everything, use **Clear all days** first.
- Backups are limited to 2 MB and 5,000 days.

## Privacy and security

- Data is saved in your browser's local storage on each device. Nothing is sent anywhere. The only network request is Google Fonts for the typeface.
- Imported backups, reports and stored data are validated and escaped before being shown, so a crafted file can't inject script.
- A warning appears if the browser can't save (storage full or blocked).
- Clearing browser data erases your days, so export a backup now and then. Each device and browser keeps its own data.

## Browser support

Any recent browser. Reading `.xlsx` files needs Chrome 80+, Safari 16.4+ or Firefox 113+.

## Not built yet

- **Pay cycle rule:** the month for pay runs from the 21st to the 20th, leftover buffer resets to 0 after it, and any deficit becomes LOP. The balance currently adds up all days together.
- Reminders were intentionally skipped. A web page can only notify you while it's open.

## Files

```
index.html   the whole app (HTML, CSS and JavaScript in one file)
README.md    this file
```

## Run on GitHub Pages

1. Create a new **public** repository on GitHub.
2. Upload `index.html` (and `README.md`) to the root: **Add file → Upload files → Commit changes**.
3. Open **Settings → Pages**. Under **Build and deployment**, set **Source** to **Deploy from a branch**, choose **main** and **/ (root)**, then **Save**.
4. Wait a minute or two. Your app is live at `https://<your-username>.github.io/<repository-name>/`.
5. Optional on a phone: open the link and use **Add to Home Screen**.
