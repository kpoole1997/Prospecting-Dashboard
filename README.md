[README.md](https://github.com/user-attachments/files/34490936/README.md)
# Prospect Board

A single-file prospecting dashboard for tracking companies and the contacts at each one. It works like a lightweight Monday.com-style board: groups, status labels, kanban, bulk editing, rich-text updates with file attachments, and undo history. There is no server, no account, and no subscription.

Everything runs in the browser. Your data is stored on your own computer and never sent anywhere.

---

## Contents

- [Quick start](#quick-start)
- [Hosting on GitHub Pages](#hosting-on-github-pages)
- [Where your data lives](#where-your-data-lives)
- [Backups](#backups)
- [Features](#features)
- [Importing and exporting](#importing-and-exporting)
- [Keyboard shortcuts](#keyboard-shortcuts)
- [Browser support](#browser-support)
- [Privacy and security](#privacy-and-security)
- [Troubleshooting](#troubleshooting)
- [Repository layout](#repository-layout)

---

## Quick start

1. Open `index.html` in Chrome or Edge, either from your computer or from the hosted address.
2. The app starts with a sample "Prospecting Board" so you can see how it works. Delete the sample rows, or create a new board with **+ New board**.
3. Add a company in a group with **+ Add company**. Click the arrow beside a company to add its contacts.
4. Set up an automatic backup first (see [Backups](#backups)) so your work is protected.

> The main file is named `prospect-board.html` in this project. When publishing with GitHub Pages, upload it as **`index.html`** so the site opens at its address.

---

## Hosting on GitHub Pages

1. Put the HTML file in the repository root, named `index.html`.
2. Go to **Settings > Pages**, choose the `main` branch and the root folder, and save.
3. After the deploy finishes, open the address GitHub gives you.

To update the app later, replace `index.html` and commit.

**Important**

- The repository holds only the application code. **Never commit exported data**: the CSV files, backup `.json` files, or attachments contain real contact information. Add them to `.gitignore` (see [Repository layout](#repository-layout)). If the repository is public, anyone can read whatever you commit.
- Browsers store data **per web address**. Keep using the same address, or back up and restore when it changes (see [Moving to a new address or computer](#moving-to-a-new-address-or-computer)).
- GitHub Pages cannot password-protect a site. That is fine here because the page contains no data, but it means anyone with the link can load the empty app.
- Deploys can occasionally stall on GitHub's side. If one hangs, check the **Actions** tab ("pages build and deployment") and githubstatus.com.

---

## Where your data lives

| What | Where | Notes |
|---|---|---|
| Boards, groups, companies, contacts, columns, updates, settings | Browser `localStorage`, key `prospect-board-v1` | One JSON document |
| Attached files | Browser IndexedDB, database `prospect-board-files` | Files are not stored inside the JSON |
| Backup folder permission | Browser IndexedDB, database `prospect-board` | Used by the automatic backup |

Things that follow from this:

- Data does not sync between computers or browsers. Each browser profile has its own copy.
- Clearing "cookies and other site data" for the site erases it. Clearing only the cache does not.
- Some company-managed browsers clear site data when the browser closes. Ask your IT team if you are unsure.
- The app asks the browser for persistent storage when you attach a file, which reduces the chance of automatic clean-up on a low-disk computer.

---

## Backups

Because the data lives in the browser, **backups are essential**.

### Automatic backup to a folder (Chrome and Edge)

**Data > Automatic backup to a folder…**

1. Create a folder, ideally inside a synced location such as OneDrive (for example `ProspectBoardBackups`). Browsers do not allow selecting Documents or Desktop themselves, so use a subfolder.
2. Choose that folder once.
3. A few seconds after you make changes, the app writes:
   - `prospect-board-latest.json`: the current state, overwritten each time.
   - `prospect-board-YYYY-MM-DD.json`: one dated copy per day. The last 30 are kept by default (adjustable).
   - `attachments/`: a copy of every file attached to an update.
   - `prospect-board-before-shrink-….json`: only if a save would have far fewer rows than the file already in the folder, so a wiped browser cannot silently overwrite a good backup.

Notes:

- The folder is tied to the web address and browser. After restarting the browser it may ask you to re-approve access. Click **Reconnect** in the sidebar.
- The sidebar shows the time of the last backup and warns when something needs attention.
- **Restore latest** in that dialog reloads the newest backup from the folder, including attachments.

### Manual backups

Under **Data**:

| Option | What it does |
|---|---|
| Back up everything (JSON) | Downloads all data. **Does not include attached files.** |
| Back up with attachments (one file) | Downloads one JSON file containing the data and the files (up to about 150 MB total). |
| Restore from backup | Replaces everything in the app with the chosen backup file. |
| Manage attachments… | Shows storage used and removes unused files. |

### Moving to a new address or computer

1. In the old copy, run **Back up with attachments (one file)**, or rely on the automatic folder backup.
2. Open the app at the new address and use **Restore from backup**, or choose the same backup folder and accept the offer to restore.
3. Check a few companies, contacts and updates before deleting the old copy.

---

## Features

### Boards, groups, companies and contacts

- Multiple boards. Reorder them by dragging in the left sidebar or from the board menu (⋯). Hide or show the sidebar with the panel icon.
- Groups are colored and collapsible. Rename, recolor, delete and reorder them (drag the ⋮⋮ handle or use the ⋯ menu).
- Each **company** (top-level row) can have **contacts** (nested rows), each with their own columns. The two levels can be renamed in the board menu (for example "Item" and "Subitem").
- A progress bar under each group summarizes its first status column.

### Columns

- Types: **Text, Status, Date, Number, Link, Checkbox**.
- Status columns have editable, colored labels.
- Rename, delete, sort, hide, reorder (drag the header or use Move left/right) and resize (drag the header edge; double-click to reset).
- Company columns and contact columns are managed separately.

### Date roll-ups

A company date column (such as "Last outreach") can summarize the dates on its contacts: a range, the latest date, or the earliest date. Companies with no contact dates keep their own editable date. Set it from the column header menu: **Roll up from contacts…**.

### Views

- **Table**: the main view. Group by any status column (drag rows between the groups to change the value).
- **Kanban**: cards grouped by a status column. Drag cards to change status. Clicking a card opens its details.
- **Search** covers names, values, update text and attachment names.

### Details and updates panel

Click the speech-bubble icon on a company or contact (or a Kanban card) to open a side panel:

- **Details**: every column as an editable field, including long multi-line notes.
- **Updates**: a feed of dated notes with the editor described below.

The board also has its own updates feed (the **Updates** button in the toolbar).

### Updates: rich text and attachments

- Formatting: bold, italic, underline, bulleted and numbered lists, text color, text size, and clear formatting. Links are made clickable automatically.
- Attach files with the 📎 button, by dragging them onto the editor, or by pasting a screenshot. Up to 25 MB per file.
- Images show as thumbnails with a larger view. PDFs and text files open in a new tab. Other files download.
- Edit, delete (with Undo), like, and reply to updates. Replies are plain text.
- Files are stored in the browser (see [Where your data lives](#where-your-data-lives)). Include them in backups using the options above.

### Selecting and bulk editing

- Row checkboxes with shift-click range selection. Group checkboxes select a whole group, and shift-click selects a range of groups.
- A bar appears with **Move to…**, **Duplicate**, **Delete** and **Clear**. Selected rows can also be dragged together onto a group.
- **Multi-edit:** with several rows selected, change a status, date, text or link on any selected row and the change applies to all of them. A message appears with **Undo**.
- Contacts can be selected the same way, with **Move to company…** and **Delete**.

### History and undo

The clock icon opens a list of recent changes in plain language ("Edited 'Status' on …", "Deleted 2 companies"). Click any step to go back to it, or use **Undo** and **Redo**. History keeps the last 60 steps and is cleared when you reload the page or restore a backup. Use the backups for longer-term recovery.

### Everything else

- **Night mode** toggle (follows your system until you choose).
- **Hover cards**: hover over a truncated text cell to read all of it.
- **Keyboard navigation** across the table (see below).
- **Hide columns**, **Group by**, and sorting from the toolbar and column menus.

---

## Importing and exporting

**Data menu**

| Action | Format |
|---|---|
| Export companies | CSV |
| Export contacts | CSV |
| Export updates | CSV |
| Import CSV | Companies or contacts |

### CSV import rules

The importer matches columns **by name** (case-insensitive). Any column it does not recognise becomes a new text column. Status labels that do not exist yet are created automatically. Dates should be `YYYY-MM-DD` (other formats are converted when possible).

**Companies file** (a `Group` column is optional)

```csv
Group,Company,Status,Last outreach,Notes,Website
Prospects,Example Co,Outreached,2026-10-20,Left a voicemail,https://example.com
```

**Contacts file**: a `Contact` (or `Subitem`) column, or whatever you renamed the contact level to, marks it as contacts. Each contact is attached to the company with the matching name. Import the companies file first.

```csv
Group,Company,Contact,Title,Status,Last outreach,Phone number,Email
Prospects,Example Co,Jane Doe,Director,Working on it,2026-10-20,555-0100,jane@example.com
```

Company names that appear more than once all receive the contacts of the first match, so make company names unique before importing contacts.

---

## Keyboard shortcuts

The ⌨ icon in the toolbar shows these in the app.

| Keys | Action |
|---|---|
| ← ↑ ↓ → | Move between cells. In a text cell, ← and → move the cursor first, then jump to the next cell at the edge. |
| Enter | Finish editing and move down. On a status cell, opens the label list. |
| Space | Tick a checkbox or open a status list. |
| ↑ ↓ then Enter | Choose a label in an open status list. Esc closes it. |
| Alt + ↓ | Open the calendar in a date cell (you can also type the date). |
| Ctrl/Cmd + Z, Ctrl/Cmd + Y | Undo and redo, when not typing in a cell. |
| Ctrl/Cmd + Enter | Post an update or reply. |
| Esc | Close a menu, or clear the row selection. |

---

## Browser support

- **Chrome and Edge** (current versions): everything works, including the automatic folder backup.
- **Firefox and Safari**: the app uses standard browser features and should work, but it has only been tested in Chrome. The automatic folder backup is not available there, so use the manual backup options.
- Attachments need a regular browser tab with storage enabled (private/incognito windows may discard data when closed).

---

## Privacy and security

- The page makes **no network requests** and loads no outside scripts, fonts or analytics. Everything is in the one HTML file.
- Data stays in your browser and in any backup folder you choose.
- Rich text in updates is cleaned before it is saved and again before it is shown. Scripts, images, frames, forms and event handlers are stripped, and links are limited to `http`, `https` and `mailto`.
- If you use this with business or customer data, make sure storing it this way fits your organisation's policies, and keep exported files out of public repositories.

---

## Troubleshooting

| Symptom | What to try |
|---|---|
| The page shows only the sample data | You are at a different web address or browser profile than before. Restore from a backup. |
| Data disappeared after clearing the browser | Restore from the backup folder or a downloaded backup. |
| "Backup folder needs permission" | Click **Reconnect** in the sidebar and approve access. |
| An attachment is greyed out | The file is not stored on this computer. Restore a backup that includes attachments. |
| The GitHub Pages deploy seems stuck | Check **Actions > pages build and deployment**, cancel a stuck run, and push a new commit. Resetting the Pages setting off and on can also clear a stuck deployment. |
| An old version still appears after deploying | Hard-refresh with Ctrl+Shift+R. |
| Arrow keys will not leave a date cell | Date cells use the arrows to change the date segments. Click or tab out, or type the date. |

---

## Repository layout

```
index.html      The whole application (HTML, CSS and JavaScript in one file)
README.md       This file
.gitignore      Keeps data and backups out of the repository
```

Suggested `.gitignore`:

```gitignore
# Never commit exported data or backups
*.csv
*.json
attachments/
prospect-board-*
```

---

## License

Add a license of your choice here if you plan to share the code.
