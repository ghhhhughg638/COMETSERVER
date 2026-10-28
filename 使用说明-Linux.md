# Comet Server Linux 版（x86_64）

程序文件：`CometServer-linux-x86_64`。在 Ubuntu 24.04 上用 Nuitka 编译，**需要 glibc 2.38 或更新**：Ubuntu 24.04+、Debian 13+、Fedora 39+ 等可以运行；Ubuntu 22.04、Debian 12 及更老的系统不能运行这个版本。

SHA256：`19a91e53de76e4a273de47094e2c6ceb279170792af596c521c96ff64e8ef9c6`

## 目录

Windows 版和 Linux 版共用同一份 `config/` 和 `admin_ui/`。放在同一个文件夹里：

```
CometServer/
  CometServer-linux-x86_64   服务端程序
  config/                外置配置（卡牌资料、规则、模板）
  admin_ui/              管理后台网页和图片
  config.example.yaml    首次生成 config.yaml 用的模板
```

用 `git clone` 会保留程序的可执行权限；用 “Download ZIP” 下载则需要手动加权限：

```bash
chmod +x CometServer-linux-x86_64
```

## 首次启动

```bash
./CometServer-linux-x86_64
```

旁边没有 `config.yaml` 时，程序会创建它并退出；检查监听地址和端口（默认 `0.0.0.0:5231`，监听所有网卡，请配置防火墙），然后创建主管理员（会提示输入至少 10 位密码）：

```bash
./CometServer-linux-x86_64 --create-admin 你的名字 --owner
```

再次运行 `./CometServer-linux-x86_64` 启动，浏览器打开 `http://服务器地址:5231/admin/`。

数据库自动创建在程序旁的 `data/comet.db`，运行日志是 `app.log`，后台操作日志是 `logs/admin_audit.jsonl`。程序总是使用**可执行文件所在目录**，与你从哪里启动无关。

## 用 systemd 常驻（示例）

把 `/opt/comet` 和用户名换成你的，不要用 root 运行：

```ini
[Unit]
Description=Comet Server
After=network.target

[Service]
User=comet
WorkingDirectory=/opt/comet
ExecStart=/opt/comet/CometServer-linux-x86_64
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

## 注意

- 修改规则、模板和文字后可以在后台热重载；监听端口、数据库路径和登录密钥需要重启。
- 更新时先停止服务并备份 `config.yaml` 和 `data/`，只替换程序文件和 `admin_ui/`。
- 端口低于 1024 需要特权，建议放在反向代理（Nginx/Caddy）后面，并自行配置 HTTPS 与防火墙。
- 后台里的卡牌图片和卡牌资料来自游戏，权利归各自权利人所有。
