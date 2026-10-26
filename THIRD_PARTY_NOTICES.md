# 第三方组件与许可

本发行版使用 Python 3.12.10 和 `requirements.txt` 中的运行依赖。以下列出项目直接依赖及其构建环境解析到的主要传递依赖版本和许可证标识。各组件的完整许可证文本和声明应以对应发行包为准，并在分发时一并保留。

| 组件 | 版本 | 许可证 |
| --- | --- | --- |
| Python | 3.12.10 | Python Software Foundation License 2.0 |
| aiofiles | 25.1.0 | Apache-2.0 |
| aiosqlite | 0.22.1 | MIT |
| AnyIO | 4.15.1 | MIT |
| click | 8.5.0 | BSD-3-Clause |
| Falcon | 4.4.0 | Apache-2.0 |
| Granian | 2.8.5 | BSD-3-Clause |
| idna | 3.20 | BSD-3-Clause |
| iso8601 | 2.1.0 | MIT |
| psutil | 7.2.2 | BSD-3-Clause |
| PyJWT | 2.15.1 | MIT |
| pypika-tortoise | 0.6.5 | Apache-2.0 |
| PyYAML | 6.0.3 | MIT |
| Tortoise ORM | 1.1.9 | Apache-2.0 |
| tzdata | 2026.5 | Apache-2.0 |
| Vue（打包进后台网页） | 3.5.x | MIT |
| Vite（构建工具，部分运行时代码随网页打包） | 8.x | MIT |

## Nuitka Runtime Library

The Nuitka Runtime Library is licensed under AGPLv3 with the Nuitka Runtime Library Exception 1.0. The exception permits a target program produced by the Nuitka compilation process to be distributed under terms of the distributor's choice when independent modules are combined with the runtime through that process. The Nuitka compiler itself is not included in the server release.

The exception text is distributed with Nuitka's package as `LICENSE-RUNTIME.txt`. The full AGPLv3 text is available at <https://www.gnu.org/licenses/agpl-3.0.html>.

## Scope

This inventory covers the application's declared runtime requirements and the main resolved dependencies used by this build. It does not replace the original license files or notices for those components, nor the terms for Windows runtime libraries and other system components included by the build process.

## 游戏素材

后台中的卡牌图片、头像与角色图片及卡牌资料来自游戏或用户提供的素材，相关权利归各自的权利人所有。本发行仓库不对这些素材授予任何许可，使用者应自行确认再分发是否合规。
