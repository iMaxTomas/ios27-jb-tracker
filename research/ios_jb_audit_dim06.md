# iOS 越狱报告核查（维度 06）：A14/A15 安全机制、越狱滞后成因与 Calif iOS 27.0.1 链可移植性

- 核查日期：2026-10-17（UTC）
- 核查方法：网络搜索 26 次独立查询；交叉比对 Apple 官方文档、越狱工具官方发布说明、多家独立媒体报道
- 被核查陈述（报告原文）："硬件防御每代都在变，Calif 的 iOS 27.0.1 越狱链（演示于 iPhone 18 Pro/T8160）大概率不适配 A14/A15"
- 设备背景：iPhone 12 Pro Max（A14）、iPad mini 6（A15），均运行 iOS/iPadOS 26.1

**总体裁决：部分成立（PARTIAL PASS）。** "硬件防御每代都在变"属实（高置信度）；"大概率不适配 A14/A15"属于**方向合理但证据不足的推断**——在 Calif 未披露漏洞的情况下，任何一方都无法给出确切结论；且历史先例显示纯软件链从新机移植到旧机相当常见，旧芯片因缺少 MIE 等新缓解在某些环节反而更容易。更严谨的表述应为"**是否适配未知；若公开，移植需要补齐 iOS 27 上 A14 的 PPL bypass 与 A15 的 SPTM bypass 等按芯片缺失的环节**"。

---

## 〇、背景事实层核查（先确认地基）

### 0.1 Calif 的 iOS 27.0.1 演示属实
**裁决：属实。置信度：高。**

- 2026-10-16，安全公司 Calif（Calif.io）在巴黎 Hexacon 2026（Synacktiv 主办）开场 keynote（Bruce Dang 与 Thai Duong 主讲）现场演示 iPhone 18 Pro 上的 untethered 越狱：重启后自动恢复 root，SSH 2222 端口 `id` 返回 `uid=0`；演示机信息为 **iPhone19,2 / T8160 / iOS 27.0.1 build 24A446 / Darwin 27.0.0 (XNU 13432.2.10)**。[^1^][^2^][^3^]
- 逐字摘录："The uname output showed Darwin Kernel 27.0.0 built on XNU 13432.2.10 for the T8160 chip, with the model listed as iPhone19,2. The sw\_vers command confirmed iOS 27.0.1, build 24A446"（CyberSecurityNews，2026-10-16）[^1^]
- 漏洞未披露、无 CVE、无公开工具；Calif newsletter 称四项演示 "Everything will be released in due time."[^4^][^5^]
- heise 称其为 iOS 9 以来首个 reboot-proof（untethered）越狱；AI（Claude/Mythos） reportedly 参与发现漏洞链。[^2^][^6^]

### 0.2 时间线锚点
- iOS 27：2026-09-14 发布，修复 120+ 漏洞（含 20 个内核漏洞）。[^7^][^1^]
- iOS 27.0.1（build 24A446）：2026-09-28 发布；iOS 27.1 已在 beta。[^8^][^5^]
- T8160 = A20 Pro（iPhone 18 Pro/Pro Max/Duo 所用芯片，TSMC N2，ARMv9.3-A）。[^9^]

---

## 问题 1：A14/A15 关键安全机制的代际差异（PAC / PPL / SPTM / MIE）

### 1.1 PAC（Pointer Authentication Codes）
**裁决：A12 首发，A14/A15 均具备。置信度：高。**

- 逐字摘录："In 2018, Apple shipped the **A12 Bionic** chip with Pointer Authentication Code (PAC). This made them the first in the industry to deploy this protection at scale."（8ksec MIE 深度分析，2026-01-27）[^10^]
- 因此 iPhone 12 Pro Max（A14）与 iPad mini 6（A15）都有 PAC；任何越狱链在这两台设备上同样需要 PAC bypass 或规避手段。

### 1.2 PPL（Page Protection Layer）
**裁决：2017 年（A10 代）引入；A14 仍使用 PPL（APRR 机制）。置信度：高。**

- 逐字摘录："Introduced in 2017, Apple designed PPL to deny user-space code modification after successful code signature verification… This PPL-trampoline is the only entry point to PPL"（arXiv:2510.09272，2025-10-10）[^11^]
- 同文明确："**SPTM has replaced the PPL mitigation on A15 / M2 or newer SoCs**"——即 **A14 不在 SPTM 覆盖范围内，仍是 PPL 设备**。[^11^]

### 1.3 SPTM / TXM（Secure Page Table Monitor / Trusted Execution Monitor）
**裁决：硬件上仅 A15/M2 及更新 SoC 支持，取代 PPL；软件上自 iOS 17.0b1 首次出现、逐步推广。置信度：高（Apple 官方原文）。**

- Apple Platform Security 官方逐字摘录："Secure Page Table Monitor (SPTM) is supported in the **A15 or later and M2 or later** SOCs and **replaces Page Protection Layer** on supported platforms."（support.apple.com，2026-01-28 更新）[^12^]
- 官方续文："In the A15 or later and M2 or later SOCs, SPTM (in combination with TXM) replaces the PPL, providing a smaller attack surface that doesn't rely on trust of the kernel, even during early boot. SPTM relies on new silicon primitives that are an evolution of the Fast Permission Restrictions that PPL utilizes, and are available only on the processors listed"[^13^]
- 推广时间线（社区一手分析）："SPTM was first introduced in iOS 17.0b1, and has gradually replaced PPL on more and more devices… if you look e.g. at A15 on iOS 15 or 16, you will see PPL, whereas in 17 or later, you will see SPTM."（Nicolás Alvarez，2025-06-22）[^14^]

**关键结论（对报告的一处细化）**：报告把 A14/A15 并列，但二者在页表防护上分属两代：
- iPhone 12 Pro Max（A14）= PAC + **PPL**（无 SPTM 硬件）；
- iPad mini 6（A15）= PAC + **SPTM/TXM**（iOS 17 起）。
这一差异直接决定问题 2 与问题 4 中两台设备所需的 bypass 类型不同。

### 1.4 MIE（Memory Integrity Enforcement）
**裁决：报告表述需修正——MIE 首发于 A19/A19 Pro（iOS 26）与 M5（macOS 26），并非"A17/M 系列"；A14/A15 没有 MIE。置信度：高。**

- Apple 安全研究官方博客（2025-09-09）："Memory Integrity Enforcement (MIE) is the culmination of an unprecedented design and engineering effort… the most significant upgrade to memory safety in the history of consumer operating systems."[^15^]
- 逐字摘录："MIE is currently available **only in the A19 and A19 Pro chips** used in the iPhone 17 and iPhone Air."（TidBITS Talk，2025-09-12）[^16^]
- 逐字摘录："Memory Integrity Enforcement… ships on the **A19 and A19 Pro from iOS 26** and on the **M5 from macOS 26**."（sigreturn.com，2026-06-21）[^17^]
- 技术构成：MIE = 安全类型感知分配器（kalloc_type / xzone malloc）+ 同步模式 EMTE + Tag Confidentiality Enforcement；对越界写/UAF 在写入瞬间硬件 fault。[^10^]
- **对报告句的裁决**："A17/M 系列之后新增的 MIE"若解读为"A17 这一代之后（即 A19 起）新增"，方向正确；若解读为"自 A17/M 系列起新增"，则**错误**——A17 Pro、A18、M1–M4 均无 MIE。精确表述：MIE 始于 A19/A19 Pro 与 M5。
- T8160/A20 Pro 是否含 MIE：无 Apple 一手逐字确认；第三方（efani LinkedIn）称 "Memory Integrity Enforcement on A20/A20 Pro… Apple introduced MIE with the A19"。**标注：推断（置信度中高）**，依据：MIE 是苹果五年工程投入的主推防线，没有理由在继任芯片上移除；且 Calif 此前曾在 2026 年 5 月演示 5 天内绕过 M5 的 MIE，说明其研究路线正对 MIE 类防御。[^18^][^19^]

---

## 问题 2：为何近年越狱对 A14+ 明显滞后/缺失——"A12/A13 反而更容易"是否成立

### 2.1 现象本身属实
**裁决：属实。置信度：高（工具官方数据）。**

- Dopamine 官网逐字摘录："Supported on iOS 15.0 - 17.3.1 (A14 - A17 / M1 - M2) · Supported on iOS 15.0 - 18.7.1 and 26.0 - 26.0.1 (A8 - A13)"（ellekit.space）[^20^]
- Dopamine 3.0（2026-08-07）发布说明逐字摘录：
  - "Add Titan PPL/SPTM bypass by @alfiecg24, introduces support for all devices running 16.5.1 - 17.3.1"
  - "Add momentarius PPL bypass by @staturnzz and @TheRealClarity, introduces support for **A12/A13** devices running **16.6 - 18.7.1, 26.0 - 26.0.1**"
  - "obviously only works up to 17.3.1 on real devices **due to lack of an SPTM bypass on >=17.4**"[^21^]
- 后续 3.0.5（2026-08-13）又为 A12/A13 增加 iOS 26.1 beta 1–3。[^22^]
- 即：截至 2026-10-17，**A14/A15 设备在任何 iOS 26 正式版上都没有公开越狱**，而 A12/A13 可到 26.0.1。第三方综述亦确认："there is no public jailbreak for iOS 26.1 or later, or for iPhone 12 and newer models on any iOS 26 build"（techengage，2026-09-14）[^7^]

### 2.2 成因分解
**(a) ≥17.4 的 SPTM bypass 空缺是 A15+ 真机的硬墙——属实，直接引文。**
Dopamine 3.0 说明原文（见上）[^21^]；onejailbreak 转述："SPTM devices get general support up to 27.0 betas, though real hardware is still capped at 17.3.1 because no SPTM bypass exists for 17.4 and later."[^23^]

**(b) momentarius 是 PPL bypass，只落地到 A12/A13——属实（范围），具体技术原因未公开（标注：推断）。**
发布说明明确其覆盖仅 A12/A13。[^21^] 为何不含 A14（同为 PPL 设备）官方未解释。推断依据：研究者 diff 发现 iOS 17.0→17.5.1 间 PPL 被静默加固（新增 `pmap_cs_runtime` 子系统、PPL trap 校验等），"likely a Dopamine-style physical R/W PPL bypass" 的对应修复[^24^]；可能使同一 PPL bypass 手法在 A14 的 17.4+ 上失效。

**(c) usbliter8 BootROM 漏洞只覆盖 A12/A13——属实，且有"A14+ 为何免疫"的直接解释。置信度：高。**
- 逐字摘录："There's a SecureROM bug (released by Paradigm Shift)… **It only supports A12 and A13** (S4, S5 on Apple Watch series are also supported)."（usbliter8-fun README，2026-07-08）[^25^]
- 逐字摘录："**A14 and later chips appear to configure DART correctly in SecureROM, making the same route unexploitable.**"（Infosecurity Magazine，2026-06-22）[^26^]
- 该漏洞 2026-06-18/19 披露，基于 Synopsys DWC2 USB 控制器 DMA 缓冲区下溢 + A12/A13 的 USB DART bypass 配置；不可软件修复；原始披露博文因 Magnet Forensics 起诉 Paradigm Shift（商业秘密）而被删除。[^27^][^26^]
- 社区已用它在 iPhone 11 Pro（A13）上启动越狱版 iOS 27.0 beta 2（tethered、需 RP2350 硬件 + DFU，代价是 SEP/Wi-Fi/基带/蓝牙失效）——这是"旧芯片反而先上 iOS 27"的直接原因之一。[^28^][^29^]

**(d) 历史模式对照**："A12/A13 比 A14/A15 更容易"是**反常但真实存在的窗口期现象**，并非历史常态。常态是"越新越难"（mitigation 叠加）。当前反常来自两个巧合叠加：① 唯一可用的后期 PPL bypass（momentarius）恰好只做了 A12/A13；② 七年来首个新 BootROM 漏洞（usbliter8）恰好只存在于 A12/A13 硅片。中文社区综述亦持相同解读："为什么 A11 及以前这么从容？因为 Checkm8 漏洞藏在 BootROM…而 A12 及以上机型没有这道硬件后门"——2026 年后 A12/A13 因 usbliter8 成为新的"幸运代"。[^30^]

**问题 2 总裁决：属实（高置信度）。** 报告提示的两个机制（SPTM/PPL bypass 的代际可用性、usbliter8 仅覆盖 A12/A13）均被一手来源证实。

---

## 问题 3：历史上"先在最新机型演示、后适配旧机"的概率与案例

**裁决：纯软件链跨芯片移植是历史常态（高置信度）；硬件漏洞链不可跨代（高置信度）。报告隐含的"新机型演示 ⇒ 大概率不适配旧机"缺乏历史支撑。**

支持移植常见的证据：
- Fugu14（2021，iOS 14.3–14.5.1 untethered 链）："Fugu14 **should support all arm64e devices (iPhone XS and newer)**… Support for lower versions… can be added by editing… offsets.swift"——同一代码跨 A12–A14。[^31^]
- Fugu15（2022，Linus Henze）："iOS 15 jailbreak up to 15.4.1 · **Supports A12+ (support for other devices can be added)**"，实测覆盖 iPhone XS Max、11、12、12 Pro Max、13。[^32^][^33^]
- 其衍生 Dopamine 2.0 覆盖 A8–A16/M2；kfd 内核原语被评估可用于"all modern devices, including iPhone 14 Pro"。[^21^][^34^]
- 攻击性全链同样跨代设计：Operation Triangulation "was designed to support **both old and new iPhones** and included a Pointer Authentication Code (PAC) bypass for exploitation of recent models."（Kaspersky Securelist，2023-12-27）[^35^]

不支持移植的证据（反例类型）：
- BootROM 类：checkm8 仅 A5–A11；usbliter8 仅 A12/A13（+S4/S5）——硅片相关，永不可移植。[^26^][^36^]
- 会议演示不转化为工具的先例存在：Hexacon 2022 上 Luca Todesco 演示过 iOS 16.1 beta 越狱，从未公开；"Conference demos prove the bug class exists. They do not hand you a tool you can run on your own phone the next morning."（ONE Jailbreak，2026-10-16）[^8^]

方向性推断（标注：推断，置信度中高）：**新→旧移植通常比旧→新容易**，因为旧芯片 mitigation 更少。具体到本案：A14/A15 无 MIE，若 Calif 链在 A20 上必须包含 MIE 对抗环节（EMTE 使 UAF/越界写在写入瞬间 fault），该环节在 A14/A15 上直接不需要，内存破坏原语的可靠性反而更高。[^10^][^16^]

**问题 3 总裁决**：历史先例偏向"纯软件链最终覆盖多代芯片"。报告"大概率不适配"一语若作为概率判断，与历史基线（软件链常移植）相悖；但作为"在漏洞细节未知时的谨慎措辞"可以接受。置信度：报告的绝对化表述**不成立**，修正后为推断。

---

## 问题 4：Calif 链若公开，移植到 A14/A15 需要哪些额外工作；社区讨论

### 4.1 当前可知与不可知
- 不可知（一手）：漏洞列表、CVE、入口向量、持久化机制、是否含 MIE/SPTM/PAC bypass——全部未披露。[^1^][^4^]
- heise 的权威判断（经由转述，中英文两个版本一致）："这条漏洞链能否在更早的芯片上运行并不清楚，**因为硬件防御每一代都有变化**"（heise online 原文转引）[^19^]——这正是报告"硬件防御每代都在变"的直接出处，属实。

### 4.2 理论工作量分解（标注：推断；依据为公开的机制差异与既有越狱工程惯例）
1. **Offsets / patchfinding 适配**：每设备、每 build 必做（惯例；Dopamine 用 XPF 自动化大部分）。[^21^][^31^]
2. **PAC 环节**：A14/A15 同为 PAC 设备（A12+），若 Calif 的 PAC 手段是按 OS 版本而非按硅片实现，迁移成本中等。[^10^]
3. **A15（iPad mini 6）需 iOS 27 上的 SPTM bypass**：公开界在真机 ≥17.4 上完全没有 SPTM bypass（Dopamine 说明原文）[^21^]；但 A20/T8160 同样是 SPTM 芯片，**Calif 链必然已解决 SPTM**（或绕过页表路径），其 SPTM 手段若按版本实现则可能直接下移——这是"新→旧"移植可能反而顺的关键点。
4. **A14（iPhone 12 Pro Max）需 iOS 27 上的 PPL bypass**：公开界 momentarius 不含 A14；A14 在 17.4+ 的 PPL 路径另有静默加固。[^21^][^24^]
5. **MIE 环节在 A14/A15 上不需要**（两台设备均无 MIE），原语层面难度下降。[^16^]
6. **untether/持久化**：机制未知，若依赖 T8160 特有行为则可能无法下移——最大不确定性。[^4^]
7. **版本错配（针对本案用户）**：用户设备在 **26.1**，而 Calif 链针对 **27.0.1**（2026-09-28 发布，27.1 已在 beta）。即便链公开且移植成功，用户还需能移动到 27.0.1——签名窗口大概率已关闭或即将关闭（推断，依据：Apple 已停签 26.0/26.0.1 的惯例[^37^]）。

### 4.3 社区讨论实况
- r/jailbreak 热帖（2026-10-16，202 votes / 104 comments；镜像版 128 upvotes / 69 comments）："It looks absolutely real. **But I doubt the it will ever become public.** Any thoughts?"——社区主流情绪是"真实但大概率不会公开"。[^38^]
- r/jailbreak 另有衍生讨论（如 "iPhone duo jailbreak"，2026-10-17）："Do you think calif will work on the iPhone duo or will the firmware be unsupported"——说明社区已在讨论跨机型适用性，但无任何技术结论。[^39^]
- ONE Jailbreak 记录社区第一时间猜测该演示是否与本月早些时候 Apple 封堵的 TSS/服务端签名 bug（曾让研究者在 iOS 27 上获得 root 文件系统访问、含 iPhone 18 Pro Max 后期 beta）同源，结论是不同路径。[^8^]
- 行业侧：Cellebrite 数周前刚宣称 iOS 27 无法在任何现有真机上越狱、iOS 18 以上无可靠公开越狱，本次演示直接挑战该结论。[^2^]
- X 平台：Billy Ellis 的现场视频（@bellis1000）与 @FCE365 等的转述为主要一手素材；截至核查时未见知名越狱开发者（如 opa334）对 A14/A15 可移植性发表实质评论。（标注：搜索覆盖有限，reddit/X 正文无法直接抓取，仅依据搜索快照。）[^40^]

**问题 4 总裁决：推断（置信度中）。** "大概率不适配"无法证实；同样"会适配"也无法证实。可证实的只有：移植必须补齐 iOS 27 上 A14 的 PPL bypass 与 A15 的 SPTM bypass，而这两个环节目前公开界均为空白；MIE 的缺失对旧机是有利因素。

---

## 问题 5：iPhone 12 Pro Max 与 iPad mini 6 是否支持 iOS/iPadOS 27

**裁决：两者均支持。置信度：高（含 Apple 官方页面）。**

- Apple 官方支持页《iPhone models compatible with iOS 27》列表含 "iPhone 12 mini · iPhone 12 · **iPhone 12 Pro** · iPhone 12 Pro Max…"。[^41^]
- Macworld 全列表逐字："**iPhone 12 Pro (Max) 2020 — iOS 27**"；iOS 27 于 2026-09-14 开放，最低支持到 iPhone 11 系列。[^42^]
- everymac 列明 iPhone 12 Pro Max 全部型号（A2342/A2410/A2411/A2412）支持 iOS 27。[^43^]
- iPadOS 27：Tom's Guide 官方兼容列表逐字含 "**iPad mini (6th generation)**"；Stuff/everymac/tapsmart 一致（iPadOS 27 砍掉的是 iPad 8、mini 5、Air 3、11" Pro 1代、12.9" Pro 3代）。[^44^][^45^][^46^][^47^]
- 注意：两台设备升级 iOS/iPadOS 27 后**均无 Apple Intelligence**（需 M1/A17 Pro 以上）。[^46^][^47^]

---

## 汇总裁决表

| 报告陈述 | 裁决 | 置信度 |
|---|---|---|
| 硬件防御每代都在变 | ✅ 成立（heise 原话 + Apple 官方 SPTM/MIE 代际文档） | 高 |
| MIE 为"A17/M 系列之后"新增 | ⚠️ 需修正：MIE 始于 **A19/A19 Pro（iOS 26）与 M5（macOS 26）**；A17/A18、M1–M4 均无 | 高 |
| A14/A15 近年越狱支持滞后/缺失（Dopamine 仅 17.3.1） | ✅ 属实；A12/A13 反到 26.0.1(+26.1β) 亦属实 | 高 |
| 反常现象与 SPTM/PPL bypass 可用性、usbliter8 仅覆盖 A12/A13 有关 | ✅ 成立，且有一手解释（A14+ SecureROM 正确配置 DART） | 高 |
| Calif 链"大概率不适配 A14/A15" | ⚠️ **推断，证据不足**；更严谨表述：适配性未知——若公开且为纯软件链，历史先例偏向可移植，但需补齐 iOS 27 上 A14 PPL bypass、A15 SPTM bypass 两项空白；A14/A15 无 MIE 反而是有利因素 | 中（作为谨慎措辞可接受；作为事实断言不成立） |
| iPhone 12 Pro Max / iPad mini 6 支持 iOS/iPadOS 27 | ✅ 均支持（无 Apple Intelligence） | 高 |

## 核查局限
1. reddit 与 heise.de 直链在本环境被访问审计拦截，社区讨论内容依赖搜索引擎快照与多家媒体转述，未逐条读取评论全文。
2. T8160/A20 Pro 是否内置 MIE 无 Apple 一手逐字确认（推断，中高置信度）。
3. Calif 未披露任何漏洞细节，问题 4 的全部"移植工作量"为基于公开机制差异的推理，非实证。
4. momentarius 不覆盖 A14 的确切技术原因未公开（推断：与 17.4+ PPL 静默加固相关）。

---

## 引用来源

[^1^]: CyberSecurityNews — "iOS 27.0.1 Untethered Jailbreak Demonstrated on iPhone 18 Pro", 2026-10-16. https://cybersecuritynews.com/ios-27-0-1-jailbreak-demonstrated/
[^2^]: GBHackers — "Researchers Demonstrate iOS 27.0.1 Untethered Jailbreak on iPhone 18 Pro", 2026-10-16. https://gbhackers.com/ios-27-0-1-untethered-jailbreak/
[^3^]: Mallory — "Calif Demonstrates Reboot-Persistent iOS 27.0.1 Jailbreak on iPhone 18 Pro", 2026-10-16. https://mallory.ai/stories/01a144a5-33e9-786d-9d6a-311be6bd2189
[^4^]: explainx.ai — "Calif.io Jailbreaks iOS 27.0.1 on iPhone 18 Pro: What Is Claimed vs. Verified", 2026-10-16. https://explainx.ai/blog/calif-io-ios-27-0-1-untethered-jailbreak-iphone-18-pro-hexacon-ai-claimed-vs-verified-2026
[^5^]: Glitchwire — "Calif Demonstrates Untethered iOS 27.0.1 Jailbreak at Hexacon…", 2026-10-16. https://glitchwire.com/news/calif-demonstrates-untethered-ios-2701-jailbreak-at-hexacon-hints-at-signal-and/
[^6^]: Yahoo Tech/Decrypt — "Hackers Jailbreak iPhone 18 Pro on Stage, and It Survives a Reboot", 2026-10-16. https://tech.yahoo.com/cybersecurity/articles/hackers-jailbreak-iphone-18-pro-193604534.html
[^7^]: TechEngage — "iOS Jailbreak History: Every Major Tool From 2007 to 2026", 2026-09-14. https://techengage.com/ios-jailbreaking-complete-history/
[^8^]: ONE Jailbreak — "Calif Demos Untethered iOS 27.0.1 Jailbreak on iPhone 18 Pro", 2026-10-16. https://onejailbreak.com/blog/calif-demos-untethered-ios-27-0-1-jailbreak-on-iphone-18-pro/
[^9^]: namu.wiki — "Apple Silicon/A 시리즈 — A20 Pro (APL1Y01-T8160)", 2026-09-18. http://en.namu.wiki/w/Apple%20Silicon/A%20%EC%8B%9C%EB%A6%AC%EC%A6%88
[^10^]: 8ksec — "Memory Integrity Enforcement (MIE) on iOS Deep Dive", 2026-01-27. https://www.8ksec.io/mie-deep-dive-kernel/
[^11^]: arXiv:2510.09272 — XNU 内核安全研究（PPL/SPTM 章节）, 2025-10-10. https://arxiv.org/html/2510.09272v1
[^12^]: Apple Platform Security — "Apple SoC security", 2026-01-28. https://support.apple.com/guide/security/apple-soc-security-sec87716a080/web
[^13^]: Apple Platform Security — "Operating system integrity", 2026-01-28. https://support.apple.com/guide/security/operating-system-integrity-sec8b776536b/web
[^14^]: Nicolás Alvarez (Mastodon) — SPTM 推广时间线, 2025-06-22. https://social.treehouse.systems/@nicolas17/114727754975559614
[^15^]: Apple Security Research — "Memory Integrity Enforcement: A complete vision for memory safety in Apple devices", 2025-09-09. https://security.apple.com/blog/memory-integrity-enforcement/
[^16^]: TidBITS Talk — "Memory Integrity Enforcement Bolsters iPhone Security", 2025-09-12. https://talk.tidbits.com/t/memory-integrity-enforcement-bolsters-iphone-security/31894
[^17^]: sigreturn.com — "Apple internals #0: The Apple security stack", 2026-06-21. https://sigreturn.com/blog/apple-security-stack/
[^18^]: efani (LinkedIn) — "iPhone 18 Security Changes to Watch: MIE…", 2026. https://www.linkedin.com/posts/efani_iphone18-apple-wwdc-activity-7502289026482139136-mOOB
[^19^]: Pasquale Pillitteri（转引 heise online）— "Calif Unveils iPhone 18 Pro Jailbreak on iOS 27.0.1 at Hexacon"（英文版 FAQ #2；中文版同文）, 2026-10-16. https://pasqualepillitteri.it/en/news/24540/jailbreak-iphone-18-pro-ios-27-calif-hexacon ; https://pasqualepillitteri.it/zh/news/24573/jailbreak-iphone-18-pro-ios-27-calif-hexacon
[^20^]: Dopamine 官网 — ellekit.space/dopamine（访问于 2026-10-17）. https://ellekit.space/dopamine/
[^21^]: The Apple Wiki — "Dopamine"（含 3.0 发布说明全文）, 2026-10-12. https://theapplewiki.com/wiki/Dopamine
[^22^]: iClarified — "Dopamine 3.0 Jailbreak Released With iOS 17.3.1 Support, iOS 26 on A12/A13", 2026-08-07. https://www.iclarified.com/101697/dopamine-30-jailbreak-released-with-ios-1731-support-ios-26-on-a12a13
[^23^]: ONE Jailbreak — "Dopamine 3.0 Jailbreak With iOS 26 Support", 2026-08-07. https://onejailbreak.com/blog/dopamine-3-0-jailbreak-out-with-ios-26-support/
[^24^]: GitHub Somisomair/xnu-vm-map-cur-max-bug — iOS 17.0→17.5.1 PPL 静默加固 diff, 2026-03-28. https://github.com/Somisomair/xnu-vm-map-cur-max-bug
[^25^]: GitHub 34306/usbliter8-fun — README, 2026-07-08. https://github.com/34306/usbliter8-fun/blob/main/README.md
[^26^]: Infosecurity Magazine — "Unpatchable BootROM Flaw Impacts Apple A12, A13 Chips", 2026-06-22. https://www.infosecurity-magazine.com/news/apple-bootrom-exploit-a12-a13/
[^27^]: 什么值得买 — "iOS 27 越狱还没消息…usbliter8 时间线（含 Magnet Forensics 诉讼）", 2026-09-21. https://post.smzdm.com/p/awwzz93k/
[^28^]: CyberSecurityNews — "Booted Jailbroken iOS 27 iPhone 11 Pro", 2026-07-27. https://cybersecuritynews.com/booted-jailbroken-ios-27-iphone-11-pro/
[^29^]: ONE Jailbreak — "usbliter8-fun: iOS 27 Jailbreak on iPhone 11 Pro", 2026-07-25. https://onejailbreak.com/blog/usbliter8-fun-ios-27-jailbreak-on-iphone-11-pro/
[^30^]: CSDN — "iOS 越狱完整指南：iOS 17 到 iOS 26 按芯片选工具", 2026-08-26. https://blog.csdn.net/gitblog_00466/article/details/164085296
[^31^]: GitHub neoncloud/Fugu14_onlinebuild — Fugu14 README, 2021-10-27. https://github.com/neoncloud/Fugu14_onlinebuild
[^32^]: Objective by the Sea v5 — Linus Henze《Fugu15 - The Journey to Jailbreaking iOS 15.4.1》slides. https://objectivebythesea.org/v5/talks/OBTS_v5_lHenze.pdf
[^33^]: The Apple Wiki — "Fugu15". https://theapplewiki.com/wiki/Fugu15
[^34^]: iDevice Central — "KFD Exploit Compiled for iOS 15.5 – 16.5 (A12+)", 2023-07-31. https://idevicecentral.com/jailbreak-tools/download-kfd-jailbreak-exploit-ipa/
[^35^]: Kaspersky Securelist — "Operation Triangulation: The last (hardware) mystery", 2023-12-27. https://securelist.com/operation-triangulation-the-last-hardware-mystery/111669/
[^36^]: Belkasoft — "Unc0ver: What you should know"（checkm8 仅 A5–A11）. https://belkasoft.com/what_you_should_know_about_unc0ver
[^37^]: iPhoneTweak — "Dopamine 3.0 rend iOS 26 enfin jailbreakable (A12/A13 only)"（"Apple a stoppé la signature de 26.0/26.0.1"）, 2026-08-08. https://iphonetweak.fr/2026/08/08/dopamine-3-0-rend-ios-26-enfin-jailbreakable-a12-a13-only
[^38^]: Reddit r/jailbreak — "Calif team presented untethered iOS 27 jailbreak at Hexacon"（202 votes / 104 comments；镜像 r/jailbreak_ 128 upvotes）, 2026-10-16. https://www.reddit.com/r/jailbreak/comments/1x7dwzb/calif_team_presented_untethered_ios_27_jailbreak/
[^39^]: Reddit r/jailbreak — "iPhone duo jailbreak", 2026-10-17. https://www.reddit.com/r/jailbreak/comments/1x7xmp8/iphone_duo_jailbreak/
[^40^]: X — @bellis1000 现场视频, 2026-10-16. https://x.com/bellis1000/status/2111028042495824337/photo/1
[^41^]: Apple Support — "iPhone models compatible with iOS 27". https://support.apple.com/guide/iphone/iphone-models-compatible-with-ios-27-iphe3fa5df43/ios
[^42^]: Macworld — "Can your iPhone run iOS 27? Here's the full compatibility list", 2026-09-14. https://www.macworld.com/article/1811287/which-version-of-ios-can-my-iphone-run.html
[^43^]: everymac — "iOS 27 Supported Devices List", 2026-09-20. https://everymac.com/systems/apple/iphone/iphone-faq/iphone-ios-27-supported-devices-features.html
[^44^]: Tom's Guide — "iPadOS 27 review: Compatible iPads", 2026-09-15. https://www.tomsguide.com/computing/ipads/ipados-27-review
[^45^]: Stuff — "iPadOS 27 system requirements", 2026-09-16. https://www.stuff.tv/features/ipados-27-system-requirements-will-it-run-on-your-apple-ipad/
[^46^]: everymac — "iPadOS 27 Supported Devices List", 2026-09-16. https://everymac.com/systems/apple/ipad/ipad-faq/ipados-27-supported-devices-ipad-system-requirements.html
[^47^]: Tapsmart — "Which devices are compatible with iOS 27, iPadOS 27, and more?", 2026-09-18. https://www.tapsmart.com/tips-and-tricks/2026-device-compatibility-guide/
