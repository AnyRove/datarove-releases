<p align="center"><img src="docs/images/icon.svg" width="96" alt="DataRove"></p>

<h1 align="center">DataRove</h1>

<p align="center"><b>Manage every database from your computer — and check on it from your phone, end-to-end encrypted.</b></p>

<p align="center">
<b>English</b> · <a href="README.zh-CN.md">简体中文</a> · <a href="README.zh-TW.md">繁體中文</a> · <a href="README.ja.md">日本語</a> · <a href="README.ko.md">한국어</a> · <a href="README.es.md">Español</a> · <a href="README.fr.md">Français</a> · <a href="README.de.md">Deutsch</a> · <a href="README.pt-BR.md">Português</a> · <a href="README.ru.md">Русский</a>
</p>

---

DataRove is a desktop database client with scheduled backups and sync built in. It runs on your computer (as an app or a background service), and your phone connects to it over the local network or through a relay. The connection is end-to-end encrypted, and database passwords never leave the computer.

**Databases:** MySQL · MariaDB · PostgreSQL · SQLite · SQL Server · MongoDB · Redis, with SSH tunnels and TLS.

> **Status:** 1.0.0. Download the installers from the [Releases](../../releases) page; the apps update themselves after that. Watch this repository to be notified of new versions. See [CHANGELOG](CHANGELOG.md).

## Watch the 4-minute tour

<p align="center"><a href="docs/video/DataRove-promo.mp4"><img src="docs/video/DataRove-promo-poster.jpg" width="720" alt="DataRove feature tour (video, English narration, English + Chinese subtitles)"></a></p>
<p align="center"><a href="docs/video/DataRove-promo.mp4">Play the video</a> (MP4, 1080p, English narration with English and Chinese subtitles) · subtitles as SRT: <a href="docs/video/DataRove-promo.en.srt">English</a>, <a href="docs/video/DataRove-promo.zh-CN.srt">简体中文</a></p>

![Overview](docs/images/overview.png)

## Features

### Browse and edit data
- Fast virtual-scrolling grid with filters, sorting and paging. Edit cells in place, or open a whole row as a form with an input for each column type (boolean, enum, date, JSON, NULL / default).
- Sort the table list by name, size or row count; each database shows how its space is split between tables.
- Foreign-key columns let you search the referenced table and pick a row.
- Preview the exact SQL before saving. All changes are committed in one transaction and rolled back together on error. Deletes and production connections ask for confirmation first.

![Data grid](docs/images/data.png)

### Write queries
- SQL editor with context-aware completion: tables after `FROM` / `JOIN`, columns after `alias.`, `INSERT` column lists, functions with signatures, statement templates.
- Hover a table or column to see its structure, types and comments. Wrong table names, wrong `alias.column` and `DELETE` / `UPDATE` without `WHERE` are underlined before you run them.
- Turn results into a bar, line or pie chart. Read the execution plan as a tree, with full table scans called out in plain words.
- Results of a one-table `SELECT` that includes the primary key can be edited and saved directly.
- MongoDB shell syntax (`db.users.find({...})`) and a Redis command line, both with completion.

![Query editor](docs/images/query.png)

### Understand the schema
- Databases → tables / views / collections → columns, indexes, foreign keys, triggers, DDL, row counts and size.
- ER diagram, schema diff between two databases with a generated migration script (drops are commented out), Redis key browser, server monitor (connections, transactions, cache hit rate, sessions).

![ER diagram](docs/images/er.png)

### Import and export
- Export a table or a filtered result as CSV, TSV, JSON, JSON Lines, SQL `INSERT` or Markdown; import CSV / TSV / JSON / JSON Lines (UTF-8 and GBK are both recognized).
- Large transfers run in the background on the computer with live progress and a cancel button. Files move in small pieces, so a dropped connection resumes instead of starting over.
- An import runs in one transaction: if a row fails, nothing is written and the failing row is named.

### Backups you can trust
- Back up to files with native tools (mysqldump / pg_dump / mongodump) or the built-in exporter, gzip and AES-256-GCM encryption, keep the last N files or N days. Missing tools can be installed from the app in one click.
- Each new backup file is read back right after it is written (size, SHA-256, decrypt, decompress, parse), so a broken file is caught on the night it was made. Any file can be re-checked later without touching a database.
- Sync to another database, also across database types (for example PostgreSQL → MySQL): full replace, truncate-and-load, append, upsert by primary key, or **incremental** by a watermark column. Each table is written completely or not at all, and row counts are compared before it is committed.
- Temporary network errors are retried automatically. If some tables still fail, **retry only the failed parts**.
- Schedules: every N minutes, daily, weekly, monthly or cron. Missed runs catch up at startup. The computer stays awake while a job runs. Desktop, system and webhook notifications. Restore any backup to any connection.

![Backups](docs/images/backups.png)

### Undo mistakes
- On connections marked *production* (or all writable connections, if you choose), DataRove saves the affected tables before an `UPDATE`, `DELETE`, `DROP`, `TRUNCATE`, grid edit, overwriting import, overwriting sync, restore or Redis change.
- Undo it later in one click: only the rows that differ are changed back, in one transaction, and the state before the undo is saved too. Snapshots are encrypted on the computer and cleaned up by age and size.

### Your phone, safely
- The same interface on your phone (Android app or any modern mobile browser). Scan a QR code to pair, no account needed.
- Each phone gets a role (read-only, read-write or admin) that you can change or revoke at any time. Temporary grants expire on their own, and an activity log shows what each phone did.
- On the same Wi-Fi the phone connects directly; elsewhere it goes through a relay that only forwards ciphertext.

<p align="center"><img src="docs/images/phone-data.png" width="300" alt="DataRove on a phone"></p>

### Desktop
Stays in the tray, starts at login, and can install itself as a background service (launchd / systemd / Windows Task Scheduler) so scheduled backups keep running with the window closed. Command palette for every table and command. 10 themes, light and dark, 15 interface languages.

## Security in short

- Saved passwords are encrypted on the computer with a local master key. The interface and paired phones only ever see `••••`.
- Pairing uses a 256-bit secret from the QR code, or a short code hardened with scrypt when typed by hand. Both sides authenticate each other's keys, so a relay cannot slip in a device of its own.
- Every session uses X25519 + ChaCha20-Poly1305 with fresh ephemeral keys (forward secrecy). Tampered, replayed or reordered frames are rejected, and downgrade attempts are refused.
- A public relay must use `wss://`. You can run your own relay, and pairing can happen on the local network only.
- Everything outside DataRove itself is restricted to the computer: tool paths, file browsing, SQLite file access, and sending a saved password to a changed host.

To report a vulnerability, see [SECURITY.md](SECURITY.md).

## Platforms

| | |
|---|---|
| macOS | Apple silicon and Intel |
| Windows | 10 / 11, x64 |
| Linux | AppImage, deb |
| Android | App (companion to a computer, or phone direct mode) |
| Other phones | Any modern mobile browser (installable as a web app) |

## Install

Download the file for your system from the [latest release](../../releases/latest). Every release lists SHA-256 checksums in `SHA256SUMS.txt`. The installers are not signed with an Apple or Microsoft developer certificate yet, so the system asks once:

- **macOS**: open the `.dmg` and drag DataRove to Applications. The first time you open it, macOS says it can't verify the app: open **System Settings → Privacy & Security**, scroll down and click **Open Anyway**. If macOS says the app is damaged, run `xattr -cr /Applications/DataRove.app` in Terminal and open it again.
- **Windows**: run the `.exe`. If SmartScreen appears, click **More info → Run anyway**.
- **Linux**: make the `.AppImage` executable and run it, or install the `.deb`.
- **Android**: open the `.apk` and allow installing from your browser or file manager. The app pairs with DataRove on your computer, or connects to your databases itself in phone direct mode (activate the phone once with your DataRove account).

Then sign in with your email in the app and activate the computer. DataRove is free; no payment details are needed.

## License, disclaimer and versions

- DataRove is free right now, with every feature. No payment details are needed.
- DataRove is proprietary software, not open source. Use of the app is governed by the End User License Agreement, Privacy Notice and Purchase Terms shown in the app at first start, which you accept there. See [LICENSE.md](LICENSE.md).
- DataRove changes real databases. Keep independent, tested backups and read the [DISCLAIMER](DISCLAIMER.md).
- Versions follow [Semantic Versioning](https://semver.org/). Every release lists its changes in [CHANGELOG.md](CHANGELOG.md) and ships a `SHA256SUMS.txt`; the apps check for updates here and verify the checksum before installing.

## Feedback

Bug reports and feature requests are welcome in [Issues](../../issues). This repository holds release notes, installers and documentation; the source code is not public.
