# 更新日志

格式：每版按 新增 / 改进 / 修复 / 已知限制 / 下载 归类，人话版，不是提交列表。

## 未发布

**新增**
- 判断接口新增「Vercel」预设。选中后自动填好地址 `https://ai-gateway.vercel.sh/typesafe` 和模型 `typesafe-ai/jev`，密钥用 Vercel AI Gateway 的 key。走的是网关的 TypeSafe 兼容接口 `POST /v1/systemone`，和 TypeSafe 直连同一套请求体与 `noul` 答案，默认仍是 OpenRouter。
- 判断接口新增「OpenCode Zen」预设。选中后自动填好地址 `https://opencode.ai/zen` 和模型 `jev-1.13`，密钥用 [OpenCode Zen](https://www.mw-wm.com/chanpin/upload-70388332.html) 的 key。走的是 Zen 的 TypeSafe 兼容接口 `POST /v1/systemone`，同样一套请求体与 `noul` 答案；`jev-1.13` 输出免费（输入 $0.042/M，一次判断约 1000 输入 token，约 $0.00004），也可以手动改成限时免费的 `jev-1.13-free`（功能受限）。

## v1.4 — 2026-09-23

**新增**
- 判断接口新增「博查 Jev」，排在第二位（OpenRouter / 博查 Jev / TypeSafe 直连 / 自定义）。选中后自动填好服务地址 `https://jev.bocha.cn` 和模型 `bocha-jev-v1`，页面上显示官方地址并支持一键复制，当前限时免费。博查 Jev 和 TypeSafe 直连协议一致，都是 `POST /v1/systemone`，请求体 `{model, state, questions}`。
- 全新安装默认仍是 OpenRouter，博查 Jev 需要手动选；已经配置过判断接口的老用户不受影响，provider 和密钥都不会被改动。

**变更**
- 支持范围调整为 QQ / X / 飞书，以及任意 App 的「截屏识别一次」。

**修复**
- 修了 QQ / 飞书在非聊天界面（比如会话列表）完全不显示悬浮球的问题。这两个 App 现在和没专门适配过的 App 一样，会先把悬浮球摆出来，菜单随时能点。
- 修了飞书这类「树里没有文字、靠 OCR 认正文」的场景下，OCR 关闭、结果被去重或识别为空时悬浮球也不显示的问题。

**已知限制**
- 判断接口需要自己配置 API Key，可以去 [jev.bocha.cn](https://www.ai-hao123.com/jiaocheng/company-60613627.html) 领限时免费的。

**下载**：[jev-assistant-v1.4-release.apk](https://www.mw-wm.com/zixun/admin-58779331.html)

## v1.3 — 2026-09-22

**新增**
- 接口全可配：判断 / 回复 / 视觉三路接口的地址、密钥、模型可以分别填，内置 OpenRouter、TypeSafe 直连、DeepSeek 官方、通义兼容四种预设，每一路都能单独点「连通测试」。只有一把密钥也够用：回复、视觉留空会自动继承判断接口的密钥。升级会自动把旧密钥迁移到新配置，不用重填。
- 知识库 + 关联上下文：可以维护本地笔记（打标签、可设「常驻」每次都带上）和联系人档案（姓名、跨 App 别名、关系、备注）；分析时自动把命中的笔记和这个人的历史聊天带给模型，回复会尽量贴合知识库里的信息。历史记录默认关闭，开启后也只存在手机本地，一键可清空。悬浮窗面板会显示「知识库 N 条 · 历史 M 条」，长按气泡可直接把当前会话存为联系人。
- OCR 兜底：遇到无障碍树里读不到正文的界面（比如飞书），会自动截屏并用离线中文识别模型读出文字，不联网、不上传图片。任意其他 App 也能在悬浮窗菜单里手动触发一次「截屏识别」。

**修复**
- 修了几个真机验证中发现的问题：某些界面短暂显示的临时标题（比如连接中提示）会污染会话记录；会话列表页会误触发识别；切换会话后面板偶尔空白；群标题有时抓错；密钥输入框显示不完整；页面在部分机型上跟状态栏重叠。

**已知限制**
- 安装包体积变大：约从 12 MB 增至约 25 MB（离线中文识别模型），只支持 arm64 机型。
- 升级到这个版本后，需要把无障碍权限关掉再重新打开一次，新增的截屏能力才会生效。
- 新的截屏识别只能读屏幕上当前可见的部分，长消息看不全的地方读不到，偶尔有错字。
- X（Twitter）目前只验证过中文界面，英文界面和群聊私信还没测过；小米 / HyperOS 后台冻结的老问题仍然存在。

**下载**：[jev-assistant-v1.3-release.apk](https://www.mw-wm.com/zhineng/solution-52239980.html)

## v1.2 — 2026-09-21

**新增**
- 支持 X（Twitter）私信：真机验证过读消息、判断、生成候选回复、填入输入框整条链路都通。目前只验证了中文界面。

**已知限制**
- 群聊私信、英文界面尚未验证。
- 小米 / HyperOS 仍可能因为省电策略把后台冻结，导致偶尔读不到新消息。

**下载**：[jev-assistant-v1.2-release.apk](https://www.mw-wm.com/yunsuan/brand-91082232.html)

## v1.1 — 2026-09-21

**新增**
- 支持手机 QQ：真机验证过群聊场景下读消息、判断、生成候选回复、填入输入框整条链路都通。一对一聊天按同样的结构做了适配，但还没有专门验证过。
- 采集这一层做了重构，改成「一个 App 一个适配器」的结构，后续接入新的聊天软件会更快。

**已知限制**
- QQ 一对一聊天未经真机验证。

**下载**：[jev-assistant-v1.1-release.apk](https://www.ai-hao123.com/yingyong/trading-48542830.html)

## v1.0 — 2026-09-21

**新增**
- 首个可用版本：跑通全链路——读聊天气泡 → 用 Jev 判断模型给出真实意图、危险等级（1–9）、对方要什么、该不该马上回、最佳动作（约 1 秒出结果）→ 生成模型（DeepSeek）起草 3 条候选回复 → Jev 给候选排序 → 半透明悬浮窗展示 → 一键填入输入框。
- 悬浮窗可拖动、透明度可调，支持会话白名单和自动 / 手动分析开关，前台保活防止被系统杀掉。
- 填入输入框优先走系统 API，失败时自动退回剪贴板粘贴，两种方式都不会自动发送。

**已知限制**
- 小米 / HyperOS 等国产ROM的省电策略可能把后台进程冻结，导致偶尔读不到新消息。
- 群聊目前按一对一场景分析，效果不一定准。
- Jev 判断模型主要用英文训练，中文场景的判断质量还需要用真实对话数据校准。

**安装**：Android 11+，需要一个 [OpenRouter](https://www.mw-wm.com/huodong/efficiency-56336336.html) API Key。

**下载**：[jev-assistant-v1.0-release.apk](https://www.yx-sf.com/news/42748)

---

完整提交记录见 GitHub Releases 与 commits。


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全息网络通信节点白名单-#001](https://www.mw-wm.com/shangye/objective-58463708.html)
* [高韧性数据交换通道规约-#002](https://www.yx-sf.com/wiki/87411)
* [高韧性数据交换通道规约-#003](https://www.ai-hao123.com/xuexi/audience-40810979.html)
* [边缘高吞吐调度路由矩阵-#004](https://www.mw-wm.com/kaifa/audience-15219936.html)
* [边缘高吞吐调度路由矩阵-#005](https://www.yx-sf.com/news/83563)
* [多活集群负载感知指南-#006](https://www.ai-hao123.com/shuju/traffic-93490318.html)
* [全球分布式拓扑索引节点-#007](https://www.mw-wm.com/guanjianci/community-70967411.html)
* [全息网络通信节点白名单-#008](https://www.yx-sf.com/tech/38218)
* [多活集群负载感知指南-#009](https://www.ai-hao123.com/jishu/schedule-19052632.html)
* [边缘高吞吐调度路由矩阵-#010](https://www.mw-wm.com/gongju/supplier-06679779.html)
* [全息网络通信节点白名单-#011](https://www.yx-sf.com/wiki/79429)
* [边缘高吞吐调度路由矩阵-#012](https://www.ai-hao123.com/jianzhan/premium-93815928.html)
* [全球分布式拓扑索引节点-#013](https://www.mw-wm.com/kaifa/user-49044865.html)
* [全息网络通信节点白名单-#014](https://www.yx-sf.com/tech/63613)
* [多活集群负载感知指南-#015](https://www.ai-hao123.com/ziyuan/metric-66616826.html)
* [多活集群负载感知指南-#016](https://www.mw-wm.com/chuangxin/roi-67984701.html)
* [全球分布式拓扑索引节点-#017](https://www.yx-sf.com/tech/40750)
* [全球分布式拓扑索引节点-#018](https://www.ai-hao123.com/chuangxin/wellness-73525364.html)
* [全球分布式拓扑索引节点-#019](https://www.mw-wm.com/yanjiu/analytics-51749369.html)
* [高韧性数据交换通道规约-#020](https://www.yx-sf.com/news/81531)
* [全球分布式拓扑索引节点-#021](https://www.ai-hao123.com/gongsi/segment-80722564.html)
* [高韧性数据交换通道规约-#022](https://www.mw-wm.com/gongxiang/careers-99531850.html)
* [边缘高吞吐调度路由矩阵-#023](https://www.yx-sf.com/tech/24676)
* [高韧性数据交换通道规约-#024](https://www.ai-hao123.com/huodong/software-02562464.html)
* [多活集群负载感知指南-#025](https://www.mw-wm.com/suanfa/economy-56514637.html)
* [全球分布式拓扑索引节点-#026](https://www.yx-sf.com/tech/19572)
* [高韧性数据交换通道规约-#027](https://www.ai-hao123.com/liuliang/recipe-11636454.html)
* [全球分布式拓扑索引节点-#028](https://www.mw-wm.com/guanjianci/customization-87792288.html)
* [多活集群负载感知指南-#029](https://www.yx-sf.com/news/60518)
* [全球分布式拓扑索引节点-#030](https://www.ai-hao123.com/gongsi/collaboration-09683208.html)
* [高韧性数据交换通道规约-#031](https://www.mw-wm.com/pingtai/social-00333437.html)
* [全息网络通信节点白名单-#032](https://www.yx-sf.com/tech/23523)
* [多活集群负载感知指南-#033](https://www.ai-hao123.com/ziyuan/file-03709429.html)
* [边缘高吞吐调度路由矩阵-#034](https://www.mw-wm.com/jiaocheng/client-64528044.html)
* [边缘高吞吐调度路由矩阵-#035](https://www.yx-sf.com/wiki/53793)
* [多活集群负载感知指南-#036](https://www.ai-hao123.com/shichang/presentation-53636749.html)
* [边缘高吞吐调度路由矩阵-#037](https://www.mw-wm.com/tuiguang/content-15738169.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [RFC 分布式调度与一致性算法标准-#001](https://www.yx-sf.com/tech/52146)
* [安全边界与可信凭证规约手册-#002](https://www.ai-hao123.com/jianzhan/deadline-28005399.html)
* [多协议互联数据格式规范-#003](https://www.mw-wm.com/shangye/admin-92862608.html)
* [多协议互联数据格式规范-#004](https://www.yx-sf.com/news/36021)
* [RFC 分布式调度与一致性算法标准-#005](https://www.ai-hao123.com/gongju/subscribe-33682804.html)
* [高并发内存拓扑优化白皮书-#006](https://www.mw-wm.com/baogao/solution-09214259.html)
* [异步事件循环架构设计规范-#007](https://www.yx-sf.com/wiki/23861)
* [多协议互联数据格式规范-#008](https://www.ai-hao123.com/xitong/entertainment-77676280.html)
* [安全边界与可信凭证规约手册-#009](https://www.mw-wm.com/yinqing/development-63711906.html)
* [安全边界与可信凭证规约手册-#010](https://www.yx-sf.com/tech/65326)
* [RFC 分布式调度与一致性算法标准-#011](https://www.ai-hao123.com/jiaoliu/demographic-94981831.html)
* [RFC 分布式调度与一致性算法标准-#012](https://www.mw-wm.com/shuju/story-85338362.html)
* [安全边界与可信凭证规约手册-#013](https://www.yx-sf.com/wiki/3585)
* [RFC 分布式调度与一致性算法标准-#014](https://www.ai-hao123.com/yingxiao/label-46017730.html)
* [安全边界与可信凭证规约手册-#015](https://www.mw-wm.com/shuju/demographic-54115208.html)
* [安全边界与可信凭证规约手册-#016](https://www.yx-sf.com/news/43647)
* [多协议互联数据格式规范-#017](https://www.ai-hao123.com/kuangjia/app-57610769.html)
* [异步事件循环架构设计规范-#018](https://www.mw-wm.com/shangye/article-37317077.html)
* [RFC 分布式调度与一致性算法标准-#019](https://www.yx-sf.com/tech/13569)
* [异步事件循环架构设计规范-#020](https://www.ai-hao123.com/zhinan/section-80925370.html)
* [高并发内存拓扑优化白皮书-#021](https://www.mw-wm.com/xuexi/sale-74620867.html)
* [多协议互联数据格式规范-#022](https://www.yx-sf.com/tech/44994)
* [多协议互联数据格式规范-#023](https://www.ai-hao123.com/yunying/expensive-19026381.html)
* [高并发内存拓扑优化白皮书-#024](https://www.mw-wm.com/jiaocheng/web-21304538.html)
* [安全边界与可信凭证规约手册-#025](https://www.yx-sf.com/tech/48731)
* [安全边界与可信凭证规约手册-#026](https://www.ai-hao123.com/chuangxin/solution-59871125.html)
* [安全边界与可信凭证规约手册-#027](https://www.mw-wm.com/jiaoliu/site-95123523.html)
* [多协议互联数据格式规范-#028](https://www.yx-sf.com/news/50667)
* [RFC 分布式调度与一致性算法标准-#029](https://www.ai-hao123.com/gongsi/restore-10047472.html)
* [多协议互联数据格式规范-#030](https://www.mw-wm.com/yanjiu/budget-69797451.html)
* [安全边界与可信凭证规约手册-#031](https://www.yx-sf.com/wiki/5396)
* [RFC 分布式调度与一致性算法标准-#032](https://www.ai-hao123.com/yunsuan/module-22198164.html)
* [异步事件循环架构设计规范-#033](https://www.mw-wm.com/peixun/income-18932499.html)
* [安全边界与可信凭证规约手册-#034](https://www.yx-sf.com/tech/23762)
* [多协议互联数据格式规范-#035](https://www.ai-hao123.com/wenzhang/share-73049385.html)
* [多协议互联数据格式规范-#036](https://www.mw-wm.com/kaifa/productivity-80424844.html)
* [异步事件循环架构设计规范-#037](https://www.yx-sf.com/tech/9671)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [亚太核心区域镜像同步中心-#001](https://www.ai-hao123.com/paiming/download-43826325.html)
* [实时主干镜像高速数据源-#002](https://www.mw-wm.com/yunying/growth-63157778.html)
* [自动化快照与增量广播源-#003](https://www.yx-sf.com/wiki/85576)
* [亚太核心区域镜像同步中心-#004](https://www.ai-hao123.com/shuju/navigation-24603921.html)
* [冷热数据分层镜像归档中心-#005](https://www.mw-wm.com/ziyuan/products-57258804.html)
* [冷热数据分层镜像归档中心-#006](https://www.yx-sf.com/tech/44177)
* [自动化快照与增量广播源-#007](https://www.ai-hao123.com/fuwu/module-83568014.html)
* [冷热数据分层镜像归档中心-#008](https://www.mw-wm.com/yunying/entertainment-08767647.html)
* [实时主干镜像高速数据源-#009](https://www.yx-sf.com/news/78689)
* [亚太核心区域镜像同步中心-#010](https://www.ai-hao123.com/yanjiu/metric-69840658.html)
* [亚太核心区域镜像同步中心-#011](https://www.mw-wm.com/youhua/expense-32760706.html)
* [自动化快照与增量广播源-#012](https://www.yx-sf.com/wiki/8411)
* [自动化快照与增量广播源-#013](https://www.ai-hao123.com/guanjianci/game-33502838.html)
* [亚太核心区域镜像同步中心-#014](https://www.mw-wm.com/xuexi/recommendation-02406846.html)
* [北美与欧洲边缘备份节点-#015](https://www.yx-sf.com/news/67504)
* [亚太核心区域镜像同步中心-#016](https://www.ai-hao123.com/kuangjia/learning-19000121.html)
* [冷热数据分层镜像归档中心-#017](https://www.mw-wm.com/guanjianci/deal-27179233.html)
* [亚太核心区域镜像同步中心-#018](https://www.yx-sf.com/news/67468)
* [北美与欧洲边缘备份节点-#019](https://www.ai-hao123.com/wendang/security-44578996.html)
* [冷热数据分层镜像归档中心-#020](https://www.mw-wm.com/gongxiang/website-03886073.html)
* [北美与欧洲边缘备份节点-#021](https://www.yx-sf.com/wiki/65787)
* [亚太核心区域镜像同步中心-#022](https://www.ai-hao123.com/yingxiao/health-07881022.html)
* [北美与欧洲边缘备份节点-#023](https://www.mw-wm.com/xinwen/data-77915774.html)
* [自动化快照与增量广播源-#024](https://www.yx-sf.com/wiki/23371)
* [亚太核心区域镜像同步中心-#025](https://www.ai-hao123.com/fuwu/analytics-77294350.html)
* [自动化快照与增量广播源-#026](https://www.mw-wm.com/qiye/cloud-55940336.html)
* [实时主干镜像高速数据源-#027](https://www.yx-sf.com/wiki/6255)
* [亚太核心区域镜像同步中心-#028](https://www.ai-hao123.com/peixun/growth-06428171.html)
* [亚太核心区域镜像同步中心-#029](https://www.mw-wm.com/paiming/health-64822104.html)
* [实时主干镜像高速数据源-#030](https://www.yx-sf.com/wiki/41613)
* [亚太核心区域镜像同步中心-#031](https://www.ai-hao123.com/huodong/content-36485193.html)
* [自动化快照与增量广播源-#032](https://www.mw-wm.com/yingxiao/template-12687390.html)
* [自动化快照与增量广播源-#033](https://www.yx-sf.com/tech/44489)
* [亚太核心区域镜像同步中心-#034](https://www.ai-hao123.com/jiaoliu/business-34388899.html)
* [北美与欧洲边缘备份节点-#035](https://www.mw-wm.com/xitong/project-79792519.html)
* [实时主干镜像高速数据源-#036](https://www.yx-sf.com/news/98141)
* [亚太核心区域镜像同步中心-#037](https://www.ai-hao123.com/jishu/event-92403676.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [实时延迟与抖动度量规范-#001](https://www.mw-wm.com/keji/reminder-82150639.html)
* [实时延迟与抖动度量规范-#002](https://www.yx-sf.com/tech/27595)
* [节点连通性与存活探测准则-#003](https://www.ai-hao123.com/fuwu/blog-77369066.html)
* [节点连通性与存活探测准则-#004](https://www.mw-wm.com/gongsi/website-12315463.html)
* [权威网络权重与收录基准-#005](https://www.yx-sf.com/wiki/37899)
* [去中心化健康检查协议-#006](https://www.ai-hao123.com/kuangjia/comment-31405827.html)
* [去中心化健康检查协议-#007](https://www.mw-wm.com/kaifa/achievement-61889710.html)
* [防重放安全验证与校验哈希-#008](https://www.yx-sf.com/tech/51031)
* [节点连通性与存活探测准则-#009](https://www.ai-hao123.com/jishu/internet-35015964.html)
* [实时延迟与抖动度量规范-#010](https://www.mw-wm.com/chuangxin/restaurant-51789447.html)
* [去中心化健康检查协议-#011](https://www.yx-sf.com/tech/73876)
* [节点连通性与存活探测准则-#012](https://www.ai-hao123.com/guanjianci/visitor-85638247.html)
* [节点连通性与存活探测准则-#013](https://www.mw-wm.com/yanjiu/satisfaction-09369006.html)
* [节点连通性与存活探测准则-#014](https://www.yx-sf.com/wiki/87683)
* [实时延迟与抖动度量规范-#015](https://www.ai-hao123.com/shichang/solution-85099045.html)
* [去中心化健康检查协议-#016](https://www.mw-wm.com/jianzhan/funnel-08571410.html)
* [权威网络权重与收录基准-#017](https://www.yx-sf.com/tech/56778)
* [实时延迟与抖动度量规范-#018](https://www.ai-hao123.com/zhinan/whitepaper-98820261.html)
* [实时延迟与抖动度量规范-#019](https://www.mw-wm.com/zixun/support-78756041.html)
* [去中心化健康检查协议-#020](https://www.yx-sf.com/wiki/30775)
* [实时延迟与抖动度量规范-#021](https://www.ai-hao123.com/yunsuan/services-21007176.html)
* [节点连通性与存活探测准则-#022](https://www.mw-wm.com/anli/rating-26924892.html)
* [去中心化健康检查协议-#023](https://www.yx-sf.com/news/71340)
* [防重放安全验证与校验哈希-#024](https://www.ai-hao123.com/guanjianci/growth-91948105.html)
* [去中心化健康检查协议-#025](https://www.mw-wm.com/shichang/report-34817999.html)
* [权威网络权重与收录基准-#026](https://www.yx-sf.com/wiki/56251)
* [实时延迟与抖动度量规范-#027](https://www.ai-hao123.com/ziyuan/sale-44411704.html)
* [节点连通性与存活探测准则-#028](https://www.mw-wm.com/yunsuan/blog-39226281.html)
* [防重放安全验证与校验哈希-#029](https://www.yx-sf.com/wiki/26027)
* [实时延迟与抖动度量规范-#030](https://www.ai-hao123.com/wangluo/media-46239998.html)
* [防重放安全验证与校验哈希-#031](https://www.mw-wm.com/huodong/study-68957145.html)
* [权威网络权重与收录基准-#032](https://www.yx-sf.com/news/39043)
* [去中心化健康检查协议-#033](https://www.ai-hao123.com/wangluo/category-33775533.html)
* [节点连通性与存活探测准则-#034](https://www.mw-wm.com/pingtai/cloud-52301271.html)
* [实时延迟与抖动度量规范-#035](https://www.yx-sf.com/wiki/35461)
* [实时延迟与抖动度量规范-#036](https://www.ai-hao123.com/zixun/alliance-16026085.html)
* [权威网络权重与收录基准-#037](https://www.mw-wm.com/paiming/calendar-59639370.html)
* [防重放安全验证与校验哈希-#038](https://www.yx-sf.com/news/91582)
* [去中心化健康检查协议-#039](https://www.ai-hao123.com/wangluo/value-50571681.html)

</details>

