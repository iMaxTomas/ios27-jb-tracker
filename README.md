# iOS 27.0.1 越狱事件 · 事实核查与持续追踪

> **数据截至 2026-10-24 · 此为发展中事件，持续更新**
> 本仓库是对「2026-10-16 Calif 于巴黎 Hexacon 大会演示 iPhone 18 Pro 上 iOS 27.0.1 untethered（重启后仍有效）越狱」这一事件的**事实核查档案**：10 维度并行核查、交叉验证、127 条参考文献。不含任何越狱工具或漏洞利用内容。
>
> 📌 **最新（10-24）**：Calif 于 **iPhone Duo 发售日（10-23）当天官宣攻破 Duo**（"we popped the iPhone Duo on release day"，6 秒演示视频 79 万+观看）；二进制比对确认链相关漏洞在 27.1 仍未修补。**27.2 正式版预计 10-26/27 发布、27.0.1 仍在签**——想越狱请立即锁定 27.0.1，勿碰 27.2。详见 [CHANGELOG](CHANGELOG.md)。

## 生态进度工程图（2026-10-24 快照）

```
图例：██ 已完成  ▒▒ 进行中  ░░ 等待发令枪  ◆ 关键事件  ❓ 是非判定

时间轴        8月       10-16     10-23     27.2发布    未来
              │          │         │          │
【上游 · 漏洞供给】
Calif         ████████演示████攻破Duo██░░披露?◆
              (MIE绕过/untethered)    (等27.2补丁落地)
SPTM作者      ██绕过成品██░░░░░░░░░░░░░░░░░░  (实物已备,缺内核洞)
内核洞供应     ▒▒AI持续挖洞▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒  (ClearSword模式延续)

【中游 · 分析监测】
johnny        ▒▒▒▒二进制比对▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒  (持续监测链活性)
opa334        ▒▒PSA预警▒▒░░░░░░░░░░░░░░░░░░░  (等披露→可能做工具)

【下游 · 用户】
蹲守用户                        ░░刷27.0.1蹲守░░

【依赖关系】
Calif披露 ──────────→ opa334等第三方开工做工具
新内核洞 + 独立SPTM绕过 ──→ A15工具（不依赖Calif的B计划）
A14：❓ write-up是否含PPL/A14内容？
      ├─ 有 → 挂上主线进度条（披露后月计）
      └─ 无 → 孤岛，无人有动机从零造PPL绕过
```

**读图要点**：全图灰色（░░）都汇向 Calif 一个节点——整个生态的瓶颈不在技术，而在披露；A15 是唯一有 B 计划的阵营（独立 SPTM 绕过 + 新内核洞即可解套）；A14 的命运是❓判定菱形而非进度条。

[English abstract below](#english-abstract)

---

## 三句话结论

1. **事件是真的**：大会、公司、演示流程均经一手来源与六家媒体交叉确认，最强旁证是演示中的 XNU 内核 build 号与真实 iOS 27.0.1 用户 panic 日志逐字一致；但「演示属实 ≠ 漏洞公开 ≠ 工具将发布」。
2. **被审报告的升级结论方向没错，但论据有三处硬错误**：「自 iOS 9 以来首个 untethered」（2021 年 Fugu14 才是）、「iOS 27 砍掉 iPhone 11」（官方列表证伪）、「下一个 27.0.x 或 27.1 修补」（普通 iPhone 无 27.1，实际通道为 27.2）。
3. **对 A14/A15 设备（如 iPhone 12 Pro Max、iPad mini 6），『留在 26 等越狱』的收益是确定的零**：Dopamine 3.0 的 iOS 26.0–26.0.1 越狱仅限 A12/A13；A14/A15 公开越狱上限仍是 iOS 17.3.1。

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

**保留的三个折扣**：① 版本覆盖（≤26.x）从未演示，漏洞是否 27 新引入纯未知；② 链通用 ≠ 工具通用，A14/A15 适配仍需社区后续工作；③ 发布模式已明朗：**无终端工具、披露+可能 PoC**（Esser 10-19 专访），但 PoC 只是 "might"、无 ETA。注意：A14 的 Titan 绕过仅限 ≤17.4b3，不构成 27.0.1 的出路。

## 芯片阵营图：你的设备在哪一队？

公开越狱停在 17.3.1、26.x 无门、指望 ≤27.0.1 窗口的设备，按芯片分四个阵营：

| 阵营 | 芯片 | 代表设备 | 距越狱还缺什么 |
|---|---|---|---|
| 🟥 **PPL 阵营**（10-21 出现转机信号） | A14、M1 | iPhone 12 全系、iPad Air 5、iPad Pro 2021 | 原判**两块拼图**：PPL 绕过 + 新内核漏洞。✅10-21：Esser 称链绕过 MIE 且"适用任何 27.0.1 设备"——PPL 绕过**可能已在链中**，待 write-up 验证；若披露不含 PPL 内容则回到原判（注：Titan 绕过仅限 ≤17.4b3，对 27.0.1 无效） |
| 🟨 **SPTM 阵营**（缺口最小）⭐ | A15、A16、A17 Pro、A18/Pro、M2/M3/M4 | iPhone 13→16 全系、SE3、iPad mini 6/7、iPad Air (M2/M3)、iPad Pro (M4) | **只差一块**：>26.0.1 的新内核漏洞（SPTM 绕过已有实物，≤27.0.1 有效）；Calif 披露后或直接解套 |
| 🟩 **MIE 阵营**（已演示 ×2） | A19（含 17e）、A20 Pro | iPhone 17 全系、iPhone 18 Pro、**iPhone Duo** | Calif 已直接演示（18 Pro @ 27.0.1 + **Duo @ 27.1 发售日**），等披露后第三方工具即可 |
| 🟦 **A12/A13**（对照组） | A12/A13 | iPhone XS/XR/11、SE2、iPad 8/Air 3/mini 5 | 已在岸上：26.0.1 有 Dopamine 3.0 + usbliter8 双保险 |

要点：SPTM 阵营覆盖 iPhone 13→16 整整四代，人多势众、缺口最小，社区火力大概率优先落在这里；PPL 阵营（A14/M1）原本最渺茫，10-21 起出现实质转机信号——**下一关键观察点：Calif write-up 是否含 PPL/A14 相关内容**。⚠️ **27.2 正式版预计 10-26/27 发布**，届时链漏洞大概率被修、27.0.1 或将关签——窗口只剩数日。

## 核查裁决速览

| 类别 | 数量 | 代表条目 |
|---|---|---|
| 核心事件属实（T1 级来源确认） | — | 演示流程、公司背景、技术细节多源一致 |
| 事实错误 E1–E3 | 3 | Fugu14 反例 / iPhone 11 未被砍 / 修补通道误判（部分证伪） |
| 需修正 P1–P6 | 6 | 性能数字为营销上限、Siri AI 门槛、usbliter8 工具链现状等 |
| 重大遗漏 O1–O3 | 3 | 用户 A14/A15 在 26.x 无越狱、26 分支仍在维护、升级不可逆 |
| 追踪新增 V7–V15 | 9 | 独立 SPTM 绕过 / 需新内核漏洞 / Esser 专访三条 / Duo 发售日被攻破 / 链覆盖 27.1 / Chollima 代号传闻 / 27.2 倒计时 |

## 仓库结构

| 路径 | 内容 |
|---|---|
| `report/full-audit-zh.md` | 完整审核报告（约 9,600 字，281 处引用，127 条文献） |
| `research/` | 10 份维度核查档案 + 交叉验证 + 洞察 + Duo 事件四路核查（可复现证据链） |
| `data/claims.json` | 机器可读断言库：claim / verdict / confidence / sources |
| `data/timeline.json` | 事件时间线（持续更新） |
| `SKILL.md` | Agent 技能入口：AI 代理可将本仓库作为技能加载调用 |
| `CHANGELOG.md` | 事件进展更新记录 |

## 给 AI Agent 的入口

本仓库同时是一个 **Agent Skill**：将仓库 clone 到 agent 的 skills 目录（或打包为 `.skill` 文件安装），agent 在遇到「iOS 27 越狱 / Calif / Hexacon / iPhone Duo / 26 vs 27 升级 / 我的芯片能不能越狱」类问题时会自动加载本档案的结论与引用。详见 `SKILL.md`。

## 免责声明

本仓库为新闻事实核查与安全研究档案，仅供信息参考；不提供、不评价任何越狱工具的获取方式；不构成任何升级/不升级的建议承诺。所有结论标注了置信度与时效。

---

## English abstract

Fact-check archive of the **iOS 27.0.1 untethered jailbreak demo** by Calif at Hexacon 2026 (Paris, Oct 16) — and the follow-up **iPhone Duo release-day compromise** (Oct 23, "we popped the iPhone Duo on release day"). The events are verified (primary sources + kernel build match with real panic logs), but the viral report around them contains 3 factual errors, 6 overstatements, and 3 critical omissions — most importantly: the Dopamine 3.0 jailbreak for iOS 26.0–26.0.1 only covers A12/A13 chips, so A14/A15 devices gain nothing by staying on iOS 26. **Latest (Oct 24): binary diffing indicates the chain's bugs remain unpatched across 27.0.1→27.1; iOS 27.2 (expected Oct 26–27) reportedly fixes most of them — stay on ≤27.0.1 if you care about jailbreaking.** Includes a chip-camp map (PPL vs SPTM vs MIE) showing what each Apple silicon generation still lacks for a jailbreak. Full audit in `report/` (Chinese), machine-readable claims in `data/claims.json`, living timeline in `data/timeline.json`. Data as of 2026-10-24; evolving story.

License: CC BY 4.0 — attribution required when reusing content.
