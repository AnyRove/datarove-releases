<p align="center"><img src="docs/images/icon.svg" width="96" alt="DataRove"></p>

<h1 align="center">DataRove</h1>

<p align="center"><b>在電腦上管理所有資料庫，在手機上隨時查看——端對端加密。</b></p>

<p align="center">
<a href="README.md">English</a> · <a href="README.zh-CN.md">简体中文</a> · <b>繁體中文</b> · <a href="README.ja.md">日本語</a> · <a href="README.ko.md">한국어</a> · <a href="README.es.md">Español</a> · <a href="README.fr.md">Français</a> · <a href="README.de.md">Deutsch</a> · <a href="README.pt-BR.md">Português</a> · <a href="README.ru.md">Русский</a>
</p>

---

DataRove 是內建排程備份與同步的桌面資料庫用戶端。它在你的電腦上執行（桌面應用程式或背景服務），手機透過區域網路直連或中繼伺服器連線。連線全程端對端加密，資料庫密碼不會離開電腦。

**支援的資料庫：** MySQL · MariaDB · PostgreSQL · SQLite · SQL Server · MongoDB · Redis，支援 SSH 通道與 TLS。

> **狀態：** 1.0.0。安裝檔在 [Releases](../../releases) 頁面下載，裝好後應用程式會自動更新。可以 Watch 本儲存庫接收新版本通知。更新紀錄見 [CHANGELOG](CHANGELOG.md)。

## 4 分鐘功能介紹影片

<p align="center"><a href="docs/video/DataRove-promo.mp4"><img src="docs/video/DataRove-promo-poster.jpg" width="720" alt="DataRove 功能介紹影片（英文配音，中英字幕）"></a></p>
<p align="center"><a href="docs/video/DataRove-promo.mp4">播放影片</a>（MP4，1080p，英文配音，中英雙語字幕）· 字幕檔：<a href="docs/video/DataRove-promo.en.srt">English</a>、<a href="docs/video/DataRove-promo.zh-CN.srt">简体中文</a></p>

![概覽](docs/images/overview.png)

## 功能

### 瀏覽與編輯資料
- 虛擬捲動表格，篩選、排序、分頁。儲存格就地編輯，或把整列開成表單，每種欄位型別都有對應的輸入方式（布林、列舉、日期、JSON、NULL / 預設值）。
- 外鍵欄位可以在被參照的資料表中搜尋並選取一列。
- 儲存前可預覽將執行的 SQL。所有修改在同一個交易中提交，出錯時全部復原。刪除與正式環境會先確認。
- 複製為 CSV / TSV / JSON / SQL / Markdown，匯出檔案，匯入 CSV / JSON。

![資料表格](docs/images/data.png)

### 撰寫查詢
- SQL 編輯器依上下文補全：`FROM` / `JOIN` 後補資料表，`別名.` 後補欄位，`INSERT` 欄位清單，函式附簽章，語句範本。
- 滑鼠停在資料表或欄位上會顯示結構、型別與註解。寫錯的資料表名稱、`別名.欄位`，以及沒有 `WHERE` 的 `DELETE` / `UPDATE`，執行前就會標示出來。
- 單一資料表的 `SELECT` 結果只要包含主鍵，就能直接編輯儲存。
- 支援 MongoDB shell 語法（`db.users.find({...})`）與 Redis 命令列，皆有補全。

![查詢編輯器](docs/images/query.png)

### 看懂結構
- 資料庫 → 資料表 / 檢視 / 集合 → 欄位、索引、外鍵、觸發器、DDL、列數與大小。
- ER 圖；比對兩個資料庫的結構並產生遷移 SQL；Redis 鍵瀏覽；伺服器監控（連線數、QPS、慢查詢）。

![ER 圖](docs/images/er.png)

### 匯入與匯出
- 整張表或篩選結果匯出為 CSV、TSV、JSON、JSON Lines、SQL `INSERT` 或 Markdown；匯入 CSV / TSV / JSON / JSON Lines（UTF-8 與 GBK 都能辨識）。
- 大量傳輸在電腦背景進行，即時顯示進度，可以取消。檔案分小塊傳輸，連線中斷後接著傳。
- 匯入在一個交易裡完成：有一列失敗就什麼都不寫，並指出是哪一列。

### 靠得住的備份
- 使用原生工具（mysqldump / pg_dump / mongodump）或內建匯出備份到檔案，支援 gzip 壓縮、AES-256-GCM 加密，保留最近 N 份或 N 天。
- 每份新備份寫完後立即讀回驗證（校驗和、解密、解析），壞掉的檔案當晚就能發現。
- 同步到另一個資料庫：完整取代、清空後匯入、附加、依主鍵 upsert，或依水位欄位**增量**同步（`updated_at`、自動遞增 id 等）。
- 排程：每 N 分鐘、每天、每週、每月或 cron。錯過的排程在啟動後補跑。桌面通知與 Webhook。任何備份一鍵還原到任何連線。
- 同步也支援跨資料庫類型（例如 PostgreSQL → MySQL）。每張表要麼完整寫入、要麼保持原樣，提交前核對列數；暫時性錯誤自動重試，仍失敗時可以**只重試失敗的部分**。缺少的工具（pg_dump 等）可以在應用程式裡一鍵安裝。

![備份](docs/images/backups.png)

### 誤操作可復原
- 在標記為「正式」的連線上，執行 `UPDATE`、`DELETE`、`DROP`、`TRUNCATE`、表格編輯、覆寫式匯入、同步或還原之前，DataRove 會先儲存受影響的表。
- 之後一鍵復原：只改回有差異的列，在一個交易裡完成。快照加密儲存在電腦上。

### 手機，安心用
- 手機上是同一套介面。掃描 QR Code 配對，不需要註冊帳號。
- 每支手機有自己的權限（唯讀、可寫或管理），可隨時修改或移除。臨時提權到期自動收回，活動紀錄能看到每支手機做了什麼。
- 同一個 Wi-Fi 下直連電腦，在外面則經由中繼伺服器，中繼只轉送密文。

<p align="center"><img src="docs/images/phone-data.png" width="300" alt="手機上的 DataRove"></p>

### 桌面端
常駐系統匣、開機啟動，可以把自己安裝成背景服務（launchd / systemd / Windows 工作排程器），關掉視窗排程備份照常執行。10 套主題，深淺色，15 種介面語言。

## 安全重點

- 儲存的密碼以本機主金鑰加密存放在電腦上，介面與已配對的手機只看得到 `••••`。
- 掃碼配對使用 QR Code 中的 256 bit 金鑰；手動輸入的短配對碼經 scrypt 強化。雙方互相驗證公鑰，中繼無法偷偷配對自己的裝置。
- 每次工作階段使用 X25519 + ChaCha20-Poly1305，並產生新的臨時金鑰（前向保密）。遭竄改、重送或亂序的資料會被拒收，降級攻擊會被拒絕。
- 公網中繼必須使用 `wss://`。可以自架中繼，也可以只在區域網路內配對。
- 涉及 DataRove 以外的操作只能在電腦上進行：工具路徑、瀏覽檔案、存取 SQLite 檔案、把已儲存的密碼送往變更過的位址。

回報安全漏洞請看 [SECURITY.md](SECURITY.md)。

## 平台

| | |
|---|---|
| macOS | Apple 晶片與 Intel |
| Windows | 10 / 11，x64 |
| Linux | AppImage、deb |
| Android | 應用程式（作為電腦的手機端，或手機直連） |
| 手機 | 任何現代行動瀏覽器（可加入主畫面） |

## 安裝

從 [最新版本](../../releases/latest) 下載對應系統的檔案。每個版本都在 `SHA256SUMS.txt` 列出 SHA-256 校驗和。安裝檔暫時沒有 Apple / 微軟的開發者簽章，所以系統會提示一次：

- **macOS**：打開 `.dmg`，把 DataRove 拖到「應用程式」。第一次打開時系統提示無法驗證：打開「系統設定 → 隱私權與安全性」，往下找到 DataRove，點「強制打開」。如果提示「已損毀」，在終端機執行 `xattr -cr /Applications/DataRove.app` 後再打開。
- **Windows**：執行 `.exe`。出現 SmartScreen 時點「其他資訊 → 仍要執行」。
- **Linux**：給 `.AppImage` 加上執行權限後執行，或安裝 `.deb`。
- **Android**：打開 `.apk`，允許瀏覽器或檔案管理員安裝應用程式。應用程式可以和電腦上的 DataRove 配對使用，也可以用「手機直連」自己連資料庫（第一次用 DataRove 帳戶啟用這台手機）。

然後在應用程式裡用電子郵件登入，啟用這台電腦。DataRove 免費，不需要付款資訊。

## 授權、免責與版本

- DataRove 現在免費，包含全部功能，不需要付款資訊。
- DataRove 是專有軟體，不是開源軟體。應用程式的使用受首次啟動時在應用程式內顯示並由你確認的《最終使用者授權合約》《隱私說明》與《購買條款》約束。見 [LICENSE.md](LICENSE.md)（以英文為準）。
- DataRove 會修改真實的資料庫。請保留獨立且驗證過的備份，並閱讀 [免責聲明](DISCLAIMER.md)。
- 版本號遵循 [語意化版本](https://semver.org/lang/zh-TW/)。每次發布在 [CHANGELOG.md](CHANGELOG.md) 列出變更並附上 `SHA256SUMS.txt`。

## 意見回饋

歡迎在 [Issues](../../issues) 回報問題與提出建議。本儲存庫用於發布說明與安裝檔，原始碼不公開。
