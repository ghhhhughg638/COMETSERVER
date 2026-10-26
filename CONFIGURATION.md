> 最新热重载、统计、玩家管理和登录 IP 配置见 [docs/LIVE_OPERATIONS.md](docs/LIVE_OPERATIONS.md)。以下保留早期规则字段说明；“所有修改都需重启”不再适用于当前游戏进程。

# 外部商店与竞技场配置

所有文件位于程序旁的 config 目录。修改后重启生效，不需要重新编译 EXE。请使用合法 JSON：双引号、不写注释、不保留末尾逗号。更新前备份自己的配置。

## 商店

| 文件 | 内容 |
| --- | --- |
| store_template.json | 商店商品列表及展示内容 |
| store_transactions.json | 以 offerId 为键的交易响应、购买奖励 |
| pack_template.json | 玩家卡包列表模板 |
| shop_rules.json | 领取奖励、开包张数和概率 |

shop_rules.json 字段：

| 字段 | 含义 |
| --- | --- |
| claim_rewards | /crate/claim 的完整奖励响应 |
| pack_rules | 以卡包 id 为键的规则，JSON 中的键写作字符串 |
| pack_rules 中的 card_count | 每次开包生成的卡牌数量 |
| pack_rules 中的 gold_probability | 闪卡概率，0 到 1；例如 0.1 为 10% |
| card_defaults | 每张开包卡牌的基础字段，例如 dust、recycled |
| new_card_probability | 返回 new_card=true 的概率，0 到 1 |
| total_dust | 开包响应的 total_dust |

开包卡池仍来自 library_template.json 的 cards。以上配置管理现有接口的响应数据，不会额外实现真实支付、扣款或库存结算。

## 竞技场

文件：draft_rules.json。默认值沿用原服务端。

| 字段 | 含义 |
| --- | --- |
| card_back | 主国家对应的卡背 |
| faction_loc | 主国家对应的总部卡牌名称 |
| m_factions / a_factions | 主国家和盟国列表 |
| num_rarity | 各稀有度卡牌的最大重复数量，受卡组编码格式约束为 1 至 4 |
| rarity_weights | 常规选牌稀有度权重；数值按比例解释，无需合计为 100 |
| rarity_num | 各稀有度每次提供几张卡的权重；数量键写作字符串 |
| m_a_faction | 主国 m 和盟国 a 的抽取权重 |
| total_cards | 轮抽卡组总张数，包含总部；作用于新建/重置竞技场状态 |
| choice_count | 每次提供的候选国家/卡牌数量 |
| ally_selection_at | 达到多少张卡时开始选择盟国 |
| late_weights_at / late_rarity_weights | 末段选牌启用阈值及稀有度权重 |
| ally_card_limit | 盟国卡牌数量上限 |
| max_selection_attempts | 随机抽取尝试次数，超过后在合法卡牌中选择单张候选 |
| excluded_sets | 不参与选牌的卡包/集合 |
| excluded_name_suffixes | 不参与选牌的卡牌名称后缀 |
| ally_rarity_replacements | 盟国抽到某稀有度时改用的稀有度 |
| entry_response | 开始竞技场时返回的 admissions、gold 等字段 |
| completion_rewards | 结束/重置竞技场时返回的奖励列表 |
| overview_cost | 竞技场概览 cost 的 base、per_win、per_loss 系数 |
| overview_reward | 竞技场概览的奖励字段；wins 由玩家战绩填写 |

已有竞技场进度不会随改配置自动迁移。数据中的国家、稀有度和卡牌名称应与 data.json 一致。校验失败会提示配置文件名；如果规则下没有足够合法候选，选牌接口会返回冲突错误。

## 已删除

clientfp 目录、活动 /activity 和公告 /announcement 接口已删除。XActionCheat 动作分支保持启用。


## WebSocket 显示文字

文件：config/ws_texts.json。它只含显示文字，不含消息转发、匹配、鉴权或指令执行代码。

| 字段 | 内容及允许的占位符 |
| --- | --- |
| help / welcome | 帮助和连接欢迎提示 |
| bot_name / world_channel_name | 机器人与世界频道显示名称 |
| world_enabled / world_disabled | 世界频道开关提示 |
| name_changed_disconnect | 改名后断开提示，可使用 {name} |
| name_success / name_full / name_reserved / name_failed | 改名成功、编号满、保留名及失败提示 |
| name_prompt / name_no_spaces / name_error | 改名输入提示 |
| gold_cleared | 闪卡记录清理提示 |
| logged_in_elsewhere | 账号在别处登录提示 |
| unknown_command | 未知命令提示 |
| default_player | 缺少玩家显示名时的名称，可使用 {user_id} |
| world_message | 世界消息格式，可使用 {name}、{tag}、{message} |
| online_status | 在线概览，可使用原文件列出的 online、playing、wait_*、matches_* 占位符 |

普通花括号写作 {{ 和 }}。占位符只允许预先列出的名字，不支持表达式、属性访问或代码执行。指令名称与协议字段属于编译逻辑，不随显示文字修改。修改后重启生效。

## 匹配与乱斗

lobbyplayers 中的卡牌黑名单、版本组合限制和自动 DIY 分流已移除。玩家仍需使用自己的卡组；登录身份、WebSocket 连接和手动封禁检查保留。正常游戏的卡组代码格式解析仍然存在。

乱斗从数据库读取双方实际选择的卡组，使用常规卡组解析流程，不再替换为固定卡组。

## 二进制发布

运行 build.ps1 后，对外提供 dist/CometServer-release.zip。该包只收录 EXE、config、admin_ui 网页资源和说明文档；不会收录 ws.py、其他 Python 源码或 Nuitka 构建目录。外部目录如含 Python 源码或字节码，脚本会拒绝生成发布包。

源码压缩包供开发维护使用，不要把它当成隐藏源码的二进制发布包。


## 统一管理员后台

入口为 /admin/，取代旧 /home 和独立封号页面。源码与 EXE 均支持 --create-admin NAME 交互设置新管理员，密码保存为 PBKDF2 摘要，发行配置不预置账号。

- 服务设置与初始化模板可通过表单或 JSON 结构编辑，保存后仍使用 YAML 格式。
- 卡牌、代码表及交易响应提供表格搜索与单条编辑；其他 JSON 提供结构化表单和高级 JSON 模式。
- 小助手文字提供分项编辑和文字预览；占位符规则仍受校验。
- 保存时携带文件版本；检测到其他程序修改会拒绝覆盖。未保存修改离开页面时会提示。
- 玩家管理可查询、单个/批量封禁与解封，并浏览数据库中的封禁记录。独立管理模式只编辑文件，不连接游戏数据库。
- 本地 config.yaml、登录信息和数据库不随公开包分发。账户凭据变化会使旧后台会话失效。

管理网页与图片保存在 admin_ui/。后台编辑文件的 Python 逻辑编译入 EXE，admin_ui 只有浏览器需要的 HTML、JavaScript、CSS 和图片。


## Vue 前端维护

Vue 3 源码位于 admin_frontend/src，图片源文件位于 admin_frontend/public/assets。修改后在 admin_frontend 执行 npm.cmd run build，生成 admin_ui。不要直接修改 admin_ui 中带内容指纹的构建文件；下次构建会覆盖它们。

开发服务器为 5173，默认把 /admin/api 代理到本机 5232。正式部署继续使用 Python 的 /admin/ 入口，Node.js 仅参与开发和构建。


## FP 与商店展示

config/fp_template.json、store_template.json、store_transactions.json、shop_rules.json 与 pack_template.json 均提供专用可视化编辑，仍保留 JSON 模式。config/presentation_assets.json 为外部展示资源映射，保存或重载后重新读取。详细字段含义、缺图处理和静态交易限制见 docs/PRESENTATION_EDITORS.md。
