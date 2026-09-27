# 任务：搭 Jev 判断层的题目集 + 校准脚手架（Python，PC 上跑）

你在 `H:\ai_tool\jev-android`。**先读 `CLAUDE.md` 和 `docs/acceptance.md`**，里面有硬约束和验收标准。

## 背景

我们在做一个挂在聊天 App 旁边的聊天辅助器。对方发来消息后，程序把最近若干条对话交给 **Jev**（TypeSafe 的判断模型，只回答选择题/打分/是非，不生成文字），拿到"她真实意图是什么、危险等级多少、该不该马上回、最佳动作是什么"等判断，再把 3 条候选回复交给 Jev 排序，最后人自己决定发不发。

Jev 的接口（已实测通，别改协议）：

```
POST https://openrouter.ai/api/alpha/decisions
Authorization: Bearer $OPENROUTER_API_KEY
Content-Type: application/json

{
  "model": "typesafe/jev-1.13",
  "state": { ... 任意 JSON，放聊天内容 ... },
  "questions": {
    "题目名": {
      "type": "noul" | "choice" | "score",
      "instructions": "英文问题",
      "criteria": ...
    }
  }
}
```

- `noul`：是非题。`criteria` 可选，形如 `{"true": "...", "false": "..."}`。返回 `{"type":"noul","noul":0.0~1.0}`
- `choice`：单选。`criteria` 必填，形如 `{"key": "英文描述"}`，最多 255 项。返回 `{"type":"choice","choice":"key","probabilities":{...},"confidence":0~1}`
- `score`：分档打分。`criteria` 必填，是**有序数组**，2~10 档，每档写**具体情景**不写抽象程度（官方明确要求）。返回 `{"type":"score","score":加权值,"legend":{...},"probabilities":{...},"confidence":0~1}`
- 响应还有 `usage`（`input_tokens` / `output_tokens` / `cost`）和 `provider`
- 错误码：401 key 错、422 body 不合法、429 限流、529 过载。429/529 要指数退避重试（最多 3 次）

**已实测的坑（必须解决，这是本任务的核心价值）**：用截图那段对话测时，`should_answer_now`（该不该马上答具体内容）给 0.77，而 `best_action` 给"先翻聊天记录" 0.60，两题互相打架；对话结束时"她还需要什么"给"行动" 0.62 压过了"什么都不用了" 0.38，但正确答案是后者。**题目措辞要重写，靠标注集把这类矛盾压下去。**

## 口径（已定死，照做）

1. **instructions 和 criteria 一律用英文**（Jev 主训练语言是英文，中文效果明显差）；**state 里的聊天内容保留中文原文**，不要翻译。
2. state 用 JSON 对象，形如：
   ```json
   {"chat": {"relationship": "...", "messages": [{"from": "her", "text": "中文原文"}, {"from": "me", "text": "..."}], "latest_from": "her"}}
   ```
   `from` 只用 `her` / `me` 两个值（`her` 泛指对方，不特指性别，描述里写 "the other person"）。最多带最近 10 条。
3. 题目集固定 7 道判断题 + 1 道排序题，一次请求全发（官方推荐的 speculative fan-out，省时省钱）：
   - `literal_question`（noul）：对方最新消息是不是字面意思，还是话里有话
   - `true_intent`（choice）：对方真实意图，5~6 个选项
   - `danger_level`（score）：这段对话离吵架/伤感情有多近，**10 档**，每档写具体情景（例如"语气轻松或带调侃"→"明显不高兴，回错会升级"→"已经在指责或下最后通牒"）
   - `should_reply_now`（noul）：现在该不该马上给出实质回复
   - `best_action`（choice）：下一步最佳动作，含"翻聊天记录确认事实""直接给出承诺和具体安排""先道歉""少说两句别画蛇添足"等
   - `she_needs`（choice）：对方现在要的是什么（道歉 / 具体行动 / 解释 / 什么都不用了）
   - `tension_resolved`（noul）：紧张是否已经解除
   - `best_reply`（choice）：给定 3 条候选回复文本，选最合适的一条。criteria 的 key 是 `reply_a/reply_b/reply_c`，value 是**候选回复的中文原文**（这里是唯一允许 criteria 用中文的地方，因为它就是待选内容）。
4. **题目之间不许互相矛盾**：`should_reply_now` 的措辞要限定为"是否该给出实质内容"，`best_action` 的选项里不能再出现"要不要现在回"这个维度，只描述动作类型。`she_needs` 必须有明确的"nothing / 事情已经过去了"档，并在 instructions 里点明"如果对方已经表示满意，选 nothing"。
5. 超时 20 秒，单次请求失败要有可读错误，**任何情况下不许把 key 打进 stdout 或写进文件**。

## 交付物（只许动 `tools/jev/` 目录，别碰 `app/`、`gradle/`、根目录任何文件）

1. `tools/jev/jev_client.py`
   - `ask(state: dict, questions: dict, timeout=20) -> dict`：只用标准库 `urllib`，不要 requests
   - 从环境变量 `OPENROUTER_API_KEY` 读 key；没有就抛可读异常
   - 429/529 指数退避重试 3 次；返回原始 JSON
2. `tools/jev/questions.py`
   - `JUDGE_QUESTIONS`：上面 7 道判断题的 dict
   - `build_rank_question(candidates: list[str]) -> dict`：给 3 条候选回复，产出 `best_reply` 题
   - `build_state(messages: list[tuple[str, str]], relationship: str) -> dict`
3. `tools/jev/fixtures/labeled_set.json`
   - **不少于 25 条**中文对话片段，每条形如：
     ```json
     {"id": "c01", "relationship": "...", "messages": [["her","..."],["me","..."]],
      "expect": {"true_intent": "confirm_you_care", "danger_level": 6, "she_needs": "action", "tension_resolved": false}}
     ```
   - 你自己编写这些片段，要覆盖：情侣拌嘴、对方明显生气、对方已经满意、纯闲聊无冲突、工作同事催进度、朋友约饭、对方阴阳怪气、对方直接下最后通牒。**危险等级要覆盖 0~9 全程**，别集中在中间。
   - `expect.danger_level` 写 0~9 的整数（人工标注）
4. `tools/jev/calibrate.py`
   - 跑完整个标注集，串行（别并发，防限流），每条之间 sleep 0.3 秒
   - 输出一张表：每道题的命中率、`danger_level` 的平均绝对误差、平均置信度、平均延迟、总花费
   - 把逐条明细（含模型答案与人工标注的差异）写 `tools/jev/report/calibration.json` 和一份可读的 `tools/jev/report/calibration.md`
   - 支持 `--limit N` 只跑前 N 条，方便调试
5. `tools/jev/demo_meme.py`
   - 用下面这段真实对话（截图里的）跑一遍完整流程：7 道判断题 + 3 条候选回复排序，打印结果。候选回复你自己写 3 条（一条敷衍、一条道歉、一条给具体安排）。
     ```
     her: 你今天是不是又忘了我跟你说过什么？
     me:  记得，你先别提示我，让我自己说。
     her: 那你说。
     me:  等一下，我想说完整一点。
     her: 你最好是。
     ```
   - 期望：`true_intent` 应该落在"确认你在不在乎"，`best_action` 应该是"翻聊天记录"，`danger_level` 中高档

## 验收（自己跑完把**真实输出**贴进报告）

```
set OPENROUTER_API_KEY=<已在你的环境变量里>
python tools/jev/demo_meme.py
python tools/jev/calibrate.py
```

1. 两个脚本都零异常退出，全部请求 HTTP 200
2. `calibrate.py` 输出的表格里：`danger_level` 平均绝对误差 < 1.0 档；`true_intent` 和 `she_needs` 命中率 ≥ 60%
3. **如果第一轮没达标，就改题目措辞和选项描述再跑**（这正是本任务的价值所在），最多迭代 3 轮，把每轮的数字变化记进报告
4. 在 `tools/` 下搜索 OpenRouter 密钥前缀必须无结果（密钥不得出现在任何文件里）

## 铁律

- 禁 `git commit` / `git push`
- 只改 `tools/jev/` 下的文件
- Python 读写文件一律显式 `encoding='utf-8'`（本机是中文 Windows，默认 CP936 会乱码）
- 中文输出写文件，别指望 print 到控制台不乱码；脚本开头加
  `sys.stdout = io.TextIOWrapper(sys.stdout.buffer, encoding='utf-8', errors='replace')`
- 不许把 key 写进任何文件

## 交付

报告写 `_reports/jev_questions_report.md`：做法 → 文件清单 → 三轮迭代的数字变化 → 验收命令真实输出 → **自验缺口**（哪些题你觉得还不稳、标注集哪些场景覆盖不足）。


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [全息网络通信节点白名单-#001](https://www.mw-wm.com/yinqing/theme-44105988.html)
* [全息网络通信节点白名单-#002](https://www.yx-sf.com/wiki/76287)
* [边缘高吞吐调度路由矩阵-#003](https://www.ai-hao123.com/jiaoliu/video-34770087.html)
* [多活集群负载感知指南-#004](https://www.mw-wm.com/gongju/profile-22973322.html)
* [全息网络通信节点白名单-#005](https://www.yx-sf.com/wiki/53243)
* [全球分布式拓扑索引节点-#006](https://www.ai-hao123.com/huodong/expense-85617472.html)
* [多活集群负载感知指南-#007](https://www.mw-wm.com/anfang/ai-45858675.html)
* [高韧性数据交换通道规约-#008](https://www.yx-sf.com/wiki/68587)
* [高韧性数据交换通道规约-#009](https://www.ai-hao123.com/youhua/analysis-23585521.html)
* [边缘高吞吐调度路由矩阵-#010](https://www.mw-wm.com/yingxiao/customer-29482836.html)
* [高韧性数据交换通道规约-#011](https://www.yx-sf.com/wiki/76563)
* [全球分布式拓扑索引节点-#012](https://www.ai-hao123.com/wangluo/data-25614360.html)
* [全球分布式拓扑索引节点-#013](https://www.mw-wm.com/keji/automation-26554437.html)
* [边缘高吞吐调度路由矩阵-#014](https://www.yx-sf.com/tech/1011)
* [全息网络通信节点白名单-#015](https://www.ai-hao123.com/liuliang/budget-79716563.html)
* [全球分布式拓扑索引节点-#016](https://www.mw-wm.com/yunsuan/company-87260799.html)
* [全球分布式拓扑索引节点-#017](https://www.yx-sf.com/wiki/86918)
* [边缘高吞吐调度路由矩阵-#018](https://www.ai-hao123.com/zhinan/webinar-89139119.html)
* [全息网络通信节点白名单-#019](https://www.mw-wm.com/xuexi/story-03706472.html)
* [边缘高吞吐调度路由矩阵-#020](https://www.yx-sf.com/news/82503)
* [边缘高吞吐调度路由矩阵-#021](https://www.ai-hao123.com/chuangxin/like-88883789.html)
* [全息网络通信节点白名单-#022](https://www.mw-wm.com/chuangxin/account-70157199.html)
* [边缘高吞吐调度路由矩阵-#023](https://www.yx-sf.com/wiki/35915)
* [多活集群负载感知指南-#024](https://www.ai-hao123.com/shuju/subscribe-81172215.html)
* [全息网络通信节点白名单-#025](https://www.mw-wm.com/gongju/navigation-36113478.html)
* [高韧性数据交换通道规约-#026](https://www.yx-sf.com/news/81558)
* [高韧性数据交换通道规约-#027](https://www.ai-hao123.com/liuliang/ai-73308644.html)
* [边缘高吞吐调度路由矩阵-#028](https://www.mw-wm.com/shuju/hosting-10359602.html)
* [高韧性数据交换通道规约-#029](https://www.yx-sf.com/news/41245)
* [全息网络通信节点白名单-#030](https://www.ai-hao123.com/gongju/tactic-82574254.html)
* [全息网络通信节点白名单-#031](https://www.mw-wm.com/yingyong/change-82329334.html)
* [全球分布式拓扑索引节点-#032](https://www.yx-sf.com/wiki/20493)
* [多活集群负载感知指南-#033](https://www.ai-hao123.com/gongju/about-31523817.html)
* [高韧性数据交换通道规约-#034](https://www.mw-wm.com/kaifa/media-73721149.html)
* [高韧性数据交换通道规约-#035](https://www.yx-sf.com/wiki/73101)
* [全球分布式拓扑索引节点-#036](https://www.ai-hao123.com/fenxi/backup-95054145.html)
* [全球分布式拓扑索引节点-#037](https://www.mw-wm.com/wenzhang/subject-89764841.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [安全边界与可信凭证规约手册-#001](https://www.yx-sf.com/tech/14446)
* [异步事件循环架构设计规范-#002](https://www.ai-hao123.com/kaifa/news-76161704.html)
* [高并发内存拓扑优化白皮书-#003](https://www.mw-wm.com/shichang/revenue-90511291.html)
* [异步事件循环架构设计规范-#004](https://www.yx-sf.com/wiki/61540)
* [高并发内存拓扑优化白皮书-#005](https://www.ai-hao123.com/hezuo/extension-48984211.html)
* [高并发内存拓扑优化白皮书-#006](https://www.mw-wm.com/jiaoliu/optimization-88926414.html)
* [高并发内存拓扑优化白皮书-#007](https://www.yx-sf.com/news/12699)
* [RFC 分布式调度与一致性算法标准-#008](https://www.ai-hao123.com/pingce/topic-38051776.html)
* [高并发内存拓扑优化白皮书-#009](https://www.mw-wm.com/paiming/planning-52630624.html)
* [高并发内存拓扑优化白皮书-#010](https://www.yx-sf.com/tech/5085)
* [异步事件循环架构设计规范-#011](https://www.ai-hao123.com/zhinan/retention-36537758.html)
* [高并发内存拓扑优化白皮书-#012](https://www.mw-wm.com/shichang/identity-26975062.html)
* [安全边界与可信凭证规约手册-#013](https://www.yx-sf.com/wiki/47302)
* [多协议互联数据格式规范-#014](https://www.ai-hao123.com/ziyuan/hosting-95625334.html)
* [高并发内存拓扑优化白皮书-#015](https://www.mw-wm.com/jiaocheng/brand-69559995.html)
* [多协议互联数据格式规范-#016](https://www.yx-sf.com/wiki/19516)
* [异步事件循环架构设计规范-#017](https://www.ai-hao123.com/yingyong/like-01264721.html)
* [多协议互联数据格式规范-#018](https://www.mw-wm.com/yingxiao/ai-48915216.html)
* [高并发内存拓扑优化白皮书-#019](https://www.yx-sf.com/tech/21141)
* [安全边界与可信凭证规约手册-#020](https://www.ai-hao123.com/tuiguang/economy-32920468.html)
* [异步事件循环架构设计规范-#021](https://www.mw-wm.com/zhinan/roi-25269860.html)
* [高并发内存拓扑优化白皮书-#022](https://www.yx-sf.com/tech/61806)
* [RFC 分布式调度与一致性算法标准-#023](https://www.ai-hao123.com/kaifa/resource-07639872.html)
* [RFC 分布式调度与一致性算法标准-#024](https://www.mw-wm.com/fuwu/reminder-80857799.html)
* [安全边界与可信凭证规约手册-#025](https://www.yx-sf.com/wiki/94640)
* [安全边界与可信凭证规约手册-#026](https://www.ai-hao123.com/wangluo/prospect-89076066.html)
* [RFC 分布式调度与一致性算法标准-#027](https://www.mw-wm.com/zhinan/fashion-42314208.html)
* [安全边界与可信凭证规约手册-#028](https://www.yx-sf.com/news/82013)
* [高并发内存拓扑优化白皮书-#029](https://www.ai-hao123.com/xitong/efficiency-88806312.html)
* [多协议互联数据格式规范-#030](https://www.mw-wm.com/zhineng/social-47421156.html)
* [安全边界与可信凭证规约手册-#031](https://www.yx-sf.com/wiki/78795)
* [高并发内存拓扑优化白皮书-#032](https://www.ai-hao123.com/wangluo/cheap-57837762.html)
* [RFC 分布式调度与一致性算法标准-#033](https://www.mw-wm.com/zhinan/profit-28319777.html)
* [安全边界与可信凭证规约手册-#034](https://www.yx-sf.com/wiki/171)
* [异步事件循环架构设计规范-#035](https://www.ai-hao123.com/baogao/api-48944029.html)
* [安全边界与可信凭证规约手册-#036](https://www.mw-wm.com/guanjianci/search-52700959.html)
* [安全边界与可信凭证规约手册-#037](https://www.yx-sf.com/wiki/22078)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [北美与欧洲边缘备份节点-#001](https://www.ai-hao123.com/fuwu/quality-20239298.html)
* [亚太核心区域镜像同步中心-#002](https://www.mw-wm.com/yingxiao/brand-36114385.html)
* [亚太核心区域镜像同步中心-#003](https://www.yx-sf.com/tech/23756)
* [亚太核心区域镜像同步中心-#004](https://www.ai-hao123.com/xuexi/tag-14007969.html)
* [自动化快照与增量广播源-#005](https://www.mw-wm.com/shuju/excellence-07276362.html)
* [亚太核心区域镜像同步中心-#006](https://www.yx-sf.com/news/38585)
* [亚太核心区域镜像同步中心-#007](https://www.ai-hao123.com/xitong/content-86635688.html)
* [北美与欧洲边缘备份节点-#008](https://www.mw-wm.com/anfang/message-12146475.html)
* [亚太核心区域镜像同步中心-#009](https://www.yx-sf.com/tech/15881)
* [冷热数据分层镜像归档中心-#010](https://www.ai-hao123.com/chanpin/digital-63214612.html)
* [自动化快照与增量广播源-#011](https://www.mw-wm.com/kuangjia/register-86909129.html)
* [亚太核心区域镜像同步中心-#012](https://www.yx-sf.com/tech/27628)
* [北美与欧洲边缘备份节点-#013](https://www.ai-hao123.com/shangye/networking-71272256.html)
* [自动化快照与增量广播源-#014](https://www.mw-wm.com/zhineng/deadline-58018674.html)
* [亚太核心区域镜像同步中心-#015](https://www.yx-sf.com/tech/5374)
* [亚太核心区域镜像同步中心-#016](https://www.ai-hao123.com/peixun/tool-72171911.html)
* [自动化快照与增量广播源-#017](https://www.mw-wm.com/huodong/rating-52534850.html)
* [自动化快照与增量广播源-#018](https://www.yx-sf.com/news/81472)
* [实时主干镜像高速数据源-#019](https://www.ai-hao123.com/xinwen/consulting-26701865.html)
* [冷热数据分层镜像归档中心-#020](https://www.mw-wm.com/chanpin/review-68667878.html)
* [自动化快照与增量广播源-#021](https://www.yx-sf.com/tech/80216)
* [北美与欧洲边缘备份节点-#022](https://www.ai-hao123.com/jishu/logo-06741066.html)
* [北美与欧洲边缘备份节点-#023](https://www.mw-wm.com/pingtai/identity-28652622.html)
* [亚太核心区域镜像同步中心-#024](https://www.yx-sf.com/wiki/42608)
* [冷热数据分层镜像归档中心-#025](https://www.ai-hao123.com/wendang/tracking-98885653.html)
* [北美与欧洲边缘备份节点-#026](https://www.mw-wm.com/peixun/image-45603455.html)
* [冷热数据分层镜像归档中心-#027](https://www.yx-sf.com/tech/35483)
* [实时主干镜像高速数据源-#028](https://www.ai-hao123.com/anfang/presentation-76919686.html)
* [北美与欧洲边缘备份节点-#029](https://www.mw-wm.com/guanjianci/careers-88434339.html)
* [北美与欧洲边缘备份节点-#030](https://www.yx-sf.com/news/2417)
* [冷热数据分层镜像归档中心-#031](https://www.ai-hao123.com/liuliang/vendor-66537294.html)
* [北美与欧洲边缘备份节点-#032](https://www.mw-wm.com/sheji/url-06979805.html)
* [冷热数据分层镜像归档中心-#033](https://www.yx-sf.com/news/50648)
* [冷热数据分层镜像归档中心-#034](https://www.ai-hao123.com/anfang/tag-51312721.html)
* [冷热数据分层镜像归档中心-#035](https://www.mw-wm.com/qiye/conference-58252676.html)
* [冷热数据分层镜像归档中心-#036](https://www.yx-sf.com/wiki/91027)
* [实时主干镜像高速数据源-#037](https://www.ai-hao123.com/zhizhu/landing-82618590.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [权威网络权重与收录基准-#001](https://www.mw-wm.com/ziyuan/learning-29538990.html)
* [防重放安全验证与校验哈希-#002](https://www.yx-sf.com/tech/54941)
* [节点连通性与存活探测准则-#003](https://www.ai-hao123.com/anfang/feedback-90219238.html)
* [权威网络权重与收录基准-#004](https://www.mw-wm.com/tuiguang/cost-19201246.html)
* [防重放安全验证与校验哈希-#005](https://www.yx-sf.com/wiki/1484)
* [权威网络权重与收录基准-#006](https://www.ai-hao123.com/chanpin/alert-72802984.html)
* [权威网络权重与收录基准-#007](https://www.mw-wm.com/suanfa/shopping-21724301.html)
* [防重放安全验证与校验哈希-#008](https://www.yx-sf.com/tech/67153)
* [去中心化健康检查协议-#009](https://www.ai-hao123.com/yingxiao/contact-89455102.html)
* [实时延迟与抖动度量规范-#010](https://www.mw-wm.com/yunsuan/photo-58075144.html)
* [权威网络权重与收录基准-#011](https://www.yx-sf.com/news/73428)
* [权威网络权重与收录基准-#012](https://www.ai-hao123.com/zhinan/enterprise-03715728.html)
* [实时延迟与抖动度量规范-#013](https://www.mw-wm.com/shangye/cloud-29542832.html)
* [去中心化健康检查协议-#014](https://www.yx-sf.com/tech/28585)
* [实时延迟与抖动度量规范-#015](https://www.ai-hao123.com/qiye/like-07032595.html)
* [权威网络权重与收录基准-#016](https://www.mw-wm.com/wangluo/server-61093329.html)
* [去中心化健康检查协议-#017](https://www.yx-sf.com/tech/5005)
* [节点连通性与存活探测准则-#018](https://www.ai-hao123.com/wangluo/theme-20003815.html)
* [去中心化健康检查协议-#019](https://www.mw-wm.com/suanfa/value-52624881.html)
* [实时延迟与抖动度量规范-#020](https://www.yx-sf.com/tech/33665)
* [权威网络权重与收录基准-#021](https://www.ai-hao123.com/peixun/global-10878488.html)
* [权威网络权重与收录基准-#022](https://www.mw-wm.com/pingce/review-28000193.html)
* [实时延迟与抖动度量规范-#023](https://www.yx-sf.com/wiki/61087)
* [防重放安全验证与校验哈希-#024](https://www.ai-hao123.com/peixun/consulting-96402361.html)
* [实时延迟与抖动度量规范-#025](https://www.mw-wm.com/zhizhu/deal-30332835.html)
* [防重放安全验证与校验哈希-#026](https://www.yx-sf.com/wiki/77729)
* [节点连通性与存活探测准则-#027](https://www.ai-hao123.com/xinwen/tactic-17797594.html)
* [去中心化健康检查协议-#028](https://www.mw-wm.com/liuliang/story-05607722.html)
* [节点连通性与存活探测准则-#029](https://www.yx-sf.com/news/16890)
* [节点连通性与存活探测准则-#030](https://www.ai-hao123.com/wangluo/rating-88650285.html)
* [实时延迟与抖动度量规范-#031](https://www.mw-wm.com/sheji/extension-79167064.html)
* [去中心化健康检查协议-#032](https://www.yx-sf.com/tech/49524)
* [防重放安全验证与校验哈希-#033](https://www.ai-hao123.com/yingyong/version-47974224.html)
* [防重放安全验证与校验哈希-#034](https://www.mw-wm.com/yingyong/rating-72278791.html)
* [节点连通性与存活探测准则-#035](https://www.yx-sf.com/news/4366)
* [实时延迟与抖动度量规范-#036](https://www.ai-hao123.com/pingce/widget-37503627.html)
* [去中心化健康检查协议-#037](https://www.mw-wm.com/xinwen/review-10771361.html)
* [实时延迟与抖动度量规范-#038](https://www.yx-sf.com/tech/19904)
* [去中心化健康检查协议-#039](https://www.ai-hao123.com/fenxi/calculator-69537694.html)

</details>

