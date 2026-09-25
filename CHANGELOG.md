# 更新记录

## 首次公开版本

- 发布 Windows x64 单文件服务端程序 `CometServer.exe`。
- 首次运行时在程序旁创建 `config.yaml`，然后退出；配置完成后再次运行即可启动。
- 默认使用 SQLite，缺少数据库文件时自动创建 `game.sqlite3` 和所需数据表。
- 日志按日期保存在 `logs` 文件夹中，每个文件达到 10 MiB 后轮换到新文件。
