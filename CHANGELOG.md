# Changelog

All notable changes to DataRove are listed here. Versions follow [Semantic Versioning](https://semver.org/): `MAJOR.MINOR.PATCH`. Anything that affects your data or saved settings is called out under **Important**.

Each release on the [Releases](../../releases) page has installers named `DataRove-<version>-<os>-<arch>.<ext>` (macOS `.dmg` / `.zip`, Windows `.exe`, Linux `.AppImage` / `.deb`, Android `.apk`) and a `SHA256SUMS.txt`. The desktop and Android apps check this repository for updates and verify the checksum before installing.

## [1.0.0] — 2026-10-30

### Updates
- DataRove now updates itself. It checks about once an hour (and when you come back to the window or the computer wakes), downloads a new version in the background and installs it when you restart. The banner at the top and **Settings → About** show the download progress, and you can cancel a download at any time. A cancelled version is not downloaded again by itself.
- Windows installs the update silently when DataRove quits, into the same folder. macOS and AppImage swap the app in place.
- Android downloads the update in the app on Wi-Fi, checks its SHA-256 and asks you to tap **Install**. On mobile data it only tells you.
- Turn it off in **Settings → About → Automatic updates** or the tray menu.
- Upgrading from 0.1.x: those versions still check the old way (every few hours, or **Check for updates…** in the tray menu). Install 1.0.0 once and later versions arrive by themselves.

### Feedback
- **Send feedback** in Settings → About, the Help menu and the tray menu: describe a problem or idea, add screenshots (capture the window, pick, paste or drop images) and an optional reply email. Version, OS and run mode can be attached; connections, paths and data never are.

### Data
- Data compare: the Compare page has a **Schema / Data** switch. Data mode compares tables with the same name row by row by primary key, lists missing, different and extra rows (changed columns only, old → new) and writes the `INSERT` / `UPDATE` (and, if you tick it, `DELETE`) that makes the target match. Read-only on both sides.
- SQL Server: **Explain** (⌘E) shows the estimated plan with row counts, costs, indexes and hints (scans with filters, key lookups, big sorts, implicit conversions, missing indexes). The statement is not run.
- SQL editor: **F2** (or ⇧F6) renames an alias, table or CTE everywhere in the statement.
- Tables and query results: right-click **Copy as INSERT** / **Copy as UPDATE**.
- MariaDB and TiDB were run against real servers: safety snapshots, backups, restore and sync work, and read-only connections work on TiDB.

### Fixes
- Dialogs no longer scroll away as a whole.

### Important
- The terms and privacy notice (version 2026-10-30.3) now cover feedback (kept for a year) and say that update checks go through data.runrove.com first. DataRove asks you to accept them again the first time you open this version.
