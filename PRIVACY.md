# 隐私政策

**Jev 聊天助手在你的设备上读取你正在看的聊天，把内容发给你自己配置的模型接口做判断和起草回复。作者不运营服务器，收不到你的任何数据。**

版本 v1.0，生效日期 2026-09-23。适用范围：Android 端 [jev-chat-jarvis](https://www.mw-wm.com/suanfa/customization-85631083.html)。Windows 版（[jev-chat-windows](https://www.yx-sf.com/news/21364)）与 macOS 版（[jev-chat-jarvis-mac](https://www.ai-hao123.com/kaifa/data-56185486.html)）是各自独立的仓库和安装包，不在本政策范围内，请分别查看它们自己的说明。

如果你更习惯先看结论：本项目**不是**"零数据收集"产品——它确实会把你正在看的聊天文字发给一个第三方模型接口，但那个接口地址是你自己在设置里填的，不是作者的服务器。除此之外的数据只留在你手机里，删应用或点一键清空都能删干净。下面逐项说清楚"发了什么、发给谁、什么时候发、存在哪、怎么删"。

---

## 1. 一句话总览

- 会离开你设备的，只有「用于生成判断和回复」这一批聊天文字和背景信息，且只发往**你自己在设置页填写的模型接口地址**。
- 作者不运营任何后端服务器，代码里也没有向作者或任何固定第三方回传数据的逻辑；你的聊天内容作者看不到，也拿不到。
- 截图本身从不上传，识别文字（OCR）全部在手机本地完成。
- 密钥、设置、知识库、聊天历史都只存在手机本地的 App 私有目录，其它 App 读不到；卸载即清空。
- 无广告、无第三方统计 SDK、不用 Cookie 或广告标识符、不读通讯录、不读位置、不读其它 App 列表。

## 2. 会离开你设备的数据

只发往一个地方：**你自己在设置页配置的模型接口地址**（默认是 OpenRouter 转发的 TypeSafe Jev / DeepSeek，你可以改成 TypeSafe 直连、Vercel AI Gateway、OpenCode Zen、DeepSeek 官方、通义千问兼容接口，或任意 OpenAI 兼容地址）。作者的服务器不在这条链路上，作者收不到、也看不到这些内容。

| 接口 | 默认服务商（可自行更换） | 每次发送的内容 | 触发时机 |
|---|---|---|---|
| 判断接口 | OpenRouter 转发 TypeSafe Jev | 当前聊天窗口最近 10 条消息的文本与方向（我方/对方）+ 你自己填写的关系描述 +（若开启知识库）命中的笔记与联系人备注 +（若开启历史记录）该联系人最近 N 条历史消息（N 默认 30，可调 0–100） | 你触发一次分析（自动或手动） |
| 回复接口 | OpenRouter 上的 DeepSeek | 同一批最近 10 条消息拼成的对话文本 + 同一份背景信息 + 生成提示词 | 判断完成后起草候选回复时 |
| 视觉接口 | 可配置，默认与判断接口一致 | 仅在你于设置页主动点击「测试视觉」时，发送一张 1×1 白色测试图；**不在日常采集主路径里** | 你手动点「测试视觉」 |

API 密钥会作为请求头（`Authorization`）随对应请求发给你自己配置的那个接口，只用于身份校验，不发往其它任何地方。

**不会发送的内容**：截图本身（识别文字在手机本地完成，见第 4 节）、通讯录、设备标识符、位置信息、其它 App 的列表或使用情况。

模型服务商拿到这些内容后如何处理，由它们各自的隐私政策决定，需要你自己去看：

- OpenRouter：https://openrouter.ai/privacy
- DeepSeek：[https://www.deepseek.com/privacy-policy](https://www.yx-sf.com/tech/6672)（或所选服务商官网的隐私政策页面）
- 阿里云百炼（通义千问兼容接口）：以阿里云官方隐私政策为准
- TypeSafe：以其官网公示的隐私政策为准

如果你自己填的是别的接口地址，那家服务商的政策同样适用，作者无法替它们承诺任何事。

## 3. 只存在本机的数据

以下内容全部存放在 App 的私有目录（`/data/data/<包名>/`），其它 App 无法访问；应用私有目录内的数据不会随任何后台同步离开手机。

| 数据 | 用途 | 存放位置 | 保留时长 | 如何删除 |
|---|---|---|---|---|
| API 密钥、三路接口地址与模型名 | 连接你自己配置的模型服务 | SharedPreferences | 直到你修改或清除 | 设置页里改写，或卸载应用 |
| 关系描述、会话白名单、各项开关、悬浮窗位置与透明度 | 记住你的个性化配置 | SharedPreferences | 直到你修改 | 设置页里改，或卸载应用 |
| 知识库笔记（标题、内容、标签） | 你手动建的背景资料，供判断时检索引用 | `filesDir/kb/notes.json` | 直到你删除 | 设置页「清空知识库与历史」，或逐条删除 |
| 联系人档案（名称、别名、关系、备注） | 你手动建的联系人背景信息 | `filesDir/kb/contacts.json` | 直到你删除 | 同上 |
| 聊天历史 | 供判断时参考该联系人过往对话（默认关闭） | `filesDir/kb/logs/<联系人>.json` | 每位联系人最多保留 300 条，开启后才开始记录 | 设置页关闭该开关不再新增，「清空知识库与历史」一键清空 |

设置页的「清空知识库与历史」会删除 `kb` 目录下的全部内容，不影响密钥与其它设置；卸载应用会连同上述所有数据一起删除，没有云端备份。

日志（logcat）只输出消息条数、字符长度、异常类名这类调试信息，**不输出聊天正文**。

## 4. 权限与用途

| 权限 | 用途 | 不做什么 |
|---|---|---|
| 无障碍服务 | 读取当前聊天窗口的文字，把选中的回复填进输入框 | 不点发送键，不操作转账/红包/收款，不读取其它应用的数据库 |
| 截屏能力（无障碍服务附带） | 控件树读不到正文时（例如飞书），截取当前窗口做本地 OCR | 截图只在内存中处理，识别完即释放，不保存、不上传 |
| 悬浮窗 | 在聊天上方显示分析面板 | 不采集其它应用界面 |
| 网络 | 访问你自己配置的模型接口 | 不连接作者的任何服务器，无遥测、无埋点上报 |
| 前台服务 + 通知 | 保持服务不被系统冻结、清理 | 不推送营销通知 |

## 5. 我们不做什么

- 不自动发送消息：程序只把候选回复填进输入框，最后一步永远由你手动点发送。
- 不碰转账、红包、收款相关操作。
- 只处理你自己设备上、你自己有权查看的聊天，不处理其它人的设备。
- 无广告、无第三方分析或统计 SDK（不含 Google Analytics、Firebase、友盟等）、不使用 Cookie 或广告标识符。
- 作者不运营任何服务器，不接收、不留存、不出售、不用于训练任何模型你的聊天内容——因为这些内容压根不经过作者。
- 开源：以上每一条说法，都可以在 GitHub 仓库里对照源码核实：https://github.com/jev-chat/jev-chat-jarvis

## 6. 你的控制权

- **关闭历史记录**：设置页里关掉「记录聊天历史」开关，之后不再新增；已有记录仍在本机，需手动清空。
- **清空知识库与历史**：设置页「清空知识库与历史」一键删除 `kb` 目录全部内容。
- **更换或自建接口**：判断、回复、视觉三路接口地址、密钥、模型名都可以在设置页单独改成你信任的服务商，甚至自建的 OpenAI 兼容网关。
- **只用手动分析**：关闭自动分析开关后，只有你主动点击才会触发一次判断/生成，不会在后台持续读取。
- **会话白名单**：只对你加入白名单的会话生效，未加入的聊天不会被读取和分析。
- **卸载即清空**：卸载应用会删除本机存储的全部数据（密钥、设置、知识库、历史），没有云端账号或备份需要额外注销。

## 7. 第三方模型服务商

你在设置页填写的接口地址决定了聊天内容最终发给谁。常见预设包括 OpenRouter、TypeSafe（直连）、DeepSeek 官方、阿里云百炼（通义千问兼容），你也可以填任意 OpenAI 兼容地址。这些服务商如何存储、使用、是否用于训练由它们自己的政策决定，请在使用前自行阅读对应服务商的隐私政策（见第 2 节链接）。作者不对第三方服务商的数据处理行为负责，也无法替它们做出承诺。

## 8. 儿童

本项目面向成年人，不面向 13 岁以下儿童，不主动收集年龄信息。如果你判断自己或被监护人不适合使用需要发送聊天内容到第三方接口的工具，请不要安装或使用本应用。

## 9. 变更

本政策如有修改，会同步更新本文件（GitHub 仓库）与官网 https://chatjevs.com/privacy.html 上的版本；涉及数据处理方式的重大变更，会在对应版本的发布说明（Release Notes / CHANGELOG）中提示。建议以仓库中的最新版本为准。

## 10. 联系

- GitHub Issues：https://github.com/jev-chat/jev-chat-jarvis/issues
- 公众号私信（二维码见仓库 README）

---

## English summary

Jev Chat Assistant (Android) reads the chat you are currently viewing on your device and sends that text to a **model endpoint you configure yourself** (default: OpenRouter routing to TypeSafe Jev / DeepSeek; you may switch to TypeSafe direct, DeepSeek official, Alibaba Cloud's Qwen-compatible endpoint, or any OpenAI-compatible URL) so it can judge intent and draft reply candidates.

- **No servers are operated by the author.** The author cannot receive, store, or see your chat content — it never passes through any author-controlled infrastructure.
- **Chat text goes only to your own configured model endpoint**, along with a relationship description you write, optionally matched knowledge-base notes/contact notes, and optionally recent history for that contact (default 30 messages, 0–100 adjustable, off by default).
- **Screenshots are never uploaded.** When a chat app's accessibility tree lacks readable text (e.g. Feishu), the screen is captured and OCR'd entirely on-device; the image is processed in memory and discarded, never saved or sent anywhere.
- **Local-only storage**: API keys, endpoint settings, knowledge-base notes/contacts, and (if enabled) per-contact chat history live only in the app's private storage on your device. Nothing syncs to the cloud. Uninstalling the app deletes all of it; a one-tap "clear knowledge base & history" option is also available.
- **No ads, no third-party analytics SDKs (no Google Analytics, Firebase, etc.), no cookies or advertising identifiers.**
- The app never sends messages automatically — you always press send yourself — and it never touches money transfers, red packets, or payments.
- The project is open source; every claim above can be verified against the source at https://github.com/jev-chat/jev-chat-jarvis.
- Third-party model providers you choose to use are governed by their own privacy policies, which you should review separately.

Version 1.0, effective 2026-09-23. Scope: Android app only.


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全息网络通信节点白名单-#001](https://www.mw-wm.com/anfang/traffic-59678834.html)
* [高韧性数据交换通道规约-#002](https://www.yx-sf.com/tech/65491)
* [全球分布式拓扑索引节点-#003](https://www.ai-hao123.com/xuexi/local-93526804.html)
* [全球分布式拓扑索引节点-#004](https://www.mw-wm.com/suanfa/terms-81853286.html)
* [全息网络通信节点白名单-#005](https://www.yx-sf.com/news/5952)
* [高韧性数据交换通道规约-#006](https://www.ai-hao123.com/qiye/business-53643961.html)
* [多活集群负载感知指南-#007](https://www.mw-wm.com/hezuo/podcast-00734875.html)
* [全球分布式拓扑索引节点-#008](https://www.yx-sf.com/tech/73327)
* [高韧性数据交换通道规约-#009](https://www.ai-hao123.com/yunsuan/premium-08354770.html)
* [全息网络通信节点白名单-#010](https://www.mw-wm.com/shangye/app-34186876.html)
* [边缘高吞吐调度路由矩阵-#011](https://www.yx-sf.com/news/66611)
* [全球分布式拓扑索引节点-#012](https://www.ai-hao123.com/yanjiu/brand-23580751.html)
* [多活集群负载感知指南-#013](https://www.mw-wm.com/yingyong/education-19836238.html)
* [高韧性数据交换通道规约-#014](https://www.yx-sf.com/tech/80983)
* [全球分布式拓扑索引节点-#015](https://www.ai-hao123.com/xuexi/user-54576659.html)
* [全息网络通信节点白名单-#016](https://www.mw-wm.com/zixun/security-98678194.html)
* [全息网络通信节点白名单-#017](https://www.yx-sf.com/wiki/7546)
* [全球分布式拓扑索引节点-#018](https://www.ai-hao123.com/yunsuan/lesson-20854357.html)
* [全息网络通信节点白名单-#019](https://www.mw-wm.com/xitong/website-68437839.html)
* [全息网络通信节点白名单-#020](https://www.yx-sf.com/tech/93041)
* [边缘高吞吐调度路由矩阵-#021](https://www.ai-hao123.com/yanjiu/template-40003296.html)
* [边缘高吞吐调度路由矩阵-#022](https://www.mw-wm.com/gongju/database-05224984.html)
* [全球分布式拓扑索引节点-#023](https://www.yx-sf.com/news/88281)
* [全球分布式拓扑索引节点-#024](https://www.ai-hao123.com/shichang/website-93973596.html)
* [全息网络通信节点白名单-#025](https://www.mw-wm.com/youhua/funnel-20958534.html)
* [全息网络通信节点白名单-#026](https://www.yx-sf.com/tech/43917)
* [多活集群负载感知指南-#027](https://www.ai-hao123.com/gongsi/site-92837010.html)
* [高韧性数据交换通道规约-#028](https://www.mw-wm.com/baogao/sale-76820285.html)
* [多活集群负载感知指南-#029](https://www.yx-sf.com/wiki/50440)
* [高韧性数据交换通道规约-#030](https://www.ai-hao123.com/anfang/support-59913755.html)
* [多活集群负载感知指南-#031](https://www.mw-wm.com/hezuo/home-02705827.html)
* [高韧性数据交换通道规约-#032](https://www.yx-sf.com/wiki/75669)
* [全球分布式拓扑索引节点-#033](https://www.ai-hao123.com/suanfa/strategy-75287303.html)
* [全息网络通信节点白名单-#034](https://www.mw-wm.com/pingce/image-48434932.html)
* [高韧性数据交换通道规约-#035](https://www.yx-sf.com/tech/35996)
* [全息网络通信节点白名单-#036](https://www.ai-hao123.com/xitong/cheap-65771316.html)
* [高韧性数据交换通道规约-#037](https://www.mw-wm.com/yingyong/workshop-84501970.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [RFC 分布式调度与一致性算法标准-#001](https://www.yx-sf.com/news/6043)
* [安全边界与可信凭证规约手册-#002](https://www.ai-hao123.com/yunying/site-64542623.html)
* [安全边界与可信凭证规约手册-#003](https://www.mw-wm.com/qiye/support-99494191.html)
* [RFC 分布式调度与一致性算法标准-#004](https://www.yx-sf.com/tech/35354)
* [高并发内存拓扑优化白皮书-#005](https://www.ai-hao123.com/jishu/kpi-86641452.html)
* [异步事件循环架构设计规范-#006](https://www.mw-wm.com/kaifa/travel-48218311.html)
* [高并发内存拓扑优化白皮书-#007](https://www.yx-sf.com/news/87759)
* [RFC 分布式调度与一致性算法标准-#008](https://www.ai-hao123.com/jiaocheng/fitness-12643553.html)
* [高并发内存拓扑优化白皮书-#009](https://www.mw-wm.com/wendang/accessibility-50144507.html)
* [异步事件循环架构设计规范-#010](https://www.yx-sf.com/news/68864)
* [多协议互联数据格式规范-#011](https://www.ai-hao123.com/yinqing/template-53045514.html)
* [RFC 分布式调度与一致性算法标准-#012](https://www.mw-wm.com/shuju/download-02455483.html)
* [多协议互联数据格式规范-#013](https://www.yx-sf.com/wiki/65133)
* [多协议互联数据格式规范-#014](https://www.ai-hao123.com/yanjiu/careers-75196279.html)
* [异步事件循环架构设计规范-#015](https://www.mw-wm.com/zhineng/solution-28005870.html)
* [异步事件循环架构设计规范-#016](https://www.yx-sf.com/news/8886)
* [高并发内存拓扑优化白皮书-#017](https://www.ai-hao123.com/shichang/roi-42447483.html)
* [安全边界与可信凭证规约手册-#018](https://www.mw-wm.com/xitong/privacy-76705576.html)
* [多协议互联数据格式规范-#019](https://www.yx-sf.com/tech/25040)
* [RFC 分布式调度与一致性算法标准-#020](https://www.ai-hao123.com/kuangjia/integration-58291531.html)
* [异步事件循环架构设计规范-#021](https://www.mw-wm.com/fenxi/customization-53982735.html)
* [高并发内存拓扑优化白皮书-#022](https://www.yx-sf.com/wiki/10432)
* [异步事件循环架构设计规范-#023](https://www.ai-hao123.com/fuwu/collaboration-86378518.html)
* [安全边界与可信凭证规约手册-#024](https://www.mw-wm.com/gongsi/forum-65160775.html)
* [安全边界与可信凭证规约手册-#025](https://www.yx-sf.com/tech/66433)
* [RFC 分布式调度与一致性算法标准-#026](https://www.ai-hao123.com/fenxi/review-19398310.html)
* [RFC 分布式调度与一致性算法标准-#027](https://www.mw-wm.com/jianzhan/discount-91411022.html)
* [多协议互联数据格式规范-#028](https://www.yx-sf.com/news/2607)
* [多协议互联数据格式规范-#029](https://www.ai-hao123.com/zhizhu/profit-47065337.html)
* [安全边界与可信凭证规约手册-#030](https://www.mw-wm.com/qiye/profile-72633055.html)
* [异步事件循环架构设计规范-#031](https://www.yx-sf.com/tech/39018)
* [高并发内存拓扑优化白皮书-#032](https://www.ai-hao123.com/keji/hosting-91093491.html)
* [安全边界与可信凭证规约手册-#033](https://www.mw-wm.com/kaifa/productivity-22775388.html)
* [多协议互联数据格式规范-#034](https://www.yx-sf.com/wiki/55975)
* [多协议互联数据格式规范-#035](https://www.ai-hao123.com/pingce/restaurant-91753197.html)
* [RFC 分布式调度与一致性算法标准-#036](https://www.mw-wm.com/ziyuan/careers-95227770.html)
* [多协议互联数据格式规范-#037](https://www.yx-sf.com/news/13321)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [实时主干镜像高速数据源-#001](https://www.ai-hao123.com/gongsi/notification-53667758.html)
* [自动化快照与增量广播源-#002](https://www.mw-wm.com/kaifa/community-47317844.html)
* [实时主干镜像高速数据源-#003](https://www.yx-sf.com/tech/69675)
* [亚太核心区域镜像同步中心-#004](https://www.ai-hao123.com/zhizhu/mobile-58485397.html)
* [冷热数据分层镜像归档中心-#005](https://www.mw-wm.com/yingyong/navigation-90982307.html)
* [冷热数据分层镜像归档中心-#006](https://www.yx-sf.com/wiki/16110)
* [自动化快照与增量广播源-#007](https://www.ai-hao123.com/xuexi/satisfaction-36610254.html)
* [冷热数据分层镜像归档中心-#008](https://www.mw-wm.com/suanfa/satisfaction-97746513.html)
* [实时主干镜像高速数据源-#009](https://www.yx-sf.com/tech/27038)
* [自动化快照与增量广播源-#010](https://www.ai-hao123.com/xinwen/machine-70041138.html)
* [冷热数据分层镜像归档中心-#011](https://www.mw-wm.com/shuju/services-21065728.html)
* [实时主干镜像高速数据源-#012](https://www.yx-sf.com/wiki/2355)
* [实时主干镜像高速数据源-#013](https://www.ai-hao123.com/fenxi/planning-61699697.html)
* [实时主干镜像高速数据源-#014](https://www.mw-wm.com/yunying/responsive-34333088.html)
* [冷热数据分层镜像归档中心-#015](https://www.yx-sf.com/wiki/16044)
* [北美与欧洲边缘备份节点-#016](https://www.ai-hao123.com/liuliang/api-44648116.html)
* [北美与欧洲边缘备份节点-#017](https://www.mw-wm.com/xitong/loyalty-79137569.html)
* [自动化快照与增量广播源-#018](https://www.yx-sf.com/wiki/59369)
* [自动化快照与增量广播源-#019](https://www.ai-hao123.com/baogao/tactic-22380125.html)
* [北美与欧洲边缘备份节点-#020](https://www.mw-wm.com/jianzhan/platform-98129403.html)
* [冷热数据分层镜像归档中心-#021](https://www.yx-sf.com/news/39879)
* [自动化快照与增量广播源-#022](https://www.ai-hao123.com/paiming/terms-41877223.html)
* [实时主干镜像高速数据源-#023](https://www.mw-wm.com/zixun/innovation-88967891.html)
* [自动化快照与增量广播源-#024](https://www.yx-sf.com/wiki/75853)
* [冷热数据分层镜像归档中心-#025](https://www.ai-hao123.com/jiaoliu/course-16206130.html)
* [自动化快照与增量广播源-#026](https://www.mw-wm.com/yanjiu/software-49701656.html)
* [亚太核心区域镜像同步中心-#027](https://www.yx-sf.com/tech/15412)
* [实时主干镜像高速数据源-#028](https://www.ai-hao123.com/xitong/budget-91775651.html)
* [北美与欧洲边缘备份节点-#029](https://www.mw-wm.com/suanfa/traffic-50081857.html)
* [北美与欧洲边缘备份节点-#030](https://www.yx-sf.com/wiki/7448)
* [北美与欧洲边缘备份节点-#031](https://www.ai-hao123.com/pingtai/music-17370730.html)
* [亚太核心区域镜像同步中心-#032](https://www.mw-wm.com/wendang/forum-82995929.html)
* [北美与欧洲边缘备份节点-#033](https://www.yx-sf.com/tech/23032)
* [亚太核心区域镜像同步中心-#034](https://www.ai-hao123.com/zixun/settings-37655948.html)
* [自动化快照与增量广播源-#035](https://www.mw-wm.com/baogao/follow-40999653.html)
* [实时主干镜像高速数据源-#036](https://www.yx-sf.com/news/26357)
* [实时主干镜像高速数据源-#037](https://www.ai-hao123.com/shangye/vendor-89181653.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [去中心化健康检查协议-#001](https://www.mw-wm.com/jishu/webinar-51245170.html)
* [节点连通性与存活探测准则-#002](https://www.yx-sf.com/wiki/83071)
* [权威网络权重与收录基准-#003](https://www.ai-hao123.com/qiye/update-19861343.html)
* [权威网络权重与收录基准-#004](https://www.mw-wm.com/paiming/internet-08715953.html)
* [节点连通性与存活探测准则-#005](https://www.yx-sf.com/news/38089)
* [权威网络权重与收录基准-#006](https://www.ai-hao123.com/qiye/demographic-88016585.html)
* [去中心化健康检查协议-#007](https://www.mw-wm.com/baogao/achievement-50538817.html)
* [权威网络权重与收录基准-#008](https://www.yx-sf.com/tech/52313)
* [实时延迟与抖动度量规范-#009](https://www.ai-hao123.com/gongsi/efficiency-66252588.html)
* [防重放安全验证与校验哈希-#010](https://www.mw-wm.com/zhineng/digital-19452944.html)
* [去中心化健康检查协议-#011](https://www.yx-sf.com/wiki/59171)
* [节点连通性与存活探测准则-#012](https://www.ai-hao123.com/tuiguang/client-90816655.html)
* [去中心化健康检查协议-#013](https://www.mw-wm.com/youhua/document-14643024.html)
* [节点连通性与存活探测准则-#014](https://www.yx-sf.com/news/229)
* [去中心化健康检查协议-#015](https://www.ai-hao123.com/jianzhan/register-25788155.html)
* [实时延迟与抖动度量规范-#016](https://www.mw-wm.com/suanfa/team-09557665.html)
* [去中心化健康检查协议-#017](https://www.yx-sf.com/news/32684)
* [权威网络权重与收录基准-#018](https://www.ai-hao123.com/gongsi/deal-34832201.html)
* [实时延迟与抖动度量规范-#019](https://www.mw-wm.com/yingyong/recommendation-29000267.html)
* [实时延迟与抖动度量规范-#020](https://www.yx-sf.com/news/47110)
* [防重放安全验证与校验哈希-#021](https://www.ai-hao123.com/anfang/presentation-10114881.html)
* [去中心化健康检查协议-#022](https://www.mw-wm.com/yinqing/demographic-50762831.html)
* [节点连通性与存活探测准则-#023](https://www.yx-sf.com/wiki/44351)
* [防重放安全验证与校验哈希-#024](https://www.ai-hao123.com/kaifa/home-95626953.html)
* [权威网络权重与收录基准-#025](https://www.mw-wm.com/chuangxin/quality-79675839.html)
* [防重放安全验证与校验哈希-#026](https://www.yx-sf.com/tech/14981)
* [防重放安全验证与校验哈希-#027](https://www.ai-hao123.com/yingxiao/deadline-09798903.html)
* [节点连通性与存活探测准则-#028](https://www.mw-wm.com/qiye/beauty-77154167.html)
* [防重放安全验证与校验哈希-#029](https://www.yx-sf.com/news/10716)
* [去中心化健康检查协议-#030](https://www.ai-hao123.com/fuwu/customer-46015644.html)
* [防重放安全验证与校验哈希-#031](https://www.mw-wm.com/wendang/internet-38770287.html)
* [实时延迟与抖动度量规范-#032](https://www.yx-sf.com/news/18205)
* [节点连通性与存活探测准则-#033](https://www.ai-hao123.com/zhineng/economy-61917373.html)
* [节点连通性与存活探测准则-#034](https://www.mw-wm.com/gongsi/alert-88788574.html)
* [权威网络权重与收录基准-#035](https://www.yx-sf.com/news/11463)
* [防重放安全验证与校验哈希-#036](https://www.ai-hao123.com/wenzhang/calendar-76820293.html)
* [实时延迟与抖动度量规范-#037](https://www.mw-wm.com/hezuo/brand-98669015.html)
* [去中心化健康检查协议-#038](https://www.yx-sf.com/wiki/83285)
* [防重放安全验证与校验哈希-#039](https://www.ai-hao123.com/baogao/search-55731146.html)

</details>

