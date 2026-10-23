# Project Timeline

A mobile-friendly project timeline tool that runs on **your own Google account**. Keep your WBS, schedule, dependencies and Gantt chart in a Google spreadsheet, and edit them from your phone or computer through a small Google Apps Script web app.

- Your data stays in your own Google Drive and spreadsheets. Nothing goes through a third party.
- The web app runs with your own permissions and counts against your own Google quota.
- No server, no build step, no dependencies: two files (`PT_Backend.gs` and `PT_Frontend.html`).

## Features

- **Task list with a WBS tree**: parents and sub-items (`1`, `1.2`, `1.2.1`), drag to reorder, renumber automatically.
- **Drag to add**: drag the pill to where the new task should go (before, after, or as a sub-item).
- **Undo / redo** for the last 20 steps (edit, delete, add, apply order). Ctrl+Z / Ctrl+Shift+Z on desktop.
- **Background saving**: the screen updates immediately and writes to the sheet in the background.
- **Gantt chart** in week, month or quarter view, with a button to write the colored bars back to the sheet.
- **Scheduling tools** (⚙️):
  - roll up parent dates and budgets from sub-items
  - slack calculation (critical path)
  - tighten successors, pull predecessors back, align sync start
- **Dependencies**: `Predecessor`, `Sync Start` and `Sync End`, with ± day offsets (`2.2`, `2.2-7`, `2.2,2.3`).
- **Read-only mode** when you only have view access to a spreadsheet.
- **Read-only preview** of sheets that do not follow the timeline format.
- Remembers where you were (spreadsheet, sheet, view, scroll position) after a refresh.

## Quick start

### Option A: try the shared demo (about 2 minutes)

1. Open the template spreadsheet: **[make a copy](https://docs.google.com/spreadsheets/d/1liTi3iFLfwtTE6b3AdzdOxn1Sw7JETcm9Ns8h1rb1IM/copy)**.
2. Open the demo web app: **[demo](https://script.google.com/macros/s/AKfycbynAiJZMFzy7tGOO-H7UVKkicPCKTSQ__lvE7bd8110BvhzY9KWG2bI-insThEqaw4BsA/exec)**. The first time, Google asks you to authorize it. "Google hasn't verified this app" is expected; click **Advanced → Go to … (unsafe) → Allow**.
3. In the tool, tap **+Add**, paste the URL of your copy, and open the "Clinical Trial Prep Example" or "Demo Project" sheet.

The demo runs as *you*, so it can only see the spreadsheets you add. It saves a small file named `ProjectTimeline_config.json` in your Drive root to remember your spreadsheet list.

### Option B: run your own copy (about 10 minutes, once)

1. Open the template and choose **File → Make a copy**: **[make a copy](https://docs.google.com/spreadsheets/d/1liTi3iFLfwtTE6b3AdzdOxn1Sw7JETcm9Ns8h1rb1IM/copy)**. The code is bound to the spreadsheet, so it is copied too.
2. In your copy, choose **Extensions → Apps Script**. You should see `PT_Backend.gs` and `PT_Frontend.html`.
3. Click **Deploy → New deployment → Web app**:
   - Execute as: **Me**
   - Who has access: **Only myself**
4. Authorize when asked (**Advanced → Go to … (unsafe) → Allow**). This is your own code.
5. Open the **Web app URL** you get. Bookmark it; on a phone, use **Add to Home screen**.
6. Tap **+Add** and paste the URL of your spreadsheet.

### Installing from this repository instead

Create a new Apps Script project (or open **Extensions → Apps Script** in any spreadsheet), add a script file `PT_Backend.gs` and an HTML file `PT_Frontend.html` with the contents from this repository, then deploy as in Option B. The file names must match exactly.

## Sheet format

A sheet works as a timeline when it follows this layout:

| Row | Content |
| --- | --- |
| 1 | Year labels / hidden exact dates for the Gantt chart |
| 2 | Header row. Must contain `WBS` and `Task Name` |
| 3 and below | Data |

| Column | Field | Notes |
| --- | --- | --- |
| A | WBS | `1`, `1.2`, `1.2.1`. Stored as plain text |
| B | Task Name | |
| C, D | Start Date, End Date | |
| E | Duration (days) | |
| F | Predecessor ±days | `1.2`, `1.2+3` (3 days later), `1.2-2` (2 days earlier). Separate several with commas |
| G | Sync Start ±days | Start together with another task |
| H | Sync End ±days | Finish together with another task |
| I | Budget | |
| J | Slack | Written by the slack calculation |
| K | Milestone | Enter `Yes` |
| L | Owner | |
| M | Deliverable | |
| N | Status | Free text |
| O | Blocker | |
| P | Risk Level | |
| Q | Mitigation | |
| R–T | Reserved | |
| U onward | Gantt bars | Written by "Update sheet" |

Parent items (such as `1`, `2`, `3`) get their dates and budgets from their sub-items when you run **Roll up parent dates / budget**. Sheets with the legacy Chinese headers (`編號`, `工作項目`) are also recognized.

## Sharing safely

- Keep one private master. Share it only as view access with a `/copy` link (replace `/edit` at the end of the URL with `/copy`), so people get their own copy and cannot change yours.
- Do **not** deploy a shared web app that executes as *you* and is open to everyone: every visitor would then act with your permissions and share your config file. Use **Execute as: User accessing the web app**, or have everyone deploy their own copy (Option B).
- Test the copy link with a second Google account that has no edit rights before announcing it.

## Updating

Your copy keeps the code it had when you made it. To update, paste the new `PT_Backend.gs` and `PT_Frontend.html` into **Extensions → Apps Script**, then **Deploy → Manage deployments → edit → New version → Deploy**. Your spreadsheet data is not affected.

The two files carry a version number each (`PT_VERSION` and `APP_VERSION`). If they differ, the tool shows a red banner: update both files and redeploy.

## Versioning

`Major.Minor.Patch`, for example `1.2.0`:

- Major `X.0.0`: big change to architecture or data format
- Minor `1.X.0`: new feature or noticeable UX change
- Patch `1.0.X`: bug fix or small tweak

## Limitations

- Apps Script has daily quotas and execution time limits; very large sheets load in two stages.
- Web apps that ask for Drive and Sheets scopes show Google's "unverified app" warning, and unverified apps have a user limit. Fine for small groups.
- Dates are calendar days (weekends and holidays are not skipped).

## License

Add your license here (for example MIT).
