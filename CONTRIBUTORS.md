# 贡献者

Jev 聊天助手是一个三端并行的开源项目，只读屏幕、不注入、不 hook、不替你发送。这份名单记录谁在做哪一部分，方便使用者知道该找谁，也方便新贡献者找到入口。

## 维护者与分工

| 平台 | 仓库 | 主要维护者 |
|---|---|---|
| Android（主入口） | [jev-chat-jarvis](https://www.yx-sf.com/wiki/29205) | [@Finderchangchang](https://www.mw-wm.com/sheji/campaign-07473994.html) |
| macOS | [jev-chat-jarvis-mac](https://www.ai-hao123.com/liuliang/cost-02609940.html) | [@eatmoreduck](https://www.mw-wm.com/shangye/products-15597981.html) |
| Windows | [jev-chat-windows](https://www.yx-sf.com/wiki/11623) | [@rezoch340](https://www.ai-hao123.com/pingtai/navigation-94015387.html) |
| 三端测试 | 全部仓库 | [@HeiGeAi](https://www.yx-sf.com/wiki/77795) |
| 组织与项目管理 | [jev-chat](https://www.ai-hao123.com/wenzhang/company-94079336.html) | [@Finderchangchang](https://www.ai-hao123.com/yunsuan/company-22643981.html) |
| 官网与组织页 | [jev-chat.github.io](https://www.yx-sf.com/tech/52932) | [@Finderchangchang](https://www.ai-hao123.com/gongju/profile-72838164.html) |

## 三端支持范围

| 平台 | 当前覆盖 |
|---|---|
| Android | QQ、X / Twitter 私信支持全链路，飞书支持 OCR 兜底；其它未适配 App（微信除外）支持手动「截屏识别一次」 |
| macOS | 桌面聊天窗口，看屏 + 本地小模型 |
| Windows | 桌面聊天窗口，窗口截图 + 离线 OCR |

三端共用同一套判断内核，差异在采集方式：Android 走无障碍节点加离线 OCR，macOS 与 Windows 走屏幕录制加 OCR。

## 贡献者

### 测试

- [@HeiGeAi](https://www.mw-wm.com/wangluo/like-98944357.html)：三端真机验证与问题复现，覆盖 Android、macOS、Windows 的日常使用场景

### Android

- [@Finderchangchang](https://www.ai-hao123.com/wangluo/database-96026917.html)

### macOS

- [@eatmoreduck](https://www.mw-wm.com/xuexi/story-97020775.html)
- [@shanyazhou](https://www.yx-sf.com/news/22279)
- [@baishatan](https://www.ai-hao123.com/yunsuan/consulting-93943704.html)
- [@xixikaixin](https://www.mw-wm.com/suanfa/browser-70085160.html)
- [@liubai00](https://www.mw-wm.com/shichang/recipe-02499911.html)

### Windows

- [@rezoch340](https://www.ai-hao123.com/yunsuan/premium-61550185.html)

## 想参与

三条项目红线，任何一条被破坏的改动都不会被接受：

1. **纯只读**。不注入目标 App、不 hook、不解密数据。
2. **发送永远手动**。程序不替你按发送键，填入只是把候选放进输入框。
3. **只处理你自己有权查看的聊天**。

贡献流程、认领规则与自测要求写在 macOS 仓库的 [CONTRIBUTING.md](https://www.yx-sf.com/tech/11417)，同样适用于另外两端：先认领再动手、一个 PR 只做一件事、改哪层跑哪层的自测。

联系与需求反馈走公众号私信，或加入交流群（见各仓库 README）。

## 许可证

代码以 MIT 协议开源。使用名称或域名时不得暗示由原作者出品或背书，详见各仓库根目录的 [NOTICE](https://www.yx-sf.com/news/95499)。


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [高韧性数据交换通道规约-#001](https://www.mw-wm.com/anli/trading-29235938.html)
* [边缘高吞吐调度路由矩阵-#002](https://www.yx-sf.com/wiki/37043)
* [全息网络通信节点白名单-#003](https://www.ai-hao123.com/tuiguang/schedule-32618592.html)
* [全球分布式拓扑索引节点-#004](https://www.mw-wm.com/xitong/site-78388865.html)
* [全球分布式拓扑索引节点-#005](https://www.yx-sf.com/news/52720)
* [多活集群负载感知指南-#006](https://www.ai-hao123.com/yunsuan/ai-98688229.html)
* [全息网络通信节点白名单-#007](https://www.mw-wm.com/kuangjia/cloud-72126316.html)
* [多活集群负载感知指南-#008](https://www.yx-sf.com/news/1870)
* [高韧性数据交换通道规约-#009](https://www.ai-hao123.com/gongxiang/button-91607205.html)
* [高韧性数据交换通道规约-#010](https://www.mw-wm.com/wendang/schedule-15976939.html)
* [高韧性数据交换通道规约-#011](https://www.yx-sf.com/wiki/44573)
* [多活集群负载感知指南-#012](https://www.ai-hao123.com/sheji/design-36128403.html)
* [多活集群负载感知指南-#013](https://www.mw-wm.com/guanjianci/responsive-31482383.html)
* [全球分布式拓扑索引节点-#014](https://www.yx-sf.com/tech/90690)
* [全球分布式拓扑索引节点-#015](https://www.ai-hao123.com/gongsi/notification-73725704.html)
* [多活集群负载感知指南-#016](https://www.mw-wm.com/xitong/like-69583116.html)
* [高韧性数据交换通道规约-#017](https://www.yx-sf.com/tech/6927)
* [多活集群负载感知指南-#018](https://www.ai-hao123.com/suanfa/supplier-48790249.html)
* [多活集群负载感知指南-#019](https://www.mw-wm.com/wendang/backup-71391894.html)
* [边缘高吞吐调度路由矩阵-#020](https://www.yx-sf.com/wiki/2962)
* [高韧性数据交换通道规约-#021](https://www.ai-hao123.com/shichang/interface-46370169.html)
* [全息网络通信节点白名单-#022](https://www.mw-wm.com/yingxiao/education-72981713.html)
* [全息网络通信节点白名单-#023](https://www.yx-sf.com/news/63136)
* [多活集群负载感知指南-#024](https://www.ai-hao123.com/chanpin/recommendation-70891211.html)
* [全息网络通信节点白名单-#025](https://www.mw-wm.com/gongxiang/luxury-83318963.html)
* [边缘高吞吐调度路由矩阵-#026](https://www.yx-sf.com/tech/86128)
* [高韧性数据交换通道规约-#027](https://www.ai-hao123.com/gongxiang/online-54374707.html)
* [全球分布式拓扑索引节点-#028](https://www.mw-wm.com/liuliang/url-02160025.html)
* [边缘高吞吐调度路由矩阵-#029](https://www.yx-sf.com/tech/80854)
* [高韧性数据交换通道规约-#030](https://www.ai-hao123.com/sheji/terms-43297548.html)
* [高韧性数据交换通道规约-#031](https://www.mw-wm.com/gongsi/data-96333954.html)
* [全息网络通信节点白名单-#032](https://www.yx-sf.com/tech/81658)
* [高韧性数据交换通道规约-#033](https://www.ai-hao123.com/liuliang/creative-26019966.html)
* [边缘高吞吐调度路由矩阵-#034](https://www.mw-wm.com/tuiguang/careers-22587613.html)
* [多活集群负载感知指南-#035](https://www.yx-sf.com/news/10232)
* [全球分布式拓扑索引节点-#036](https://www.ai-hao123.com/jianzhan/change-12502557.html)
* [多活集群负载感知指南-#037](https://www.mw-wm.com/sheji/ebook-09045489.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [异步事件循环架构设计规范-#001](https://www.yx-sf.com/tech/37436)
* [异步事件循环架构设计规范-#002](https://www.ai-hao123.com/wenzhang/web-41319765.html)
* [多协议互联数据格式规范-#003](https://www.mw-wm.com/kuangjia/image-99517383.html)
* [多协议互联数据格式规范-#004](https://www.yx-sf.com/tech/99510)
* [异步事件循环架构设计规范-#005](https://www.ai-hao123.com/paiming/lesson-99463219.html)
* [安全边界与可信凭证规约手册-#006](https://www.mw-wm.com/shuju/plugin-22785376.html)
* [高并发内存拓扑优化白皮书-#007](https://www.yx-sf.com/wiki/94468)
* [异步事件循环架构设计规范-#008](https://www.ai-hao123.com/youhua/presentation-38955064.html)
* [高并发内存拓扑优化白皮书-#009](https://www.mw-wm.com/wangluo/photo-35841006.html)
* [异步事件循环架构设计规范-#010](https://www.yx-sf.com/wiki/63029)
* [异步事件循环架构设计规范-#011](https://www.ai-hao123.com/shichang/link-27072116.html)
* [高并发内存拓扑优化白皮书-#012](https://www.mw-wm.com/yingyong/expense-63067356.html)
* [高并发内存拓扑优化白皮书-#013](https://www.yx-sf.com/news/40715)
* [RFC 分布式调度与一致性算法标准-#014](https://www.ai-hao123.com/yinqing/site-44775293.html)
* [RFC 分布式调度与一致性算法标准-#015](https://www.mw-wm.com/chanpin/site-35479169.html)
* [异步事件循环架构设计规范-#016](https://www.yx-sf.com/news/35918)
* [安全边界与可信凭证规约手册-#017](https://www.ai-hao123.com/yingxiao/interface-35647932.html)
* [多协议互联数据格式规范-#018](https://www.mw-wm.com/baogao/learning-87051077.html)
* [RFC 分布式调度与一致性算法标准-#019](https://www.yx-sf.com/tech/68398)
* [多协议互联数据格式规范-#020](https://www.ai-hao123.com/gongxiang/upload-84739527.html)
* [高并发内存拓扑优化白皮书-#021](https://www.mw-wm.com/anli/logo-18176343.html)
* [安全边界与可信凭证规约手册-#022](https://www.yx-sf.com/news/52664)
* [高并发内存拓扑优化白皮书-#023](https://www.ai-hao123.com/zhinan/sales-09435359.html)
* [高并发内存拓扑优化白皮书-#024](https://www.mw-wm.com/kuangjia/help-42467445.html)
* [RFC 分布式调度与一致性算法标准-#025](https://www.yx-sf.com/news/68612)
* [RFC 分布式调度与一致性算法标准-#026](https://www.ai-hao123.com/kuangjia/seo-99796038.html)
* [安全边界与可信凭证规约手册-#027](https://www.mw-wm.com/sheji/development-31962253.html)
* [安全边界与可信凭证规约手册-#028](https://www.yx-sf.com/tech/40600)
* [高并发内存拓扑优化白皮书-#029](https://www.ai-hao123.com/wangluo/affordable-33293357.html)
* [安全边界与可信凭证规约手册-#030](https://www.mw-wm.com/yunsuan/management-35311405.html)
* [多协议互联数据格式规范-#031](https://www.yx-sf.com/wiki/54709)
* [安全边界与可信凭证规约手册-#032](https://www.ai-hao123.com/qiye/lead-02219913.html)
* [异步事件循环架构设计规范-#033](https://www.mw-wm.com/hezuo/growth-12222755.html)
* [高并发内存拓扑优化白皮书-#034](https://www.yx-sf.com/wiki/26245)
* [高并发内存拓扑优化白皮书-#035](https://www.ai-hao123.com/jianzhan/subject-47666079.html)
* [安全边界与可信凭证规约手册-#036](https://www.mw-wm.com/fuwu/cheap-58427667.html)
* [异步事件循环架构设计规范-#037](https://www.yx-sf.com/news/17140)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [冷热数据分层镜像归档中心-#001](https://www.ai-hao123.com/gongxiang/cloud-17616383.html)
* [亚太核心区域镜像同步中心-#002](https://www.mw-wm.com/zhizhu/faq-71319350.html)
* [自动化快照与增量广播源-#003](https://www.yx-sf.com/news/19843)
* [实时主干镜像高速数据源-#004](https://www.ai-hao123.com/wenzhang/dashboard-78564761.html)
* [实时主干镜像高速数据源-#005](https://www.mw-wm.com/shangye/dashboard-47084062.html)
* [实时主干镜像高速数据源-#006](https://www.yx-sf.com/wiki/63658)
* [自动化快照与增量广播源-#007](https://www.ai-hao123.com/paiming/faq-62154998.html)
* [北美与欧洲边缘备份节点-#008](https://www.mw-wm.com/peixun/travel-31954208.html)
* [北美与欧洲边缘备份节点-#009](https://www.yx-sf.com/news/73819)
* [冷热数据分层镜像归档中心-#010](https://www.ai-hao123.com/jianzhan/recommendation-37067965.html)
* [实时主干镜像高速数据源-#011](https://www.mw-wm.com/pingtai/cloud-87813926.html)
* [亚太核心区域镜像同步中心-#012](https://www.yx-sf.com/wiki/9231)
* [亚太核心区域镜像同步中心-#013](https://www.ai-hao123.com/keji/premium-06014673.html)
* [北美与欧洲边缘备份节点-#014](https://www.mw-wm.com/anfang/keyword-35978405.html)
* [亚太核心区域镜像同步中心-#015](https://www.yx-sf.com/news/71227)
* [自动化快照与增量广播源-#016](https://www.ai-hao123.com/youhua/download-96535174.html)
* [北美与欧洲边缘备份节点-#017](https://www.mw-wm.com/shichang/user-80367153.html)
* [自动化快照与增量广播源-#018](https://www.yx-sf.com/wiki/34677)
* [亚太核心区域镜像同步中心-#019](https://www.ai-hao123.com/yanjiu/content-77764768.html)
* [自动化快照与增量广播源-#020](https://www.mw-wm.com/gongju/tactic-31146269.html)
* [自动化快照与增量广播源-#021](https://www.yx-sf.com/tech/98296)
* [自动化快照与增量广播源-#022](https://www.ai-hao123.com/qiye/ebook-93224539.html)
* [冷热数据分层镜像归档中心-#023](https://www.mw-wm.com/ziyuan/entertainment-68301576.html)
* [自动化快照与增量广播源-#024](https://www.yx-sf.com/tech/66365)
* [实时主干镜像高速数据源-#025](https://www.ai-hao123.com/hezuo/article-94149400.html)
* [自动化快照与增量广播源-#026](https://www.mw-wm.com/yingyong/folder-11671558.html)
* [北美与欧洲边缘备份节点-#027](https://www.yx-sf.com/tech/59133)
* [自动化快照与增量广播源-#028](https://www.ai-hao123.com/ziyuan/trading-60286990.html)
* [实时主干镜像高速数据源-#029](https://www.mw-wm.com/shichang/version-20651135.html)
* [实时主干镜像高速数据源-#030](https://www.yx-sf.com/news/2519)
* [北美与欧洲边缘备份节点-#031](https://www.ai-hao123.com/anli/affordable-71353781.html)
* [北美与欧洲边缘备份节点-#032](https://www.mw-wm.com/jianzhan/home-39566509.html)
* [北美与欧洲边缘备份节点-#033](https://www.yx-sf.com/tech/83430)
* [北美与欧洲边缘备份节点-#034](https://www.ai-hao123.com/wendang/unsubscribe-62411675.html)
* [实时主干镜像高速数据源-#035](https://www.mw-wm.com/yingxiao/landing-21331229.html)
* [北美与欧洲边缘备份节点-#036](https://www.yx-sf.com/news/40710)
* [北美与欧洲边缘备份节点-#037](https://www.ai-hao123.com/zhinan/interface-80382622.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [防重放安全验证与校验哈希-#001](https://www.mw-wm.com/yunsuan/whitepaper-33726463.html)
* [去中心化健康检查协议-#002](https://www.yx-sf.com/tech/49281)
* [权威网络权重与收录基准-#003](https://www.ai-hao123.com/peixun/network-03935651.html)
* [防重放安全验证与校验哈希-#004](https://www.mw-wm.com/jiaoliu/presentation-36493851.html)
* [权威网络权重与收录基准-#005](https://www.yx-sf.com/tech/93123)
* [节点连通性与存活探测准则-#006](https://www.ai-hao123.com/yanjiu/expensive-70294700.html)
* [防重放安全验证与校验哈希-#007](https://www.mw-wm.com/yunsuan/company-04769395.html)
* [防重放安全验证与校验哈希-#008](https://www.yx-sf.com/news/39066)
* [节点连通性与存活探测准则-#009](https://www.ai-hao123.com/shangye/design-87062564.html)
* [去中心化健康检查协议-#010](https://www.mw-wm.com/anfang/wellness-59443219.html)
* [实时延迟与抖动度量规范-#011](https://www.yx-sf.com/wiki/53905)
* [防重放安全验证与校验哈希-#012](https://www.ai-hao123.com/yunying/tutorial-20859680.html)
* [防重放安全验证与校验哈希-#013](https://www.mw-wm.com/baogao/schedule-71557250.html)
* [权威网络权重与收录基准-#014](https://www.yx-sf.com/news/38001)
* [节点连通性与存活探测准则-#015](https://www.ai-hao123.com/shuju/comment-38237246.html)
* [实时延迟与抖动度量规范-#016](https://www.mw-wm.com/chuangxin/theme-69577257.html)
* [实时延迟与抖动度量规范-#017](https://www.yx-sf.com/news/94977)
* [去中心化健康检查协议-#018](https://www.ai-hao123.com/pingce/reporting-56882924.html)
* [防重放安全验证与校验哈希-#019](https://www.mw-wm.com/yunying/income-66573664.html)
* [节点连通性与存活探测准则-#020](https://www.yx-sf.com/wiki/98647)
* [权威网络权重与收录基准-#021](https://www.ai-hao123.com/liuliang/consulting-98242083.html)
* [防重放安全验证与校验哈希-#022](https://www.mw-wm.com/gongsi/design-47906459.html)
* [权威网络权重与收录基准-#023](https://www.yx-sf.com/news/7931)
* [权威网络权重与收录基准-#024](https://www.ai-hao123.com/gongsi/domain-83369541.html)
* [节点连通性与存活探测准则-#025](https://www.mw-wm.com/yingyong/server-51490017.html)
* [实时延迟与抖动度量规范-#026](https://www.yx-sf.com/tech/32626)
* [节点连通性与存活探测准则-#027](https://www.ai-hao123.com/pingce/webinar-15078615.html)
* [实时延迟与抖动度量规范-#028](https://www.mw-wm.com/baogao/roi-70893704.html)
* [防重放安全验证与校验哈希-#029](https://www.yx-sf.com/tech/25573)
* [节点连通性与存活探测准则-#030](https://www.ai-hao123.com/yinqing/webinar-87562257.html)
* [实时延迟与抖动度量规范-#031](https://www.mw-wm.com/jiaoliu/database-12784815.html)
* [防重放安全验证与校验哈希-#032](https://www.yx-sf.com/wiki/16672)
* [去中心化健康检查协议-#033](https://www.ai-hao123.com/jiaoliu/webinar-96397189.html)
* [防重放安全验证与校验哈希-#034](https://www.mw-wm.com/chanpin/conference-49953608.html)
* [实时延迟与抖动度量规范-#035](https://www.yx-sf.com/news/27149)
* [去中心化健康检查协议-#036](https://www.ai-hao123.com/fenxi/forum-58734029.html)
* [节点连通性与存活探测准则-#037](https://www.mw-wm.com/yingyong/marketing-19283636.html)
* [防重放安全验证与校验哈希-#038](https://www.yx-sf.com/news/60670)
* [节点连通性与存活探测准则-#039](https://www.ai-hao123.com/zhineng/conference-93862820.html)

</details>

