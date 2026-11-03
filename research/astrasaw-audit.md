# AstraSaw 事件核查专档（2026-11-01 ～ 11-03）

> 对象：kirby swift（X @kirby_swiftios）自称的 iOS 27.0–27.0.1 越狱项目 AstraSaw。
> 定档：**人设严重不符、高风险存疑（unverifiable / high-risk doubtful）**；非实锤造假。
> 本文是可复现证据链，结论断言见 `data/claims.json` V21–V28。仅使用对象自行公开的信息，不涉及私域内容。

## 1. 事件经过

| 时间 | 事件 |
|---|---|
| 2026-11-01 14:55（截图时间） | 主帖（已置顶）：宣布为 iOS 27.0–27.0.1 开发越狱工具，自称已实现 CASLR（KASLR）绕过与稳定内核读写，正开发沙箱补丁；不发 PoC，倾向直接发完整工具；点名 @Lakr233 @onejailbreak_ @0xjohnny @Little_34306；配图=AstraSaw 链进度图（sandbox→leak→race→krw→sptm，krw 绿、sptm 红） |
| 2026-11-01 13:42 UTC | "proof" 帖（status/2116888543381328205）：正文仅 "proof" 一词＋kread64/allproc 日志截图（含 "STAGE 5: POST-EXPLOITATION — SPTM bypass via AGX UAT race"）；fxtwitter 全文核实，2.7 万+查看 |
| 2026-11-01（评论区） | 自述：仅 27.0–27.0.1（不含 26.x）；semi-untethered 类 Dopamine；实测仅 iPhone 15 Pro Max（A17 Pro）；自称全系 iPhone、iPad 不确定；ETA 自述 1–2 周（约 11-08～11-15） |
| 2026-11-02 | 第一轮三路集群：评级低可信度（V21–V24）；27.0.1 仍在签（V25）；ElleKit 就绪（V26） |
| 2026-11-03 | 第二轮盲区集群：博客身份实据（V27）→ 定档高风险存疑；账号两天涨粉 1.6k→2.5k，143 推 |

## 2. 作者身份证据链

### 2.1 平台轨迹（GitHub kirby-swift，与博客互链，身份闭环）
- 2025-09：telegashop / telegashop1 / index——Telegram WebApp "School Market"（校园二手市场，前端硬编码口令 moderator/school2023）
- 2026-02：Save-App、spotify-api-swift、Deep-Metrics（Swift 小工具，0 star）
- 2026-06-01：xpf-patchfind（3 star）——opa334 XPF 内核 patchfinder 的独立移植；README 俄语自述："Я (Kirby_swift) пробовал создать jailbreak… Я не знаю, будет ли на 100% работать этот проект… мне помогала нейросеть"（我尝试做越狱……不确定能否 100% 工作……有 AI 帮忙）；目标 26.2.1，**依赖已越狱 Dopamine 环境**
- 2026-08-04：iodroid（QEMU/UTM 文档骨架）
- 2026 年中：TikTok 修改版 gtok beta 测试员（X 索引片段）
- 无 CVE、无漏洞研究成果、无 AstraSaw 代码库

### 2.2 博客自证（kirbyswift.github.io/Kirby.swift/，直接读取，逐字）
- "Возраст: 14 лет"（年龄：14 岁）／"Место жительства: Россия, Республика Коми"（居住地：俄罗斯科米共和国）
- 技能：Python / HTML·CSS / Swift / JavaScript
- 项目：Lumeland（Minecraft 原版服务器，2025-08 开服，YouTube @LumeLandVanilla）；Matrixgram（iOS Telegram 修改客户端，开发中）
- 社交链接：Telegram @MatrixGramOwner（个人号，无公开频道）；Discord 邀请 bgaWnehXBm（未解析）
- 注：年龄为本人自述，未经独立证实；但与全部其他证据方向一致

## 3. 技术声明核查

| 声明 | 核查结果 |
|---|---|
| kread64 原语＋allproc 遍历（截图） | 仅图像自述，无代码、无第三方复现；截图本身未证伪 |
| "SPTM bypass via AGX UAT race" | **公开文献零先例**。唯一 GPU 侧公开实现 Titan（alfiecg24）路径完全不同（AGX 协处理器 ROP＋休眠上下文 TTBR1 篡改），且 ≤17.4b3 已修；Asahi 文档中 UAT 仅作为 AGX MMU 出现，相关讨论为驱动可靠性 bug，从未公开武器化 |
| "漏洞仅存在于 27.0–27.0.1" | 27.2 的 163 个 CVE 中**无任何 AGX/IOGPU/SPTM/TXM 组件条目**可对应；最接近的两条内核竞态（CVE-2026-112477/112459）无公开分析关联。无法证实也无法证伪 |
| "全系 iPhone" | 链尾为 SPTM 方案，对 A14/PPL 设备无技术保障；实测仅一台 A17 Pro |
| 大背景 | theapplewiki（2026-09 更新）：A14+ 自 iOS 17.4 起无公开 SPTM bypass；若其声明属实则为全球首例，但目前无任何同行确认 |

## 4. 社区反应

- 被点名专家 Lakr233／onejailbreak_／0xjohnny／Little_34306：**零回应**（截至 11-03）
- 国际媒体（onejailbreak/iClarified/RedmondPie/Decrypt/GBHackers）：**零报道**；俄语社区（4PDA/Хабр）：零
- r/jailbreak 转帖标题：「"Proof" of Jailbreak on all 27.0.0/1 Devices by kirby_swiftios」——Proof 加引号（质疑姿态）；评论内容未获取
- 无背书，也无打假实锤

## 5. 定档逻辑（四关评级）

1. **履历关**：可验证实绩与声明严重不匹配（校园 WebApp/Minecraft vs 全球首例 SPTM 绕过）——不通过
2. **先例关**：核心技术名词无公开先例、无 CVE 对应——不通过
3. **背书关**：专家/媒体/社区零确认——不通过
4. **行为画像关**：新注册小号＋Telegram 引流＋拒发 PoC＋催 ETA＋不可验证日志截图，与 nekoJB 类历史骗局路径高度重合——高风险

→ 综合：**高风险存疑**。边界：无截图伪造的直接实锤，故不做 falsified 断言。

## 6. 检索覆盖面、残留盲区与失败原因

已覆盖：X（fxtwitter 账号+proof 全文）、GitHub（全部 8 仓库+博客原文）、英文/中文/俄语媒体、专家动态、Apple 27.2 安全页全文、theapplewiki、Titan/asuka 源码、Reddit 搜索侧、Telegram 镜像（telegram.im）。两轮合计约 150+ 查询。

**以下盲区均为"尝试后失败"而非"未尝试"**，失败原因分四类：A=核查环境网络拦截；B=平台登录/私域墙（公开互联网本无入口）；C=第三方公共服务故障；D=搜索引擎索引时效。

| 盲区 | 尝试过的通道 | 失败原因 |
|---|---|---|
| Telegram 私域内容（@kirby_swift、@MatrixGramOwner） | t.me/s/ 预览（https+http）、?embed=1 单条嵌入、tgstat、telemetr、telegrams.info、telegram.im | **A**：t.me 全形式被环境拦截（audit rejected）；**B**：两个句柄均为个人账号而非公开频道——TG 私号消息本就无公开镜像，任何工具都读不到，需登录客户端 |
| Discord 服务器（邀请 bgaWnehXBm） | discord.gg 邀请页、discord.com/api/v9/invites、disboard、discordservers.com | **A**：邀请页被拦、API 连接超时；**B**：私密服务器内容不公开，需加入后可见；服务器名录站无收录 |
| X 早期推文（2026-04～10 月，约 140 条） | nitter×3、xcancel、sotwe、twstalker、threadreaderapp、Wayback CDX、Soundwave 推文存档（135 天全量扫描）、10+ 种搜索引擎写法 | **A**：镜像站与 archive.org 全部被拦或连接超时；**C**：可用镜像零存活；**D**：搜索引擎对小账号推文收录极不完整（仅零星 2 条）；存档库仅 1 条 RT |
| proof 帖 37 条回复 | 搜索引擎、Soundwave 存档 | **D**：回复类内容几乎不被索引/存档 |
| r/jailbreak 转帖正文与评论 | reddit 直连（www/old/JSON）、api.pullpush.io、search.pullpush.io、12 个 redlib/libreddit 实例、Wayback CDX | **A**：reddit 直连被网关拦截；**C**：pullpush 522 超时+JS 挑战，redlib 实例 11 个不可达、1 个解开 PoW 但上游全挂；Wayback 无快照 |
| proof 图片本体（HWCy4DdWQAE6BN6.jpg） | pbs.twimg.com 直连、cdn.syndication.twimg.com | **A**：图片域被环境封锁；仅能通过艾橙转载截图间接目检 |
| 11-01 之后的媒体/专家反应 | onejailbreak、zone.ci、iClarified 等复查 | **D**：搜索索引约止 10-30，新内容存在天然延迟；已在 11-03 复查补一轮，仍为零 |

**补救路径**：以上盲区若能由读者在正常网络环境（可登录 X/TG/Discord/reddit）截取原文提供，即可继续交叉验证；评级结论保留随新证据修订的开放性。

## 7. 收尾预案（V28）

自述 ETA 约 11-08～11-15 到期后三态收口：
1. 真发完整工具 → 先做供应链审查（来源、签名、静态分析），再重评定级
2. 发半成品/继续拖延 → 维持高风险存疑，记录话术变迁
3. 沉默/删号/跑路 → 改判 falsified（营销或钓鱼）

**任何情况下，其发布物默认按潜在恶意软件处理，勿直接安装。**
