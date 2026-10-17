# 维度 05 核查报告：iOS/iPadOS 26 越狱现状与 Dopamine 3.0 支持范围

**核查日期**：2026-10-17
**核查人**：事实核查员（iOS 越狱生态）
**用户设备前提**：iPhone 12 Pro Max（A14）+ iPad mini 6（A15），均运行 iOS/iPadOS 26.1；用户认为"旧的 26.0.1 已经可以越狱"
**核查方法**：独立网络检索 17 次 + 直接抓取 Dopamine 官网（ellekit.space）、Apple 官方安全公告（support.apple.com/125632、100100）、theapplewiki.com、ipsw.me 原始页面

---

## 总结论（先说答案）

| 问题 | 裁决 |
|---|---|
| iPhone 12 Pro Max（A14）在**任何** iOS 26 版本上能否越狱？ | **不能。** Dopamine 的 iOS 26 支持仅限 A12/A13；A14 的 Dopamine 上限是 iOS 17.3.1 |
| iPad mini 6（A15）在**任何** iPadOS 26 版本上能否越狱？ | **不能。** theapplewiki 官方兼容表中 iPad mini 6 在 26.0/26.0.1 栏明确标注 "No"，26.1 起全行 "No Tool Available" |
| iOS 26.1 及更高版本（任何芯片）有无越狱工具？ | **没有**（仅限 26.1 beta 1–3 曾在 A12/A13 上被 Dopamine 3.0.5 支持；正式版 26.1 无工具） |
| 降回 26.0.1 是否可行？ | **不可行。** 苹果已关闭 iOS 26.0.1 验证通道（2025 年 11 月 26.1 发布后不久即停签）；截至 2026-10-17 全机型仅 iOS 27.0.1 在签。且即便能降级，A14/A15 在 26.0.1 上依然无越狱 |
| 用户前提"旧的 26.0.1 已经可以越狱" | **半真半假、对其设备不适用。** 26.0/26.0.1 可越狱仅对 A12/A13 设备成立（iPhone 11 系列/SE2、iPad 8/9、mini 5、Air 3），不包括用户的 A14/A15 |

**置信度：高。** 核心结论同时得到 Dopamine 官方 GitHub/官网、Apple 官方安全公告、theapplewiki、iClarified、MacRumors 五方独立印证。

---

## 逐条裁决

### 断言 1：Dopamine 3.0 于 2026 年 8 月发布，首次支持 iOS 26.0/26.0.1，仅限 A12/A13

**裁决：基本属实（一处设备清单需修正）。置信度：高。**

- 发布日期为 **2026 年 8 月 7 日**（部分中文媒体写 8 月 8 日，系时区差异）。theapplewiki 逐字记载："Dopamine 3.0 released on 7 August 2026 … Support for iOS 16.6-18.7.1 and iOS 26.0-26.0.1 was unexpectedly added for A12/A13 devices, thanks to the release of Momentarius."[^1^]
- MacRumors（2026-08-07）："Lars Fröder, better known as opa334, today released Dopamine 3.0, which adds support for a number of newer firmware versions, including iOS 26.0 and iOS 26.0.1 on devices with A12 or A13 chips."[^2^]
- Dopamine GitHub 官方仓库描述（逐字）："A rootless semi-untethered jailbreak for iOS 15.0 - 17.3.1 (arm64e), iOS 15.0 - 18.7.1, 26.0 - 26.0.1 (A12/A13) and iOS 15.0 - 18.7.1 (arm64)."[^3^]
- Dopamine 官网（ellekit.space）逐字："Supported on iOS 15.0 - 17.3.1 (A14 - A17 / M1 - M2) / Supported on iOS 15.0 - 18.7.1 and 26.0 - 26.0.1 (A8 - A13)."[^4^]
- **芯片限制的确切范围**：SoC 层面为 **A12 / A12X / A12Z / A13**。但报告中"iPhone XS/XR/11 系列、SE2 等"的表述有误导：**iPhone XS/XR（A12）根本不能运行 iOS 26**——苹果官方 iOS 26 支持列表为 "iPhone 11 and later"[^5^]。因此在 iPhone 上，iOS 26 越狱实际只覆盖 **A13**：iPhone 11 / 11 Pro / 11 Pro Max / SE（第 2 代）；A12 的支持实际落在 iPad 上（iPad 第 8 代、iPad Air 3、iPad mini 5 可运行 iPadOS 26）。[^6^][^7^]
- iClarified 兼容矩阵（逐字）："iPhone 11 lineup / SE (2nd Gen) | A13 | iOS 26.0.1 (Dopamine)"、"iPhone 12 lineup | A14 | iOS 17.3.1 (Dopamine)"、"iPhone 13 lineup / SE (3rd Gen) | A15 | iOS 17.3.1 (Dopamine)"。"iPhone 16 / 17 lineups | A18 / A19 families | No public jailbreak"。[^8^]

### 断言 2：iOS 26.1 及更高版本无任何越狱工具（theapplewiki 显示 26.1 "No Tool Available"）

**裁决：属实（含一个值得注明的 beta 例外）。置信度：高。**

- theapplewiki《Jailbreak/26.x》iPadOS 表格（2026-10-17 抓取，逐字）："26.0 Dopamine 3.0.11 … / 26.0.1 Dopamine 3.0.11 … / **26.1 No Tool Available** / 26.2 No Tool Available / 26.2.1 No Tool Available"；iOS 表格中 26.1 同样为 "No Tool Available"。[^7^][^9^]
- **例外（正式版之外）**：Dopamine 3.0.5（2026-08-13）官方更新日志逐字："**Actually** enable support for iOS 26.1 beta 1 - 3 (A12/A13)"[^10^]。即仅 26.1 的 beta 1–3 在 A12/A13 上可越狱；2025-11-03 发布的 26.1 正式版及之后所有版本（26.2–26.6.2，乃至 2026-09-14 发布的 iOS 27）均无公开越狱工具。[^5^][^11^]
- techengage 越狱史综述（2026-09-14，逐字）："There is no public jailbreak for iOS 26.1 or later, or for iPhone 12 and newer models on any iOS 26 build."[^6^]

### 断言 3（关键）：A14（iPhone 12 系列）与 A15（iPad mini 6、iPhone 13 系列、SE3）在 iOS/iPadOS 26.x 上有无可用越狱

**裁决：没有任何可用越狱。iClarified 的表述（Dopamine 对 A14–A17 仅支持 iOS 15.0–17.3.1）属实。置信度：高。**

- Dopamine 官网逐字："Supported on iOS 15.0 - 17.3.1 (A14 - A17 / M1 - M2)"——A14/A15 的上限就是 17.3.1，不含任何 iOS 18/26 版本。[^4^]
- theapplewiki Dopamine 词条逐字："Dopamine … is a semi-untethered jailbreak, for arm64 devices running iOS 15.0 - 18.7.1, A12/A13 devices running 15.0-18.7.1 and 26.0-26.0.1 as well as **A14- A17 Pro, M1 & M2 devices running iOS 15.0 - 17.3.1**."[^1^]
- theapplewiki《Jailbreak/26.x》iPadOS 表格中，iPad mini（6th gen.）在 26.0 和 26.0.1 两行均为 "No"（同为 "No" 的还有 iPad 10th gen/A14、mini A17 Pro、Air 4 及更新、各代 iPad Pro）。[^7^]
- iClarified 矩阵（见断言 1 引文）：iPhone 12（A14）、iPhone 13/SE3（A15）的最新可越狱版本均为 iOS 17.3.1（Dopamine）。[^8^]
- onejailbreak 兼容清单（2026-08，逐字）："**A14 / M1** (iPhone 12, 12 mini, 12 Pro, 12 Pro Max): 16.6 – 17.3.1 + 17.4 betas 1-3"、"**A15 / A16 / A17 / M2** …: 16.5.1 – 17.3.1 + 17.4 betas 1-3"——均不含 iOS 26。[^12^]

### 断言 4：为何 Dopamine 3.0 的 iOS 26 支持只覆盖 A12/A13？（是否依赖 usbliter8？）

**裁决：与 usbliter8 无关。真正原因是"momentarius PPL 绕过（仅限 A12/A13）+ ClearSword/DarkSword 内核漏洞"这一纯软件链；A14+ 设备在 iOS 17.4+/26 上由 SPTM 取代 PPL，而 ≥17.4 没有可用的 SPTM 绕过。置信度：高。**

- Dopamine 3.0 官方更新日志（theapplewiki 逐字转录）："Add Titan PPL/SPTM bypass by @alfiecg24, introduces support for all devices running 16.5.1 - 17.3.1"、"Add momentarius PPL bypass by @staturnzz and @TheRealClarity, introduces support for A12/A13 devices running 16.6 - 18.7.1, 26.0 - 26.0.1"、"obviously only works up to 17.3.1 on real devices due to lack of an SPTM bypass on >=17.4"、"Add ClearSword exploit by @TheRealClarity, cleaner alternative to DarkSword"。[^1^][^13^]
- r/jailbreak 社区技术解释（u/Yeth3，经 ComputerBase 引用，逐字）："26.0.1 is only supported on A12/A13 because they have a new PPL bypass and the darksword kernel exploit. A15+ on 17.0+ replaces PPL with SPTM, and darksword is patched on 26.1"[^14^]
- **关于 usbliter8**：它是 Paradigm Shift 团队 2026-06-18 公开的 **A12/A13 SecureROM（BootROM）系留漏洞**，利用 Synopsys DWC2 USB 控制器硬件缺陷，需 DFU 模式 + RP2350 单片机物理接触触发；"A14 and later appear to be out of reach"。[^15^][^16^] 它**不是** Dopamine 的依赖——Dopamine 3.0 的漏洞清单（theapplewiki 逐字）为 "ClearSword / DarkSword (kernel exploit)、Titan (SPTM/PPL bypass)、momentarius (PPL bypass)" 等纯软件组件[^1^]；截至目前也没有基于 usbliter8 的完整越狱工具，它主要用于研究、SSH ramdisk 和降级前置。[^16^][^17^]
- 补充：A12/A13 之所以在 iOS 26 上仍可被 momentarius 绕过 PPL，而 A14+ 不行，根本原因是新芯片在新系统上启用了 SPTM（Secure Page Table Monitor）且无人公开 ≥17.4 的 SPTM 绕过；Titan 的 SPTM 绕过能力止步于 17.3.1。[^1^][^13^]

### 断言 5：iOS 26.0.1 之后苹果修复了哪些被越狱利用的漏洞（26.1 安全说明）

**裁决：iOS 26.1（2025-11-03）修复了大量内核/沙盒/WebKit 漏洞，社区共识是其中包含越狱链所用内核漏洞的修复（故 Dopamine 止步 26.0.1）；但苹果未逐项标注哪个 CVE 对应 DarkSword/ClearSword，精确归因无法从官方确认。置信度：中高。**

Apple 官方安全公告《About the security content of iOS 26.1 and iPadOS 26.1》（发布 2025-11-03，后多次增补）中与越狱链最相关的条目（逐字）：[^18^]

- 内核："An app may be able to cause unexpected system termination or corrupt kernel memory … CVE-2025-43447: an anonymous researcher; CVE-2025-43462: an anonymous researcher"
- 内核："A malicious application may be able to cause unexpected system termination or write kernel memory … CVE-2025-43520: Apple"（2025-12-12 增补）
- 内核："An app may be able to cause unexpected system termination … CVE-2025-43398: Cristian Dinca (icmd.tech)"
- 内核/内存："A malicious application may cause unexpected changes in memory shared between processes … CVE-2025-43510: Apple"
- 沙盒逃逸 ×2："An app may be able to break out of its sandbox … CVE-2025-43407: JZ" 及 "CVE-2025-43448: Hikerell (Loadshine Lab)"
- WebKit 十余项（UAF CVE-2025-43438、缓冲区溢出 CVE-2025-43429、Google Big Sleep 发现的多枚等）

旁证：安全厂商对一份在野利用链的复盘指出，其内核逃逸阶段"已在 iOS/macOS 26.1 中修复、未分配 CVE"，疑似为 26.1 中三处内核修复之一（明确标注为未证实的推测）。[^19^] 这与社区"darksword is patched on 26.1"的说法方向一致，但**官方未确认具体 CVE 编号**，审核报告中不应断言某一 CVE 即越狱漏洞。

### 断言 6：palera1n 等 checkm8 系工具对 iOS/iPadOS 26 的支持情况（A8–A11）

**裁决：checkm8 系工具在事实上与 iOS/iPadOS 26 无缘——没有任何 checkm8 vulnerable 设备能运行 iOS 26。置信度：高。**

- checkm8 仅影响 A5–A11（iPhone 4S–iPhone X）。受影响设备的系统天花板：iPhone X/8 止步 iOS 16.7.x；最新的 checkm8 iPad（iPad 第 7 代，A10）止步 iPadOS 18.x。techengage 逐字："No checkm8 device can run iOS 26."[^6^]
- palera1n 官方定位（GitHub，逐字）："Jailbreak for A8 through A11, T2 devices, on iOS/iPadOS/tvOS 15.0, bridgeOS 5.0 and higher"[^20^]——"15.0 及以上"仅限其支持的硬件，而支持硬件里没有能跑 iOS 26 的机型。
- palera1n 实际跟进到 26 的只有 **tvOS 与 bridgeOS**：2.2（2025-09-23）"tvOS 26.0 and bridgeOS 10.0 support"；2.3（2026-05-27）"Support up to tvOS 26.5/bridgeOS 10.5"；theapplewiki 另载 palera1n 3.0 beta 2 支持 bridgeOS 10.0–10.6。[^21^][^7^]
- iClarified（逐字）："palera1n: Best for A8 - A11 devices on the latest firmware (semi-tethered, rootless)… Includes support for iPadOS 17/18 and tvOS 26 on specific models."[^8^]
- 网上部分"palera1n 支持 iOS 15–26"的中文 SEO 文章属于把"工具支持的系统范围"与"硬件系统天花板"混淆的误导性表述。[^20^]

---

## 用户核心问题的直接回答

1. **iPhone 12 Pro Max（A14，iOS 26.1）**：在当前任何 iOS 26 版本（含 26.0、26.0.1）上**都无法越狱**。A14 的 Dopamine 上限为 iOS 17.3.1；其设备要想越狱只能停留在 ≤17.3.1（早已停签，除非当前仍在该版本——显然不是）。[^4^][^8^]
2. **iPad mini 6（A15，iPadOS 26.1）**：同样**无法越狱**。theapplewiki 26.x 表中 mini 6 在 26.0/26.0.1 栏为 "No"，26.1 起整行 "No Tool Available"。[^7^]
3. **降回 26.0.1**：**不可行且即便可行也无用**。① 验证通道已关闭——ipsw.me 显示 iPhone 12 全系（iPhone13,2/13,3/13,4）当前唯一在签版本为 iOS 27.0.1（2026-09-28，build 24A446），iOS 26.0.1（23A355，2025-09-29）标记为 Unsigned ✗；苹果在 2025 年 11 月 26.1 发布后不久即停止签署 26.0.1。[^22^][^23^][^24^] ② 更根本地，Dopamine 的 26.0/26.0.1 支持不含 A14/A15，降级成功也改变不了结论。
4. **用户前提修正**："旧的 26.0.1 已经可以越狱"这句话只有加上"**仅限 A12/A13 设备**"才成立；对用户的 A14/A15 两台设备，iOS 26 全系列（26.0–26.6.2 及 iOS 27）目前都没有任何公开越狱路径。

---

## 引用来源

[^1^]: The Apple Wiki — Dopamine（页面更新于 2026-10-12）: https://theapplewiki.com/wiki/Dopamine
[^2^]: MacRumors — "iOS 26 Gets First Jailbreak Thanks to Dopamine"（2026-08-07）: https://www.macrumors.com/2026/08/07/ios-26-dopamine-jailbreak/
[^3^]: GitHub — opa334/Dopamine 官方仓库描述： https://github.com/opa334/dopamine
[^4^]: Dopamine 官网（ellekit.space，2026-10-17 抓取）: https://ellekit.space/dopamine/
[^5^]: Apple — Apple security releases（iOS 26.1 and iPadOS 26.1, 03 Nov 2025；"iPhone 11 and later…"）: https://support.apple.com/en-us/100100
[^6^]: TechEngage — "iOS Jailbreak History: Every Major Tool From 2007 to 2026"（2026-09-14）: https://techengage.com/ios-jailbreaking-complete-history/
[^7^]: The Apple Wiki — Jailbreak/26.x（2026-10-17 抓取；iPadOS 表：26.0/26.0.1 Dopamine 3.0.11，mini 6 为 "No"；26.1 起 "No Tool Available"）: https://theapplewiki.com/wiki/Jailbreak/26.x
[^8^]: iClarified — iPhone Jailbreak Guide 兼容矩阵（2026-08-07 更新）: https://www.iclarified.com/jailbreak
[^9^]: The Apple Wiki — Jailbreak/26.x 检索快照（2026-09-28）："26.1 No Tool Available": https://theapplewiki.com/wiki/Jailbreak/26.x
[^10^]: GitHub — opa334/Dopamine Releases，v3.0.5（2026-08-13）："Actually enable support for iOS 26.1 beta 1 - 3 (A12/A13)": https://github.com/opa334/Dopamine/releases
[^11^]: ipsw.me — iOS Releases（Signed IPSWs：iOS 27.0.1, September 28, 2026, for 34 devices）: https://ipsw.me/iOS/
[^12^]: ONE Jailbreak — "Dopamine Jailbreak IPA Download & Install iOS 15.0-26.0.1"（2026-08-15）: https://onejailbreak.com/blog/dopamine-jailbreak/
[^13^]: ONE Jailbreak — "Dopamine 3.0 Jailbreak With iOS 26 Support"（2026-08-07）: https://onejailbreak.com/blog/dopamine-3-0-jailbreak-out-with-ios-26-support/
[^14^]: ComputerBase — "Dopamine Jailbreak: iOS 26 wurde nach 326 Tagen geknackt"（2026-08-08，引 r/jailbreak u/Yeth3）: https://www.computerbase.de/news/smartphones/dopamine-jailbreak-ios-26-wurde-nach-326-tagen-geknackt.98764/
[^15^]: SecurityAffairs — "usbliter8 Brings Unpatchable BootROM Exploit to Apple A12 and A13 Devices"（2026-06-22）: https://securityaffairs.com/193965/hacking/usbliter8-brings-unpatchable-bootrom-exploit-to-apple-a12-and-a13-devices.html
[^16^]: GitHub — prdgmshift/usbliter8: "An A12/A13 SecureROM exploit"（2026-06-18 公开）: https://github.com/prdgmshift/usbliter8
[^17^]: usbliter8.info — "Can usbliter8 be used for Untethered A12 and A13 jailbreak?"（2026-07-03，明确"完整越狱仍需大量额外工作"）: https://www.usbliter8.info/can-usbliter8-be-used-for-untethered-a12-and-a13-jailbreak/
[^18^]: Apple — About the security content of iOS 26.1 and iPadOS 26.1（发布 2025-11-03；条目增补至 2026-01-28）: https://support.apple.com/en-us/125632
[^19^]: PK Sharma 安全简报（2026-09-16，转述 Socket 对在野链的复盘；内核修复归因标注为未证实推测）: https://www.pk-sharma.com/briefing/packagist-removed-thirteen-fourteenth-still-there
[^20^]: palera1n 官方 GitHub 项目描述（经 gitcode 镜像转载，2026-07）: https://blog.csdn.net/gitblog_00621/article/details/155635983
[^21^]: The Apple Wiki — Palera1n 发布历史（2.2: tvOS 26.0/bridgeOS 10.0；2.3: tvOS 26.5/bridgeOS 10.5）: https://theapplewiki.com/wiki/Palera1n
[^22^]: ipsw.me — Choose an IPSW for the iPhone 12（iPhone13,2）：仅 iOS 27.0.1 ✓ Signed，26.0.1 ✗ Unsigned（2026-10-17 检索）: https://ipsw.me/iPhone13,2/
[^23^]: ipsw.me — Choose an IPSW（iPhone12,3/12,5 页面，显示完整签署状态列表：26.0.1 ✗，仅 27.0.1 ✓）: https://ipsw.me/iPhone12,5
[^24^]: OSTORE — "Apple закрыла возможность отката с iOS 26.1 на iOS 26.0.1"（苹果停止签署 iOS 26.0.1，2025 年 11 月）: https://ostore.ru/blog/obzory-tovarov-i-novosti/apple-zakryla-vozmozhnost-otkata-s-ios-26-1-na-ios-26-0-1/

---

## 未能完全核实 / 保留事项

1. iOS 26.1 中具体哪个 CVE 对应 DarkSword/ClearSword 所用内核漏洞：苹果未归因，社区说法（"darksword is patched on 26.1"）方向一致但无官方确认——报告中已按"中高置信度"处理。
2. theapplewiki《Jailbreak/26.x》iPhone 表格逐字内容因浏览器超时未完整抓取；但 iPadOS 表格逐字已确认，且 iOS 表格中 "26.1 No Tool Available" 有检索快照佐证，另有 Dopamine 官方与 iClarified 双重印证，不影响结论。
3. usbliter8 衍生生态（如 SSHRD 降级、surrealra1n 等）处于快速演进中，未来可能出现基于它的 A12/A13 新工具；但这与用户 A14/A15 设备无关（usbliter8 不影响 A14+）。
