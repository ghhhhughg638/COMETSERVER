# Comet Server

Comet Server 的 Windows x64 二进制发行版。公开发行仓库只提供编译后的服务端程序，不包含项目源码。

## 下载

从 GitHub 仓库的 **Releases** 页面下载 `CometServer.exe`，将它放进一个可写入的独立文件夹。

## 首次启动

1. 运行一次 `CometServer.exe`。如果旁边没有 `config.yaml`，程序会创建配置文件并退出。
2. 编辑 `config.yaml`，检查监听地址、端口和管理员设置。默认监听地址为 `0.0.0.0`，端口为 `5231`，这会监听本机所有网络接口；请按实际部署设置防火墙。
3. 再次运行 `CometServer.exe` 启动服务。

`config.yaml` 含有 JWT 密钥等安全配置，不要公开上传或发送给他人。

## 数据与日志

- 默认数据库为 EXE 旁的 `game.sqlite3`。文件不存在时，程序会自动创建数据库和表；已有数据库会继续使用。
- 日志保存在 EXE 旁的 `logs` 文件夹，按日期分目录。每个日志文件达到 10 MiB 后会开始写入新文件。
- 更新 EXE 前请先停止服务，并备份 `config.yaml` 和 `game.sqlite3`。

如果在公开网络运行服务，请自行配置防火墙和网络访问规则。提交问题时不要附上未脱敏的配置文件、数据库或日志。

## 许可

程序二进制的使用和原样再分发条件见 [`LICENSE.md`](LICENSE.md)。构建依赖和第三方许可说明见 [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md)；这些组件仍受各自许可证约束。
