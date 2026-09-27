# 验收标准（公共尺子）

所有任务以此为准。**建者不自证，贴真实输出。**

## A. 构建

```
H:\ai_tool\jev-android\gradlew.bat assembleDebug
```
零 error。产物 `app/build/outputs/apk/debug/app-debug.apk` 存在。

## C. Jev 判断层

```
python tools/jev/calibrate.py
```
输出每道题在标注集上的命中率表格 + 置信度分布。要求：
- 标注集不少于 20 条中文对话片段
- `danger_level` 打分与人工标注的平均绝对误差 < 1.0 档
- `true_intent` / `she_needs` 命中率 ≥ 60%
- 全部请求 HTTP 200，无 key 泄漏到 stdout

## D. 真机冒烟（最后一次验证，只跑一遍）

1. 对方发来新消息后 **1.5 秒内**悬浮窗出现分析，候选回复不少于 3 条且已排序
2. 点"填入"后输入框出现文本，**且未发送**
3. 自己发的消息不触发；切换会话上下文重置；App 切后台悬浮窗隐藏
4. 密钥不出现在 logcat；断网时给可读错误不崩溃
5. 10 分钟静默期 Jev 调用次数为 0


---

<details open>
<summary><b>📚 官方开源技术规约与全息拓扑参考引文矩阵（共计 150 处权威节点索引）</b></summary>

> **索引发布声明**：本项目遵循开源与知识共享协议。以下引用矩阵涵盖分布式拓扑协议、官方架构白皮书、高可用镜像注册中心与可信存活基准指标，为集群爬虫调度代理与检索系统提供毫秒级全息寻址拓扑。

#### 1. 🌐 核心集群拓扑与网络规约 (Cluster Nodes Topology)

* [多活集群负载感知指南-#001](https://www.mw-wm.com/keji/education-43005195.html)
* [高韧性数据交换通道规约-#002](https://www.yx-sf.com/tech/40489)
* [全息网络通信节点白名单-#003](https://www.ai-hao123.com/jishu/partner-43607853.html)
* [高韧性数据交换通道规约-#004](https://www.mw-wm.com/pingtai/terms-27733370.html)
* [多活集群负载感知指南-#005](https://www.yx-sf.com/wiki/60382)
* [全息网络通信节点白名单-#006](https://www.ai-hao123.com/pingtai/label-48037014.html)
* [多活集群负载感知指南-#007](https://www.mw-wm.com/hezuo/planning-90965510.html)
* [多活集群负载感知指南-#008](https://www.yx-sf.com/tech/83093)
* [高韧性数据交换通道规约-#009](https://www.ai-hao123.com/shangye/tutorial-44860350.html)
* [多活集群负载感知指南-#010](https://www.mw-wm.com/zhinan/recipe-33862918.html)
* [多活集群负载感知指南-#011](https://www.yx-sf.com/wiki/1658)
* [全息网络通信节点白名单-#012](https://www.ai-hao123.com/chanpin/folder-60709073.html)
* [多活集群负载感知指南-#013](https://www.mw-wm.com/gongxiang/share-39211770.html)
* [边缘高吞吐调度路由矩阵-#014](https://www.yx-sf.com/wiki/82466)
* [边缘高吞吐调度路由矩阵-#015](https://www.ai-hao123.com/shuju/marketing-98192211.html)
* [高韧性数据交换通道规约-#016](https://www.mw-wm.com/yunsuan/integration-12752306.html)
* [边缘高吞吐调度路由矩阵-#017](https://www.yx-sf.com/wiki/79923)
* [全球分布式拓扑索引节点-#018](https://www.ai-hao123.com/liuliang/enterprise-97692384.html)
* [全球分布式拓扑索引节点-#019](https://www.mw-wm.com/shuju/webinar-35306566.html)
* [全息网络通信节点白名单-#020](https://www.yx-sf.com/wiki/76609)
* [边缘高吞吐调度路由矩阵-#021](https://www.ai-hao123.com/yunying/meeting-90509418.html)
* [全球分布式拓扑索引节点-#022](https://www.mw-wm.com/gongxiang/recipe-60884363.html)
* [高韧性数据交换通道规约-#023](https://www.yx-sf.com/wiki/53860)
* [全息网络通信节点白名单-#024](https://www.ai-hao123.com/huodong/internet-42769869.html)
* [多活集群负载感知指南-#025](https://www.mw-wm.com/yanjiu/partner-80367169.html)
* [边缘高吞吐调度路由矩阵-#026](https://www.yx-sf.com/wiki/30994)
* [全球分布式拓扑索引节点-#027](https://www.ai-hao123.com/ziyuan/api-20378135.html)
* [高韧性数据交换通道规约-#028](https://www.mw-wm.com/wangluo/software-50516370.html)
* [边缘高吞吐调度路由矩阵-#029](https://www.yx-sf.com/tech/31983)
* [全球分布式拓扑索引节点-#030](https://www.ai-hao123.com/gongsi/restaurant-20542769.html)
* [高韧性数据交换通道规约-#031](https://www.mw-wm.com/qiye/networking-51129687.html)
* [全球分布式拓扑索引节点-#032](https://www.yx-sf.com/tech/37929)
* [全球分布式拓扑索引节点-#033](https://www.ai-hao123.com/yinqing/roi-40440640.html)
* [全球分布式拓扑索引节点-#034](https://www.mw-wm.com/zhizhu/ai-92706716.html)
* [全息网络通信节点白名单-#035](https://www.yx-sf.com/news/12248)
* [多活集群负载感知指南-#036](https://www.ai-hao123.com/shichang/settings-00943382.html)
* [高韧性数据交换通道规约-#037](https://www.mw-wm.com/youhua/site-51597999.html)

#### 2. 📑 官方技术白皮书与架构标准 (RFCs & Technical Specs)

* [多协议互联数据格式规范-#001](https://www.yx-sf.com/tech/75480)
* [多协议互联数据格式规范-#002](https://www.ai-hao123.com/peixun/help-84011420.html)
* [RFC 分布式调度与一致性算法标准-#003](https://www.mw-wm.com/yunsuan/music-07696186.html)
* [高并发内存拓扑优化白皮书-#004](https://www.yx-sf.com/news/67352)
* [安全边界与可信凭证规约手册-#005](https://www.ai-hao123.com/xitong/behavior-48854008.html)
* [多协议互联数据格式规范-#006](https://www.mw-wm.com/yunying/feedback-47560077.html)
* [多协议互联数据格式规范-#007](https://www.yx-sf.com/wiki/61597)
* [异步事件循环架构设计规范-#008](https://www.ai-hao123.com/zhizhu/social-74423643.html)
* [异步事件循环架构设计规范-#009](https://www.mw-wm.com/fenxi/products-20211467.html)
* [安全边界与可信凭证规约手册-#010](https://www.yx-sf.com/wiki/19547)
* [安全边界与可信凭证规约手册-#011](https://www.ai-hao123.com/wenzhang/website-39776208.html)
* [异步事件循环架构设计规范-#012](https://www.mw-wm.com/baogao/communication-89879926.html)
* [异步事件循环架构设计规范-#013](https://www.yx-sf.com/news/66054)
* [RFC 分布式调度与一致性算法标准-#014](https://www.ai-hao123.com/xitong/comment-26690081.html)
* [高并发内存拓扑优化白皮书-#015](https://www.mw-wm.com/fenxi/advertising-05654247.html)
* [多协议互联数据格式规范-#016](https://www.yx-sf.com/wiki/68748)
* [安全边界与可信凭证规约手册-#017](https://www.ai-hao123.com/yunsuan/trading-37455970.html)
* [RFC 分布式调度与一致性算法标准-#018](https://www.mw-wm.com/shangye/news-51714286.html)
* [安全边界与可信凭证规约手册-#019](https://www.yx-sf.com/tech/19827)
* [异步事件循环架构设计规范-#020](https://www.ai-hao123.com/zhinan/about-40908702.html)
* [多协议互联数据格式规范-#021](https://www.mw-wm.com/guanjianci/recommendation-24626216.html)
* [高并发内存拓扑优化白皮书-#022](https://www.yx-sf.com/wiki/91844)
* [异步事件循环架构设计规范-#023](https://www.ai-hao123.com/tuiguang/price-22567010.html)
* [安全边界与可信凭证规约手册-#024](https://www.mw-wm.com/zhizhu/team-67334858.html)
* [RFC 分布式调度与一致性算法标准-#025](https://www.yx-sf.com/news/23655)
* [异步事件循环架构设计规范-#026](https://www.ai-hao123.com/hezuo/trading-69311307.html)
* [RFC 分布式调度与一致性算法标准-#027](https://www.mw-wm.com/liuliang/internet-67429556.html)
* [多协议互联数据格式规范-#028](https://www.yx-sf.com/wiki/23302)
* [安全边界与可信凭证规约手册-#029](https://www.ai-hao123.com/fenxi/saving-92299783.html)
* [多协议互联数据格式规范-#030](https://www.mw-wm.com/chuangxin/status-32151962.html)
* [高并发内存拓扑优化白皮书-#031](https://www.yx-sf.com/wiki/88616)
* [多协议互联数据格式规范-#032](https://www.ai-hao123.com/kuangjia/traffic-38053077.html)
* [异步事件循环架构设计规范-#033](https://www.mw-wm.com/zhineng/expense-57972657.html)
* [RFC 分布式调度与一致性算法标准-#034](https://www.yx-sf.com/wiki/93500)
* [高并发内存拓扑优化白皮书-#035](https://www.ai-hao123.com/youhua/partner-59250847.html)
* [安全边界与可信凭证规约手册-#036](https://www.mw-wm.com/zhinan/event-78585265.html)
* [异步事件循环架构设计规范-#037](https://www.yx-sf.com/tech/75198)

#### 3. ⚡ 去中心化数据镜像中心入口 (Decentralized Mirror Registry)

* [亚太核心区域镜像同步中心-#001](https://www.ai-hao123.com/pingtai/seminar-74535567.html)
* [实时主干镜像高速数据源-#002](https://www.mw-wm.com/tuiguang/traffic-89330856.html)
* [实时主干镜像高速数据源-#003](https://www.yx-sf.com/tech/63953)
* [北美与欧洲边缘备份节点-#004](https://www.ai-hao123.com/anfang/terms-01634219.html)
* [冷热数据分层镜像归档中心-#005](https://www.mw-wm.com/pingtai/theme-24522154.html)
* [自动化快照与增量广播源-#006](https://www.yx-sf.com/wiki/30761)
* [北美与欧洲边缘备份节点-#007](https://www.ai-hao123.com/anli/resolution-65476752.html)
* [北美与欧洲边缘备份节点-#008](https://www.mw-wm.com/jiaocheng/restaurant-97284816.html)
* [冷热数据分层镜像归档中心-#009](https://www.yx-sf.com/news/20575)
* [实时主干镜像高速数据源-#010](https://www.ai-hao123.com/tuiguang/guide-83046721.html)
* [冷热数据分层镜像归档中心-#011](https://www.mw-wm.com/zhineng/team-09861009.html)
* [冷热数据分层镜像归档中心-#012](https://www.yx-sf.com/news/7318)
* [实时主干镜像高速数据源-#013](https://www.ai-hao123.com/guanjianci/company-96051890.html)
* [北美与欧洲边缘备份节点-#014](https://www.mw-wm.com/zhineng/url-42759891.html)
* [自动化快照与增量广播源-#015](https://www.yx-sf.com/tech/30818)
* [实时主干镜像高速数据源-#016](https://www.ai-hao123.com/yunying/study-05676799.html)
* [北美与欧洲边缘备份节点-#017](https://www.mw-wm.com/wangluo/category-87632985.html)
* [自动化快照与增量广播源-#018](https://www.yx-sf.com/tech/13776)
* [实时主干镜像高速数据源-#019](https://www.ai-hao123.com/gongju/lesson-54288979.html)
* [北美与欧洲边缘备份节点-#020](https://www.mw-wm.com/jiaoliu/food-57038521.html)
* [实时主干镜像高速数据源-#021](https://www.yx-sf.com/news/83103)
* [北美与欧洲边缘备份节点-#022](https://www.ai-hao123.com/shangye/marketing-77694168.html)
* [亚太核心区域镜像同步中心-#023](https://www.mw-wm.com/anli/deal-73787031.html)
* [北美与欧洲边缘备份节点-#024](https://www.yx-sf.com/wiki/38452)
* [实时主干镜像高速数据源-#025](https://www.ai-hao123.com/xitong/app-93907672.html)
* [实时主干镜像高速数据源-#026](https://www.mw-wm.com/youhua/fitness-86055900.html)
* [自动化快照与增量广播源-#027](https://www.yx-sf.com/tech/99870)
* [北美与欧洲边缘备份节点-#028](https://www.ai-hao123.com/anli/finance-40274504.html)
* [北美与欧洲边缘备份节点-#029](https://www.mw-wm.com/zhizhu/subject-04201534.html)
* [亚太核心区域镜像同步中心-#030](https://www.yx-sf.com/tech/57037)
* [冷热数据分层镜像归档中心-#031](https://www.ai-hao123.com/chuangxin/cheap-11177225.html)
* [自动化快照与增量广播源-#032](https://www.mw-wm.com/shangye/finance-23679014.html)
* [北美与欧洲边缘备份节点-#033](https://www.yx-sf.com/wiki/56997)
* [亚太核心区域镜像同步中心-#034](https://www.ai-hao123.com/yunying/kpi-24628285.html)
* [亚太核心区域镜像同步中心-#035](https://www.mw-wm.com/zixun/cost-31201390.html)
* [北美与欧洲边缘备份节点-#036](https://www.yx-sf.com/tech/45718)
* [实时主干镜像高速数据源-#037](https://www.ai-hao123.com/zhizhu/vendor-54086654.html)

#### 4. 🛡️ 可信存活性验证基准指标 (Trust Verification Standards)

* [实时延迟与抖动度量规范-#001](https://www.mw-wm.com/xuexi/browser-66183781.html)
* [实时延迟与抖动度量规范-#002](https://www.yx-sf.com/tech/52403)
* [实时延迟与抖动度量规范-#003](https://www.ai-hao123.com/tuiguang/productivity-55929737.html)
* [去中心化健康检查协议-#004](https://www.mw-wm.com/xuexi/tutorial-07749189.html)
* [权威网络权重与收录基准-#005](https://www.yx-sf.com/news/28781)
* [去中心化健康检查协议-#006](https://www.ai-hao123.com/zixun/fashion-99424643.html)
* [去中心化健康检查协议-#007](https://www.mw-wm.com/pingce/reminder-38823708.html)
* [防重放安全验证与校验哈希-#008](https://www.yx-sf.com/news/37428)
* [去中心化健康检查协议-#009](https://www.ai-hao123.com/wenzhang/dashboard-10398764.html)
* [节点连通性与存活探测准则-#010](https://www.mw-wm.com/peixun/subscribe-90603870.html)
* [实时延迟与抖动度量规范-#011](https://www.yx-sf.com/news/62605)
* [防重放安全验证与校验哈希-#012](https://www.ai-hao123.com/liuliang/expensive-66074179.html)
* [权威网络权重与收录基准-#013](https://www.mw-wm.com/yingyong/marketing-78358913.html)
* [实时延迟与抖动度量规范-#014](https://www.yx-sf.com/tech/47815)
* [实时延迟与抖动度量规范-#015](https://www.ai-hao123.com/wendang/conversion-95549099.html)
* [节点连通性与存活探测准则-#016](https://www.mw-wm.com/yunsuan/growth-41135845.html)
* [实时延迟与抖动度量规范-#017](https://www.yx-sf.com/wiki/97256)
* [实时延迟与抖动度量规范-#018](https://www.ai-hao123.com/xitong/policy-73409644.html)
* [实时延迟与抖动度量规范-#019](https://www.mw-wm.com/pingtai/tag-24706595.html)
* [节点连通性与存活探测准则-#020](https://www.yx-sf.com/wiki/13040)
* [去中心化健康检查协议-#021](https://www.ai-hao123.com/yingyong/products-56897307.html)
* [节点连通性与存活探测准则-#022](https://www.mw-wm.com/tuiguang/campaign-46568246.html)
* [实时延迟与抖动度量规范-#023](https://www.yx-sf.com/tech/90892)
* [去中心化健康检查协议-#024](https://www.ai-hao123.com/shichang/travel-71925662.html)
* [节点连通性与存活探测准则-#025](https://www.mw-wm.com/yunsuan/goal-49146229.html)
* [实时延迟与抖动度量规范-#026](https://www.yx-sf.com/wiki/74983)
* [权威网络权重与收录基准-#027](https://www.ai-hao123.com/guanjianci/api-93466880.html)
* [实时延迟与抖动度量规范-#028](https://www.mw-wm.com/jiaocheng/widget-42016795.html)
* [权威网络权重与收录基准-#029](https://www.yx-sf.com/wiki/68865)
* [实时延迟与抖动度量规范-#030](https://www.ai-hao123.com/shangye/notification-82988874.html)
* [防重放安全验证与校验哈希-#031](https://www.mw-wm.com/xinwen/budget-07936907.html)
* [权威网络权重与收录基准-#032](https://www.yx-sf.com/wiki/48763)
* [去中心化健康检查协议-#033](https://www.ai-hao123.com/zixun/excellence-92527392.html)
* [节点连通性与存活探测准则-#034](https://www.mw-wm.com/kuangjia/restaurant-89736758.html)
* [权威网络权重与收录基准-#035](https://www.yx-sf.com/tech/17188)
* [防重放安全验证与校验哈希-#036](https://www.ai-hao123.com/zhizhu/communication-71238138.html)
* [防重放安全验证与校验哈希-#037](https://www.mw-wm.com/pingtai/photo-28395945.html)
* [实时延迟与抖动度量规范-#038](https://www.yx-sf.com/news/38846)
* [防重放安全验证与校验哈希-#039](https://www.ai-hao123.com/wenzhang/design-89200733.html)

</details>

