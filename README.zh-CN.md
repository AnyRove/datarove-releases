<p align="center"><img src="docs/images/icon.svg" width="96" alt="DataRove"></p>

<h1 align="center">DataRove</h1>

<p align="center"><b>在电脑上管理所有数据库，在手机上随时查看——端到端加密。</b></p>

<p align="center">
<a href="README.md">English</a> · <b>简体中文</b> · <a href="README.zh-TW.md">繁體中文</a> · <a href="README.ja.md">日本語</a> · <a href="README.ko.md">한국어</a> · <a href="README.es.md">Español</a> · <a href="README.fr.md">Français</a> · <a href="README.de.md">Deutsch</a> · <a href="README.pt-BR.md">Português</a> · <a href="README.ru.md">Русский</a>
</p>

---

DataRove 是一个自带定时备份和同步的桌面数据库客户端。它运行在你的电脑上（桌面应用或后台服务），手机通过局域网直连或中转服务器连接它。连接全程端到端加密，数据库密码不会离开电脑。

**支持的数据库：** MySQL · MariaDB · PostgreSQL · SQLite · SQL Server · MongoDB · Redis，支持 SSH 隧道和 TLS。

> **状态：** 1.0.0。安装包在 [Releases](../../releases) 页面下载，装好后应用会自动更新。可以 Watch 本仓库接收新版本通知。更新记录见 [CHANGELOG](CHANGELOG.md)。

## 4 分钟功能介绍视频

<p align="center"><a href="docs/video/DataRove-promo.mp4"><img src="docs/video/DataRove-promo-poster.jpg" width="720" alt="DataRove 功能介绍视频（英文配音，中英字幕）"></a></p>
<p align="center"><a href="docs/video/DataRove-promo.mp4">播放视频</a>（MP4，1080p，英文配音，中英双语字幕）· 字幕文件：<a href="docs/video/DataRove-promo.zh-CN.srt">简体中文</a>、<a href="docs/video/DataRove-promo.en.srt">English</a></p>

![概览](docs/images/overview.png)

## 功能

### 浏览和编辑数据
- 虚拟滚动表格，筛选、排序、分页。单元格就地编辑，或把整行打开成表单，每种列类型都有对应的输入方式（布尔、枚举、日期、JSON、NULL / 默认值）。
- 表列表可以按名称、大小或行数排序；每个库都能看到各张表的空间占比。
- 外键列可以在被引用的表里搜索并选择一行。
- 保存前可预览将执行的 SQL。所有修改在一个事务里提交，出错时全部回滚。删除和生产库会先确认。

![数据表格](docs/images/data.png)

### 写查询
- SQL 编辑器按上下文补全：`FROM` / `JOIN` 后补表，`别名.` 后补列，`INSERT` 列清单，函数带签名，语句模板。
- 鼠标悬停在表或列上显示结构、类型和注释。写错的表名、`别名.列`，以及没有 `WHERE` 的 `DELETE` / `UPDATE`，执行前就会标出来。
- 查询结果一键变成柱状图、折线图或饼图。执行计划以树形展示，全表扫描等问题用通俗的话标出来。
- 单表 `SELECT` 的结果只要包含主键，就能直接编辑保存。
- 支持 MongoDB shell 语法（`db.users.find({...})`）和 Redis 命令行，都带补全。

![查询编辑器](docs/images/query.png)

### 看懂结构
- 库 → 表 / 视图 / 集合 → 列、索引、外键、触发器、DDL、行数和大小。
- ER 图；两个库的结构对比并生成迁移脚本（删除语句默认注释掉）；Redis 键浏览；服务器监控（连接数、事务、缓存命中率、会话）。

![ER 图](docs/images/er.png)

### 导入和导出
- 整张表或筛选结果导出为 CSV、TSV、JSON、JSON Lines、SQL `INSERT` 或 Markdown；导入 CSV / TSV / JSON / JSON Lines（UTF-8 和 GBK 都能识别）。
- 大批量传输在电脑后台进行，实时显示进度，可以取消。文件分小块传输，连接断开后接着传，不用从头再来。
- 导入在一个事务里完成：有一行失败就什么都不写，并指出是哪一行。

### 靠得住的备份
- 用原生工具（mysqldump / pg_dump / mongodump）或内置导出备份到文件，支持 gzip 压缩、AES-256-GCM 加密，保留最近 N 份或 N 天。缺少的工具可以在应用里一键安装。
- 每份新备份写完后立即读回校验（大小、SHA-256、解密、解压、解析），坏文件当晚就能发现。之后也可以随时重新校验任意文件，不需要连接数据库。
- 同步到另一个库，也支持跨数据库类型（例如 PostgreSQL → MySQL）：全量替换、清空后导入、追加、按主键 upsert，或按水位列**增量**同步。每张表要么完整写入、要么保持原样，提交前核对行数。
- 临时网络错误自动重试；仍有表失败时，可以**只重试失败的部分**。
- 计划：每隔 N 分钟、每天、每周、每月或 cron。错过的计划在启动后补跑。任务运行时电脑不会休眠。桌面通知、系统通知和 Webhook。任意备份可以恢复到任意连接。

![备份](docs/images/backups.png)

### 误操作可撤销
- 在标记为「生产」的连接上（也可以设为所有可写连接），执行 `UPDATE`、`DELETE`、`DROP`、`TRUNCATE`、表格编辑、覆盖式导入、覆盖式同步、恢复备份或 Redis 修改之前，DataRove 会先保存受影响的表。
- 之后一键撤销：只改回有差异的行，在一个事务里完成，撤销前的状态也会保存。快照加密保存在电脑上，按时间和大小自动清理。

### 手机，放心用
- 手机上是同一套界面（Android 应用，或任意现代手机浏览器）。扫码配对，不需要注册账号。
- 每台手机有自己的权限（只读、可写或管理），可以随时修改或移除。临时提权到期自动收回，操作记录能看到每台手机做了什么。
- 同一 Wi-Fi 下直连电脑，在外面经中转服务器，中转只转发密文。

<p align="center"><img src="docs/images/phone-data.png" width="300" alt="手机上的 DataRove"></p>

### 桌面端
常驻托盘、开机启动，可以把自己装成后台服务（launchd / systemd / Windows 计划任务），关掉窗口定时备份照常执行。命令面板直达任意表和命令。10 套主题，深浅色，15 种界面语言。

## 安全要点

- 保存的密码用本机主密钥加密存在电脑上，界面和已配对的手机只看到 `••••`。
- 扫码配对用二维码里的 256 bit 密钥；手动输入的短配对码经 scrypt 加固。双方互相校验公钥，中转无法偷偷配进自己的设备。
- 每次会话用 X25519 + ChaCha20-Poly1305，并生成新的临时密钥（前向保密）。被篡改、重放或乱序的数据会被拒收，降级攻击会被拒绝。
- 公网中转必须用 `wss://`。可以自建中转，也可以只在局域网内配对。
- 涉及 DataRove 以外的操作只能在电脑上做：工具路径、浏览文件、访问 SQLite 文件、把已保存的密码发往改过的地址。

报告安全漏洞请看 [SECURITY.md](SECURITY.md)。

## 平台

| | |
|---|---|
| macOS | Apple 芯片和 Intel |
| Windows | 10 / 11，x64 |
| Linux | AppImage、deb |
| Android | 应用（作为电脑的手机端，或手机直连） |
| 其他手机 | 任意现代手机浏览器（可添加到主屏幕） |

## 安装

从 [最新版本](../../releases/latest) 下载对应系统的文件。每个版本都在 `SHA256SUMS.txt` 里列出 SHA-256 校验和。安装包暂时没有 Apple / 微软的开发者签名，所以系统会提示一次：

- **macOS**：打开 `.dmg`，把 DataRove 拖到「应用程序」。第一次打开时系统提示无法验证：打开「系统设置 → 隐私与安全性」，往下找到 DataRove，点「仍要打开」。如果提示「已损坏」，在终端执行 `xattr -cr /Applications/DataRove.app` 后再打开。
- **Windows**：运行 `.exe`。出现 SmartScreen 时点「更多信息 → 仍要运行」。
- **Linux**：给 `.AppImage` 加上可执行权限后运行，或安装 `.deb`。
- **Android**：打开 `.apk`，允许浏览器或文件管理器安装应用。应用可以和电脑上的 DataRove 配对使用，也可以用「手机直连」自己连数据库（第一次用 DataRove 账户激活这台手机）。

然后在应用里用邮箱登录，激活这台电脑。DataRove 免费，不需要付款信息。

## 许可、免责与版本

- DataRove 现在免费，包含全部功能，不需要付款信息。
- DataRove 是专有软件，不是开源软件。应用的使用受首次启动时在应用内展示并由你确认的《最终用户许可协议》《隐私说明》和《购买条款》约束。见 [LICENSE.md](LICENSE.md)（英文为准）。
- DataRove 会修改真实的数据库。请保留独立且验证过的备份，并阅读 [免责声明](DISCLAIMER.md)。
- 版本号遵循 [语义化版本](https://semver.org/lang/zh-CN/)。每次发布在 [CHANGELOG.md](CHANGELOG.md) 列出改动并附带 `SHA256SUMS.txt`；应用从这里检查更新，安装前会核对校验和。

## 反馈

欢迎在 [Issues](../../issues) 提交问题和建议。本仓库用于发布说明、安装包和文档，源代码不公开。
