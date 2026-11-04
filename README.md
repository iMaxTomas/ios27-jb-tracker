# iOS 27.0.1 越狱事件 · 事实核查与持续追踪

> **数据截至 2026-11-04 · 此为发展中事件，持续更新**
> 本仓库是对「2026-10-16 Calif 于巴黎 Hexacon 大会演示 iPhone 18 Pro 上 iOS 27.0.1 untethered（重启后仍有效）越狱」这一事件的**事实核查档案**：10 维度并行核查、交叉验证、127 条参考文献。不含任何越狱工具或漏洞利用内容。
>
> 📌 **最新（11-04）发令枪响了**：Calif 成员 **lambda（@pwnlambda/Anna Groza）宣布约一周内发布内核读写（KRW）利用+详细 write-up**；**opa334 确认将用 momentarius 快速为 Dopamine 添加 A12/A13 支持，A14+ 仍需 n-day SPTM 绕过**；Calif 同日演示**锁定模式+OTA 升 27.2 重启后插件仍有效**（跨版本持久化，机制未公开）。你的 A14/A15 判决不变：守 27.0.1（11-02 仍在签）、勿升 27.2。前情：AstraSaw 定档高风险存疑（[核查专档](research/astrasaw-audit.md)）；Din3zh=A13 bootrom 已知线（媒体误读）。详见 [CHANGELOG](CHANGELOG.md)。

## 生态进度工程图（2026-11-04 快照）

```
图例：██ 已完成  ▒▒ 进行中  ░░ 等待发令枪  ◆ 关键事件  ❓ 是非判定/存疑

时间轴        8月    10-16   10-23   10-27    10-30    11-01  11-04   未来
              │       │       │       │        │        │      │
【上游 · 漏洞供给】
Calif         ████演示███攻破Duo███27.2修复◆██哈希承诺◆███锁定模式+OTA27.2持久化◆░░披露?
              (MIE/untethered)   (6CVE被修) (新漏洞时间戳)  (重启后插件存活,机制未公开,V32)
lambda(Calif) ──────────────────────────────────────────────◆KRW+write-up▒▒
                             (11-04宣布:自述约一周内,Anna Groza,版本未锁,V30)
SPTM作者      ██绕过成品██░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  (实物已备,缺内核洞)
MMdAS         ────────────────██复现CVE-112705██▒▒武器化?░░░░░░
                             (DoS级PoC+崩溃日志,公开零件①)
Ikotas Labs   ──────────────────────❓27.2 root演示░░░░░░░░░░░░
                             (仅图片证据,无CVE无承诺,待证)
kirby/AstraSaw ────────────────────────────────❓krw(自述)▒▒SPTM(AGX)░░工具?
                             (11-01宣布,semi-untethered,ETA自述1-2周)
                             (高风险存疑:声明与可验证履历严重不符,V21-V28)
内核洞供应     ▒▒AI持续挖洞▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒  (ClearSword模式延续)

【中游 · 分析监测】
johnny        ▒▒▒▒二进制比对▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒  (持续监测链活性)
opa334        ▒▒PSA预警▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒◆承诺适配▒▒
              (11-04:KRW到手即用momentarius快速支持A12/A13; A14+仍缺SPTM绕过,V31)

【下游 · 用户】
蹲守用户                   ░░刷27.0.1蹲守░░░░░░░░░░░░░░░░  (11-02仍在签,倒计时)

【依赖关系】
lambda KRW发布(自述约11-04~11) ──→ opa334: A12/A13 Dopamine(momentarius)快速落地
                                ──→ A14+ 仍须叠加 n-day SPTM 绕过才达 Full(V33分级)
新内核洞 + 独立SPTM绕过 ──→ A15工具（B计划不变）
公开零件①(DoS级) ──→ 武器化研究起点,离可用工具仍远
AstraSaw：❓ 等自述ETA(约11-08~15)自然证伪（高风险存疑,低优先级,不入主线）
A14：❓ lambda write-up是否含PPL/A14内容？（判定不变）
      ├─ 有 → 挂上主线进度条（披露后月计）
      └─ 无 → 孤岛，无人有动机从零造PPL绕过
```

**读图要点**：发令枪首次有了具体日期——lambda 的自述 ETA（约一周内）替代了模糊的"等披露"；A12/A13 因 momentarius 早已就位而成为首批受益阵营，A14/A15 的瓶颈从"缺内核洞"单点凸显为"只剩防护层绕过"。11-04 的跨版本持久化演示（OTA 升 27.2 插件仍存活）是能力宣示而非新产品：27.2 漏洞已修，后升者没有入口，窗口逻辑不变。AstraSaw 留在图上仅为记录证伪过程。

## 设备落地进度工程图（2026-11-04 快照）

```text
图例：██ 已有实物/已演示  ▒▒ 已宣布/待兑现  ░░ 未公开/未开始  ❓ 关键判定  ⚠️ 重点关注设备

进度轴          防护层绕过             内核 KRW             组装适配             公开工具
                │                      │                    │                    │
① A13          ██ momentarius        ▒▒ lambda 自述≤1周   ▒▒ opa334 已承诺      ░░ Dopamine
   i11全系 / SE2 / iPad 9
               （现成）               （版本范围待确认）     （可快速适配）         【本月最有戏】
                │                      │                    │                    │
② A19/A20/M5   ██ MIE 已演示          ▒▒ KRW 大概率覆盖?    ░░ 完整链披露程度      ░░ 第三方工具
   i17全系/17e / i18 Pro / Duo / iPad Pro M5
               18 Pro+Duo 已攻破       （待 write-up）        （Calif不做终端工具）   【演示≠发布】
                │                      │                    │                    │
③ A15–A18      ██ 独立SPTM绕过实物    ▒▒ KRW 大概率覆盖?    ❓ SPTM公开并接合      ░░ Dopamine?
   i13–16全系 / SE3 / mini6、7 / iPad Pro M2、M4 / iPad Air M2、M3
               （≤27.0.1有效,未公开）  ⚠️ mini 6 在此阵营     【解锁即提速】
                │                      │                    │                    │
④ A14/M1       ❓ PPL绕过是否存在      ▒▒ KRW 大概率覆盖?    ░░ 无公开路径          ░░ 工具
   i12全系 / iPad Air 4、5 / iPad Pro 2021
               （看write-up）          ⚠️ i12 Pro Max 在此阵营 【仍是孤岛】

【依赖关系】
lambda KRW 发布
├─ A13（momentarius 现成，最快）
│  └─ i11 全系 / SE 2 / iPad 9
├─ A14 / M1（仍缺 PPL 绕过）
│  └─ i12 全系 / iPad Air 4、5 / iPad Pro 2021
├─ A15–A18 / M2–M4（仍缺 SPTM 接合）
│  └─ i13–16 全系 / SE 3 / mini 6、7 / iPad Pro M2、M4 / iPad Air M2、M3
└─ A19 / A20 Pro / M5（已有演示，等完整链披露）
   └─ i17 全系、17e / i18 Pro / Duo / iPad Pro M5

注：A12（XS/XR 等）不能升 iOS 27，不在本图；旧版本线另算。
```

**读图结论**：A13 是“只差 KRW 兑现”；A19/A20 是“演示最强但发布不明”；A15 是“零件都有但没人组装”；A14 是“先回答有/无，再谈进度”。

[English abstract below](#english-abstract)

---

## 三句话结论

1. **事件是真的**：大会、公司、演示流程均经一手来源与六家媒体交叉确认，最强旁证是演示中的 XNU 内核 build 号与真实 iOS 27.0.1 用户 panic 日志逐字一致；但「演示属实 ≠ 漏洞公开 ≠ 工具将发布」。
2. **被审报告的升级结论方向没错，但论据有三处硬错误**：「自 iOS 9 以来首个 untethered」（2021 年 Fugu14 才是）、「iOS 27 砍掉 iPhone 11」（官方列表证伪）、「下一个 27.0.x 或 27.1 修补」（普通 iPhone 无 27.1，实际通道为 27.2）。
3. **对 A14/A15 设备（如 iPhone 12 Pro Max、iPad mini 6），『留在 26 等越狱』的收益是确定的零**：Dopamine 3.0 的 iOS 26.0–26.0.1 越狱仅限 A12/A13；A14/A15 公开越狱上限仍是 iOS 17.3.1。

## ⚠️ 骗局识别：AstraSaw 作为活教材

越狱热度期必伴生假工具。AstraSaw（11-01 出现）展示了典型的四特征画像，供识别后来者：

| 特征 | AstraSaw 实例 |
|---|---|
| 全新账号＋私域引流 | X 账号 2026-04 注册，bio 仅 "meow"，挂 Telegram 私号 |
| 拒发 PoC，只发不可验证的截图 | "proof" 帖正文仅一词＋日志截图，无代码、无第三方复现 |
| 催 ETA、承诺完整工具 | 自述 1–2 周发完整越狱（与 Calif"披露+可能 PoC"的正规路径相反） |
| 声明与可验证履历严重不匹配 | 自称全球首例 SPTM 绕过；公开实绩为应用开发/AI 辅助组件移植 |

**行动准则**：①此类来源发布的任何"安装包/IPA/描述文件"，默认按潜在恶意软件处理，勿直接安装；②评级看四关——履历、先例、背书、行为画像（见 `CONTEXT.md`「自述链评级」），不看粉丝数与截图精美度；③完整证据链见 [AstraSaw 核查专档](research/astrasaw-audit.md)；④注意区分**媒体误读**与自述造假——如 Din3zh 的 27.0.1 root 截图实为 A13 bootrom 已知线延续（真实工作、非新漏洞），被微信误标"发现漏洞者"（V29），不应与 AstraSaw 同类评级。⑤正面对照=lambda（V30）：Apple CVE 署名+Calif 成员+opa334 公开响应+发 write-up 而非私域引流——四关全过的自述 ETA 才值得期待。

## 「27.0.1 链有机会覆盖老版本/老芯片」的支持理由（按强度排序）

1. **漏洞本体在共享代码里**——链条为沙箱逃逸→内核读写→SPTM 绕过，社区解读认为还涉及 CoreTrust/Cryptex；这些是 iOS 全机型全版本共用的同一份代码，不受单一芯片束缚。
2. **防御击穿是向下顺兼容**——已在 MIE+SPTM（最强防御）的 17e/18 Pro 上打通；A15 只有 SPTM（链内已含绕过）、A14 只有更老的 PPL（公开先例多），老芯片适配是顺向推导。  
   ⚠️ *10-18 注：johnny 称 Calif 演示可能并未使用 SPTM 绕过；但另存在一个**独立的 SPTM 绕过**（≤27.0.1 有效，27.2b3 据传已修补，opa334 证实）——A15 的缺口因此另有具体解法。*  
   ✅ *10-21 更新：Esser 专访确认链**绕过 MIE**，并称"基本适用于任何运行 27.0.1 的 iPhone（推测含 iPad）"——跨代适用性从推导升级为当事人表态。*  
   ✅ *10-24 更新：Duo（A20 Pro/MIE/27.1）发售日即被攻破，二进制比对确认链相关漏洞横跨 27.0.1→27.1 未修——跨版本、跨设备实锤各+1。*
3. **iPhone 12 保留在 iOS 27 支持列表**——A14 仍是 iOS 27 一等支持设备，27 的内核/CoreTrust/驱动栈均含 A14 构建目标，无硬件代差壁垒。
4. **性能优化回溯证明底层路径连续**——iOS 27 的 CPU 调度器优化明确回溯至 iPhone 11（比 A14 更老），说明底层代码跨代连续，移植阻力小。
5. **AI 门槛不构成漏洞豁免**——Apple Intelligence 只是功能门控（15 Pro+），不是独立系统构建；非 AI 设备跑着相同的内核与签名/磁盘子系统，链不依赖 AI 功能。
6. **一手口径佐证**——heise 原文即称「适用于直至 iOS 27.0.1 的版本」；社区知情人称测试覆盖 18 Pro 与 17e 两颗芯片、「≤27.0.1 都是好消息」（二手猜测，方向与上述逻辑一致）。

**保留的三个折扣**：① 版本覆盖（≤26.x）从未演示，漏洞是否 27 新引入纯未知；② 链通用 ≠ 工具通用，A14/A15 适配仍需社区后续工作；③ 发布模式已明朗：**无终端工具、披露+可能 PoC**（Esser 10-19 专访），11-04 lambda 给出首个具体 ETA（约一周内发 KRW+write-up，V30）。注意：A14 的 Titan 绕过仅限 ≤17.4b3，不构成 27.0.1 的出路；11-01 的 AstraSaw 自称"全系 iPhone"但已定档高风险存疑，同样不改变上述折扣。

## 芯片阵营图：你的设备在哪一队？

公开越狱停在 17.3.1、26.x 无门、指望 ≤27.0.1 窗口的设备，按芯片分四个阵营：

| 阵营 | 芯片 | 代表设备 | 距越狱还缺什么 |
|---|---|---|---|
| 🟥 **PPL 阵营**（10-21 出现转机信号） | A14、M1 | iPhone 12 全系、iPad Air 5、iPad Pro 2021 | 原判**两块拼图**：PPL 绕过 + 新内核漏洞。✅10-21：Esser 称链绕过 MIE 且"适用任何 27.0.1 设备"——PPL 绕过**可能已在链中**，待 write-up 验证；若披露不含 PPL 内容则回到原判（注：Titan 绕过仅限 ≤17.4b3，对 27.0.1 无效） |
| 🟨 **SPTM 阵营**（缺口最小）⭐ | A15、A16、A17 Pro、A18/Pro、M2/M3/M4 | iPhone 13→16 全系、SE3、iPad mini 6/7、iPad Air (M2/M3)、iPad Pro (M4) | **只差一块**：>26.0.1 的新内核漏洞（SPTM 绕过已有实物，≤27.0.1 有效）；lambda KRW 兑现后即看独立 SPTM 绕过能否接上 |
| 🟩 **MIE 阵营**（已演示 ×2） | A19（含 17e）、A20 Pro | iPhone 17 全系、iPhone 18 Pro、**iPhone Duo** | Calif 已直接演示（18 Pro @ 27.0.1 + **Duo @ 27.1 发售日**），等披露后第三方工具即可 |
| 🟦 **A12/A13**（首批候选 ⭐11-04） | A12/A13 | iPhone XS/XR/11、SE2、iPad 8/Air 3/mini 5 | 已在岸上（26.0.1 双保险）；**11-04 起成 27.0.1 首批 Dopamine 候选**：momentarius PPL 绕过现成，只欠 lambda KRW 兑现，opa334 已承诺快速适配（V31） |

要点：SPTM 阵营覆盖 iPhone 13→16 整整四代，人多势众、缺口最小；但 **11-04 后落地顺序明朗：A12/A13 先行（防护层现成），SPTM/PPL 阵营排队等 n-day SPTM 绕过**——opa334 甚至认为该绕过超出当前 LLM 能力（无样本，非 Coruna 可比）。PPL 阵营（A14/M1）的下一关键观察点不变：**lambda write-up 是否含 PPL/A14 相关内容**。⚠️ **27.2 已于 10-27 发布**，链上大部分漏洞被修——27.0.1 签名窗口进入最后倒计时（11-02 仍在签）。

## 核查裁决速览

| 类别 | 数量 | 代表条目 |
|---|---|---|
| 核心事件属实（T1 级来源确认） | — | 演示流程、公司背景、技术细节多源一致 |
| 事实错误 E1–E3 | 3 | Fugu14 反例 / iPhone 11 未被砍 / 修补通道误判（部分证伪） |
| 需修正 P1–P6 | 6 | 性能数字为营销上限、Siri AI 门槛、usbliter8 工具链现状等 |
| 重大遗漏 O1–O3 | 3 | 用户 A14/A15 在 26.x 无越狱、26 分支仍在维护、升级不可逆 |
| 追踪新增 V7–V33 | 27 | SPTM 绕过 / 内核漏洞 / Esser 专访 / Duo 被攻破 / 链覆盖 27.1 / Chollima 代号 / 27.2 发布与 6 CVE / 独立复现 / 27.2 演示传闻 / 哈希承诺 / **AstraSaw 高风险存疑 / CASLR 勘误 / 27.0.1 在签 / ElleKit 就绪 / Din3zh=A13 bootrom 线（媒体误读）/ lambda 一周 KRW 发令枪 / opa334 A12·A13 适配承诺与分级词汇 / 锁定模式+OTA 27.2 跨版本持久化演示** |

## 仓库结构

| 路径 | 内容 |
|---|---|
| `report/full-audit-zh.md` | 完整审核报告（约 9,600 字，281 处引用，127 条文献） |
| `research/` | 10 份维度核查档案 + 交叉验证 + 洞察 + Duo 事件四路核查 + **AstraSaw 核查专档**（可复现证据链） |
| `data/claims.json` | 机器可读断言库：claim / verdict / confidence / sources |
| `data/timeline.json` | 事件时间线（持续更新） |
| `CONTEXT.md` | 领域术语表：链/零件/拼图/阵营/窗口/第三链/自述链评级/momentarius/分级词汇等概念的唯一权威定义 |
| `SKILL.md` | Agent 技能入口：AI 代理可将本仓库作为技能加载调用 |
| `CHANGELOG.md` | 事件进展更新记录 |

## 给 AI Agent 的入口

本仓库同时是一个 **Agent Skill**：将仓库 clone 到 agent 的 skills 目录（或打包为 `.skill` 文件安装），agent 在遇到「iOS 27 越狱 / Calif / Hexacon / iPhone Duo / AstraSaw / 26 vs 27 升级 / 我的芯片能不能越狱」类问题时会自动加载本档案的结论与引用。详见 `SKILL.md`。

## 免责声明

本仓库为新闻事实核查与安全研究档案，仅供信息参考；不提供、不评价任何越狱工具的获取方式；不构成任何升级/不升级的建议承诺。所有结论标注了置信度与时效。

---

## English abstract

Fact-check archive of the **iOS 27.0.1 untethered jailbreak demo** by Calif at Hexacon 2026 (Paris, Oct 16) — and the follow-up **iPhone Duo release-day compromise** (Oct 23). The events are verified (primary sources + kernel build match with real panic logs), but the viral report around them contains 3 factual errors, 6 overstatements, and 3 critical omissions — most importantly: the Dopamine 3.0 jailbreak for iOS 26.0–26.0.1 only covers A12/A13 chips, so A14/A15 devices gain nothing by staying on iOS 26. **Latest (Nov 4): Calif member lambda (@pwnlambda / Anna Groza, identity confirmed via Apple's CVE-2026-112611 credit) announced an iOS kernel read/write exploit + writeup within roughly a week; opa334 pledged quick Dopamine support for A12/A13 via momentarius, while A14+ still needs the n-day SPTM bypass ("above the silicon ceiling" for LLMs). Calif also demoed cross-version persistence: Lockdown Mode on, OTA update to 27.2, reboot — tweaks still alive (mechanism undisclosed). Verdicts for A14/A15 devices unchanged: stay on 27.0.1 (still signed as of Nov 2), avoid 27.2. Previously: AstraSaw rated HIGH-RISK DOUBTFUL (see `research/astrasaw-audit.md`); Din3zh's root shots = known A13 bootrom line (media misreading); 27.2 shipped with 163 CVEs (6 Calif credits); CVE-2026-112705 independently reproduced (DoS-grade).** Includes a chip-camp map (PPL vs SPTM vs MIE), an ecosystem progress diagram, and a scam-pattern field guide. Full audit in `report/` (Chinese), machine-readable claims in `data/claims.json`, living timeline in `data/timeline.json`. Data as of 2026-11-04; evolving story.

License: CC BY 4.0 — attribution required when reusing content.
