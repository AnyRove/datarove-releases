# Disclaimer · 免责声明

[English](#english) · [简体中文](#简体中文)

## English

DataRove is a tool for working with **real databases**. Queries, edits, imports, sync jobs and restores can change or delete data, and some changes cannot be undone.

- **Keep independent, tested backups.** A sync job copies data into another database; it is not a backup. Safety snapshots, confirmation prompts, read-only connections and backup verification reduce risk, but they do not remove it.
- **Check the target** before you run a write, import, sync or restore, especially on production systems. Read the preview and the confirmation.
- **Only connect to databases you are authorized to use**, and give DataRove's database accounts no more privileges than needed. You are responsible for your computer, your credentials, your paired phones and any relay you run.
- DataRove runs on your computer. The developer cannot see or recover your databases, your backups or a forgotten backup-encryption password.
- The software is provided "as is", without a promise that it is free of errors or suits every database or use. To the extent the law allows, the developer is not liable for indirect or consequential loss; the full terms, including the limits that apply and the rights the law does not let anyone take away, are in the End User License Agreement shown in the app (sections 5, 8 and 9).
- If you notice anything that could affect the integrity of your data, stop using the affected feature and [open an issue](../../issues) (or see [SECURITY.md](SECURITY.md) for security problems).

## 简体中文

DataRove 用来操作**真实的数据库**。查询、编辑、导入、同步和恢复都可能修改或删除数据，有些改动无法撤回。

- **请保留独立且验证过的备份。** 同步任务只是把数据复制到另一个库，不能代替备份。安全快照、二次确认、只读连接和备份校验能降低风险，但不能消除风险。
- 执行写入、导入、同步或恢复之前，**请确认目标**，生产环境尤其如此。请阅读预览和确认提示。
- **只连接你有权使用的数据库**，给 DataRove 使用的数据库账号只授予必要的权限。你需要自行负责电脑、凭据、已配对的手机以及自建中转的安全。
- DataRove 运行在你的电脑上。开发者看不到、也无法找回你的数据库、备份文件或遗忘的备份加密密码。
- 软件按「现状」提供，不保证没有错误，也不保证适合所有数据库或用途。在法律允许的范围内，开发者不对间接或附带损失负责；完整条款（包括适用的责任限制，以及法律规定不可排除的权利）以应用内展示的《最终用户许可协议》第 5、8、9 条为准，英文版为准。
- 如果发现任何可能影响数据完整性的问题，请停止使用相关功能并 [提交 Issue](../../issues)（安全问题请看 [SECURITY.md](SECURITY.md)）。
