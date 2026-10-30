<p align="center"><img src="docs/images/icon.svg" width="96" alt="DataRove"></p>

<h1 align="center">DataRove</h1>

<p align="center"><b>Alle Datenbanken am Computer verwalten – und unterwegs vom Handy aus prüfen, Ende-zu-Ende-verschlüsselt.</b></p>

<p align="center">
<a href="README.md">English</a> · <a href="README.zh-CN.md">简体中文</a> · <a href="README.zh-TW.md">繁體中文</a> · <a href="README.ja.md">日本語</a> · <a href="README.ko.md">한국어</a> · <a href="README.es.md">Español</a> · <a href="README.fr.md">Français</a> · <b>Deutsch</b> · <a href="README.pt-BR.md">Português</a> · <a href="README.ru.md">Русский</a>
</p>

---

DataRove ist ein Datenbank-Client für den Desktop mit eingebauten, geplanten Backups und Synchronisierung. Er läuft auf deinem Computer (als App oder Hintergrunddienst), und das Handy verbindet sich direkt im lokalen Netz oder über ein Relay. Die Verbindung ist Ende-zu-Ende-verschlüsselt, und Datenbankpasswörter verlassen den Computer nie.

**Datenbanken:** MySQL · MariaDB · PostgreSQL · SQLite · SQL Server · MongoDB · Redis, mit SSH-Tunneln und TLS.

> **Status:** 1.0.0. Die Installer gibt es auf der Seite [Releases](../../releases); danach aktualisieren sich die Apps selbst. Mit „Watch“ auf diesem Repository wirst du über neue Versionen benachrichtigt. Änderungen im [CHANGELOG](CHANGELOG.md).

## 4-Minuten-Tour als Video

<p align="center"><a href="docs/video/DataRove-promo.mp4"><img src="docs/video/DataRove-promo-poster.jpg" width="720" alt="DataRove-Tour (Video, englischer Sprecher, englische und chinesische Untertitel)"></a></p>
<p align="center"><a href="docs/video/DataRove-promo.mp4">Video abspielen</a> (MP4, 1080p, englischer Sprecher mit englischen und chinesischen Untertiteln) · Untertitel als SRT: <a href="docs/video/DataRove-promo.en.srt">English</a>, <a href="docs/video/DataRove-promo.zh-CN.srt">简体中文</a></p>

![Übersicht](docs/images/overview.png)

## Funktionen

### Daten ansehen und bearbeiten
- Schnelles Raster mit virtuellem Scrollen, Filtern, Sortieren und Blättern. Zellen direkt bearbeiten oder eine ganze Zeile als Formular öffnen, mit passendem Eingabefeld je Spaltentyp (Boolean, Enum, Datum, JSON, NULL / Standardwert).
- Bei Fremdschlüsselspalten lässt sich die referenzierte Tabelle durchsuchen und eine Zeile auswählen.
- Vor dem Speichern das genaue SQL ansehen. Alle Änderungen werden in einer Transaktion festgeschrieben und bei einem Fehler gemeinsam zurückgerollt. Löschen und Produktionsverbindungen fragen vorher nach.
- Kopieren als CSV / TSV / JSON / SQL / Markdown, Export in Dateien, Import von CSV / JSON.

![Datenraster](docs/images/data.png)

### Abfragen schreiben
- SQL-Editor mit kontextbezogener Vervollständigung: Tabellen nach `FROM` / `JOIN`, Spalten nach `alias.`, Spaltenlisten bei `INSERT`, Funktionen mit Signatur, Vorlagen für Anweisungen.
- Fährt man über eine Tabelle oder Spalte, erscheinen Struktur, Typen und Kommentare. Falsche Tabellennamen, falsche `alias.spalte` und `DELETE` / `UPDATE` ohne `WHERE` werden vor dem Ausführen unterstrichen.
- Ergebnisse eines `SELECT` auf eine Tabelle, das den Primärschlüssel enthält, lassen sich direkt bearbeiten und speichern.
- MongoDB-Shell-Syntax (`db.users.find({...})`) und eine Redis-Befehlszeile, beide mit Vervollständigung.

![Abfrage-Editor](docs/images/query.png)

### Das Schema verstehen
- Datenbanken → Tabellen / Views / Collections → Spalten, Indizes, Fremdschlüssel, Trigger, DDL, Zeilenzahl und Größe.
- ER-Diagramm, Schemavergleich zweier Datenbanken mit erzeugtem Migrations-SQL, Redis-Schlüsselbrowser, Servermonitor (Verbindungen, QPS, langsame Abfragen).

![ER-Diagramm](docs/images/er.png)

### Import und Export
- Exportiere eine Tabelle oder ein gefiltertes Ergebnis als CSV, TSV, JSON, JSON Lines, SQL-`INSERT` oder Markdown; importiere CSV / TSV / JSON / JSON Lines (UTF-8 und GBK werden erkannt).
- Große Übertragungen laufen im Hintergrund auf dem Computer, mit Live-Fortschritt und Abbrechen. Dateien werden in kleinen Stücken übertragen, eine abgebrochene Verbindung setzt also fort, statt neu anzufangen.
- Ein Import läuft in einer Transaktion: Schlägt eine Zeile fehl, wird nichts geschrieben und die fehlerhafte Zeile genannt.

### Backups, auf die Verlass ist
- Backups in Dateien mit nativen Werkzeugen (mysqldump / pg_dump / mongodump) oder dem eingebauten Exporter, gzip und AES-256-GCM-Verschlüsselung, die letzten N Dateien oder N Tage behalten.
- Jede neue Backup-Datei wird direkt nach dem Schreiben zurückgelesen (Prüfsumme, Entschlüsseln, Parsen). Eine kaputte Datei fällt also noch in derselben Nacht auf.
- In eine andere Datenbank synchronisieren: komplett ersetzen, leeren und laden, anhängen, Upsert per Primärschlüssel oder **inkrementell** über eine Wasserstandsspalte (`updated_at`, Auto-Increment-ID …).
- Zeitpläne: alle N Minuten, täglich, wöchentlich, monatlich oder per Cron. Verpasste Läufe werden beim Start nachgeholt. Desktop- und Webhook-Benachrichtigungen. Jedes Backup mit einem Klick in jede Verbindung wiederherstellen.
- Synchronisieren geht auch zwischen Datenbanktypen (zum Beispiel PostgreSQL → MySQL). Jede Tabelle wird vollständig oder gar nicht geschrieben, und die Zeilenzahlen werden vor dem Commit verglichen. Vorübergehende Fehler werden automatisch wiederholt; schlägt danach noch etwas fehl, **wiederhole nur die fehlgeschlagenen Teile**. Fehlende Tools (pg_dump usw.) lassen sich mit einem Klick in der App installieren.

![Backups](docs/images/backups.png)

### Fehler rückgängig machen
- Auf Verbindungen, die als *Produktion* markiert sind, sichert DataRove die betroffenen Tabellen vor einem `UPDATE`, `DELETE`, `DROP`, `TRUNCATE`, einer Bearbeitung im Raster, einem überschreibenden Import, einer Synchronisierung oder Wiederherstellung.
- Später mit einem Klick rückgängig: Nur abweichende Zeilen werden zurückgesetzt, in einer Transaktion. Die Snapshots liegen verschlüsselt auf dem Computer.

### Das Handy, sicher
- Dieselbe Oberfläche auf dem Handy. Zum Koppeln einen QR-Code scannen, ohne Konto.
- Jedes Handy bekommt eine Rolle (nur lesen, lesen und schreiben oder Admin), die du jederzeit ändern oder entziehen kannst. Befristete Rechte laufen von selbst ab, und ein Aktivitätsprotokoll zeigt, was jedes Handy getan hat.
- Im selben WLAN verbindet sich das Handy direkt, sonst über ein Relay, das nur Geheimtext weiterleitet.

<p align="center"><img src="docs/images/phone-data.png" width="300" alt="DataRove auf dem Handy"></p>

### Desktop
Bleibt im Infobereich, startet bei der Anmeldung und kann sich als Hintergrunddienst installieren (launchd / systemd / Windows-Aufgabenplanung), damit geplante Backups auch bei geschlossenem Fenster laufen. 10 Designs, hell und dunkel, 15 Sprachen.

## Sicherheit in Kürze

- Gespeicherte Passwörter werden auf dem Computer mit einem lokalen Hauptschlüssel verschlüsselt. Die Oberfläche und gekoppelte Handys sehen nur `••••`.
- Beim Koppeln per QR-Code wird ein 256-Bit-Geheimnis verwendet; ein von Hand eingegebener kurzer Code wird mit scrypt gehärtet. Beide Seiten prüfen gegenseitig ihre Schlüssel, ein Relay kann also kein eigenes Gerät einschleusen.
- Jede Sitzung nutzt X25519 + ChaCha20-Poly1305 mit frischen kurzlebigen Schlüsseln (Forward Secrecy). Manipulierte, wiederholte oder umsortierte Frames werden abgewiesen, Downgrade-Versuche ebenfalls.
- Ein öffentliches Relay muss `wss://` verwenden. Du kannst ein eigenes Relay betreiben, und das Koppeln geht auch nur im lokalen Netz.
- Alles, was über DataRove hinausgeht, ist dem Computer selbst vorbehalten: Werkzeugpfade, Dateien durchsuchen, Zugriff auf SQLite-Dateien und das Senden eines gespeicherten Passworts an einen geänderten Host.

Sicherheitslücken meldest du wie in [SECURITY.md](SECURITY.md) beschrieben.

## Plattformen

| | |
|---|---|
| macOS | Apple Silicon und Intel |
| Windows | 10 / 11, x64 |
| Linux | AppImage, deb |
| Android | App (Begleiter eines Computers oder direkter Modus) |
| Handy | Jeder aktuelle mobile Browser (als Web-App installierbar) |

## Installation

Lade die Datei für dein System aus dem [neuesten Release](../../releases/latest) herunter. Jedes Release listet SHA-256-Prüfsummen in `SHA256SUMS.txt`. Die Installer sind noch nicht mit einem Apple- oder Microsoft-Entwicklerzertifikat signiert, deshalb fragt das System einmal nach:

- **macOS**: Öffne die `.dmg` und ziehe DataRove in „Programme“. Meldet macOS beim ersten Öffnen, dass die App nicht überprüft werden kann, öffne **Systemeinstellungen → Datenschutz & Sicherheit**, scrolle nach unten und klicke auf **Dennoch öffnen**. Meldet macOS, die App sei beschädigt, führe im Terminal `xattr -cr /Applications/DataRove.app` aus und öffne sie erneut.
- **Windows**: Starte die `.exe`. Erscheint SmartScreen, klicke auf **Weitere Informationen → Trotzdem ausführen**.
- **Linux**: Mache die `.AppImage` ausführbar und starte sie, oder installiere die `.deb`.
- **Android**: Öffne die `.apk` und erlaube die Installation aus deinem Browser oder Dateimanager. Die App wird mit DataRove auf deinem Computer gekoppelt oder verbindet sich im direkten Modus selbst mit deinen Datenbanken (das Telefon einmal mit deinem DataRove-Konto aktivieren).

Melde dich danach in der App mit deiner E-Mail-Adresse an und aktiviere den Computer. DataRove ist kostenlos; Zahlungsdaten sind nicht nötig.

## Lizenz, Haftungsausschluss und Versionen

- DataRove ist derzeit kostenlos, mit allen Funktionen. Zahlungsdaten sind nicht nötig.
- DataRove ist proprietäre Software, keine Open Source. Für die Nutzung der App gelten die Endbenutzer-Lizenzvereinbarung, der Datenschutzhinweis und die Kaufbedingungen, die die App beim ersten Start anzeigt und denen du dort zustimmst. Siehe [LICENSE.md](LICENSE.md) (maßgeblich ist der englische Text).
- DataRove verändert echte Datenbanken. Halte unabhängige, getestete Backups vor und lies den [Haftungsausschluss](DISCLAIMER.md).
- Versionen folgen der [semantischen Versionierung](https://semver.org/lang/de/). Jede Version listet ihre Änderungen in [CHANGELOG.md](CHANGELOG.md) und enthält eine `SHA256SUMS.txt`.

## Feedback

Fehlerberichte und Wünsche sind in den [Issues](../../issues) willkommen. Dieses Repository enthält Versionshinweise und Installer; der Quellcode ist nicht öffentlich.
