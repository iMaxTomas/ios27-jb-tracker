# 审核报告：iPhone 12 Pro Max（A14）+ iPad mini 6（A15）@ 26.1 —— "是否升级 27.0.1"建议部分的事实核查与逻辑审查

- 审核日期：2026-10-17
- 审核对象：某中文报告"升级建议"部分（4 条原始建议）
- 审核方法：18 次独立网络检索 + 推理链审查。事实部分附来源；推断部分标注【分析推断】。

---

## 一、总体结论（先行）

**报告结论方向部分正确（不建议停在 26.1），但 4 条建议中有 2 条建立在已被证伪的事实上，1 条核心论证（Calif 链）存在三处未证实的跳跃，且整份报告漏掉了对用户最重要的一个事实：用户的两台设备（A14/A15）在 iOS 26 的任何版本都越不了狱——用户"26.0.1 已可越狱"的前提对其设备根本不成立。**

---

## 二、A 部分：事实核实

### A1. 设备芯片（快速确认）——属实

- iPhone 12 Pro Max = A14 Bionic。EveryMac 按处理器索引列出 "iPhone 12 Pro Max (US/A2342) 3.0 GHz* Apple A14 Bionic" 等全部型号。[^1^]
- iPad mini（第 6 代）= A15 Bionic。"Apple has installed the glorious, sensational A15 Bionic Chip in the iPad mini (6th generation), iPhone 13 Pro and 13 Pro Max…"。[^2^]

### A2. 报告事实错误 ①："iPhone 11 被砍支持" —— **证伪**

iOS 27 完整兼容名单包含 iPhone 11 全系及 SE2，共 33 款机型："iPhone SE (2020), iPhone 11, iPhone 11 Pro, iPhone 11 Pro Max, iPhone 12, … iPhone 12 Pro Max …"。[^3^] 苹果官方安全发布页对 iOS 27.0.1 的适用范围同样写明 "iPhone 11 and later…iPad mini 6th generation and later"。[^4^] 被 iOS 27 淘汰的是 iPhone XR/XS（A12 一代），不是 iPhone 11："Does iPhone XR support iOS 27? No."[^5^]

**后果**：报告由"11 被砍"推出的"12 系列已在淘汰边缘"的论证链断裂。12 系列不仅完整支持 iOS 27，且其前一代（11）也仍在支持列表中。【分析推断】按苹果近年每代约 6-7 年大版本支持的惯例，12 系列大概率至少还有一个大版本周期，"提前进维护期"的紧迫感被夸大。

### A3. 报告事实错误 ②："留在 26 收不到修复 / 提前进维护期" —— **证伪（但需精细区分）**

苹果正在并行维护 iOS 26 分支：2026-09-28 与 iOS 27.0.1 **同日**发布了 iOS 26.7.1 / iPadOS 26.7.1，修复 CVE-2026-86950（CoreGraphics 越界写，可致任意代码执行），苹果明确表示该漏洞"may have been exploited in an extremely sophisticated attack against specific targeted individuals on versions of iOS before iOS 27"，CISA 已于 9 月 29 日将其列入 KEV 目录。[^6^][^7^][^8^]

- "留在 iOS 26 分支" ≠ 收不到安全更新：26.7.1 就是为拒绝升 27 的用户准备的，"iOS 26.7 keeps existing iOS 26 users secure since they refuse to upgrade to iOS 27"。[^9^]
- **但"停在 26.1 这个具体版本"确实收不到修复**：26.1 发布于 2025-11-03（build 23B85），此后 26.2–26.7.1 的修复（含上述在野 0day）均与 26.1 用户无关。[^10^]

报告把"分支"与"版本"混为一谈：若用户愿意在 26 分支内跟进到 26.7.1，安全维护并未终止。报告"留在 26 同样收不到修复"的表述是错的；正确表述应是"停在 26.1 收不到修复"。

### A4. 越狱生态现状（决定用户前提是否成立）——用户前提对其设备不成立

- Dopamine 3.0（2026-08-07 发布，opa334）是 iOS 26 首个越狱，但 **iOS 26.0/26.0.1 仅限 A12/A13 芯片**（实际能装 iOS 26 的即 iPhone 11 系列、SE2、iPad 9 等 A13 设备）。[^11^][^12^]
- A14–A17/M1/M2 设备的 Dopamine 上限仍是 **iOS 17.3.1**（17.4 起 SPTM 无公开绕过）。"Dopamine has no iOS 18 support for A14 and newer devices…A14 to A17 support only through iOS 17.3.1"；"There is no public jailbreak for iOS 26.1 or later, or for iPhone 12 and newer models on any iOS 26 build"。[^13^][^14^]
- Dopamine 3.0.5 虽增加了 iOS 26.1 beta 1–3 支持，但同样**仅限 A12/A13**。[^15^]

**结论：用户两台 A14/A15 设备在 iOS/iPadOS 26 的任何版本（含 26.0.1、26.1、26.7.1）均无公开越狱工具。用户"旧的 26.0.1 已经可以越狱"的前提来自 A13 设备的新闻，对 A14/A15 不成立。"留在 26 以便越狱"对这两台设备是零收益策略。**

### A5. "Calif 这条链覆盖到 27.0.1" —— 部分属实，三处关键限定被报告省略

已核实事实：

- 2026-10-16，安全公司 Calif（@calif_io）在巴黎 Hexacon 2026 现场演示了 iPhone 18 Pro（iPhone19,2，T8160 芯片）上 iOS 27.0.1（build 24A446）的 **untethered** 越狱：重启后越狱自动恢复，SSH 以 uid=0 登录。[^16^][^17^]
- **漏洞未披露、无公开工具**；"will be released in due time" 出自其 newsletter/二手转述，非明确发布承诺。[^16^][^18^]
- 演示平台是**最新芯片 T8160**；没有任何证据表明该链覆盖 A14/A15。
- iOS 27（9 月 14 日发布）本身修复了 120+ 漏洞（20 个内核级），其安全说明致谢 Calif.io 的两处 DoS bug（与越狱链无关）。[^18^][^19^]

报告未交代的限定：①无公开工具、无发布时间表；②未证实支持 A14/A15（演示用的是苹果最新硬件，PAC/SPTM 等缓解措施代际不同，链条的可移植性未知）；③一旦 Calif 向苹果披露细节，苹果会迅速修补——"How long iOS 27.0.1 stays exposed therefore depends largely on when Calif chooses to publish"。[^19^] 【分析推断】历史上会议演示型越狱（Pwn2Own/Hexacon 类）转化为公开工具的比例很低；"升级不丢失理论覆盖"中的"理论"二字承重过大。

### A6. 升级不可逆性：签名机制与 SHSH —— 报告隐含判断正确，且应讲得更硬

- 苹果已在 10 月初停止签署 iOS 27.0 与 26.7："Apple has stopped signing iOS 27 and iOS 26.7…you can no longer downgrade to these versions"。[^9^]
- "Anyone who has moved to iOS 27 can no longer downgrade to iOS 26"。[^7^]
- 对 A12 及更新（非 checkm8）设备，SHSH blob 实质上无效："On A12 and later, restoring to iOS versions other than those currently signed by Apple is not supported. Additionally, SHSH blobs are useless for downgrades."（Cryptex/SEP 兼容性使 FutureRestore 路线成为死路。）[^20^]
- 即：**对用户的 A14/A15 设备，升到 27.0.1 是单向门**。这是决策中权重很高的一条，报告未明说。
- 缓冲机制：Delayed OTA 可在苹果关签后约 90 天内通过描述文件升到已关签的版本（dhinakg.github.io/delayed-otas.html 可查有效期）。[^21^]【分析推断】这为"先观望、后定点升 27.0.1"提供了约 3 个月的技术缓冲，但依赖该机制持续可用且版本在 90 天窗口内，不宜作为唯一依赖。

### A7. 版本时间线与"锁版本窗口"的紧迫性

- iOS 27：2026-09-14 发布；iOS 27.0.1：2026-09-28（build 24A446，无公开 CVE，主要修 iPhone 18 Pro 的 Face ID 重启、相机伪色与触屏无响应）。[^4^][^22^]
- iOS 27.1 为 iPhone Duo 独占（10 月 23 日随机器上市），普通 iPhone 大概率跳过 27.1 直接到 27.2（预计 10 月下旬）。[^23^][^24^]
- 【分析推断】因此 27.0.1 作为"普通 iPhone 当前最新版"的状态大约还剩 1–2 周；27.2 发布后苹果通常在一两周内关闭 27.0.1 签名。**想"升 27 锁 27.0.1"，窗口就是现在到 27.2 发布前后**；报告未指出这一时间敏感性。
- 当前 26.x 用户 OTA 界面提供 27.0.1 与 26.7.1 两个选项，可自由选择。[^25^]

### A8. usbliter8 硬件路线与 A14/A15 —— 报告未提及，但对"等待位"判断重要

- usbliter8（Paradigm Shift，2026-06-18 公开）是 A12/A13/S4/S5 的 SecureROM 不可修复漏洞，需物理接触 + DFU + RP2350 硬件；**官方明确 A14 及更新芯片不受影响**（DART 配置变更）。[^26^][^27^]
- 中文社区有 9 月初爆料声称早期 A14/M1"同样受影响、细节未公开"[^28^]——与官方 PoC 范围矛盾，属未证实传闻，权重低。
- 【分析推断】即用户两台设备不存在"硬件级保底越狱"的预期；A14/A15 的下一次越狱只能等软件链，进一步降低"为越狱停在旧版"的期望收益。

### A9. "27 主打性能/发热/稳定性修复" —— 部分有据，但与老设备的关联未证实

iOS 27.0.1 的三个具名修复中两个仅涉 iPhone 18 Pro 系列；27.0.1 无公开 CVE。[^22^] iOS 27 大版本含 Liquid Glass 优化、性能与连接性改进等泛化描述。[^25^]【分析推断】对 A14/A15 老设备，大版本升级的体验收益通常中性偏正但无直接测量证据；"发热/性能修复"作为主要升级理由对该用户证据不足，更站得住的理由是安全补丁与新功能周期。

---

## 三、B 部分：逻辑审查（漏洞清单）

**漏洞 1（最致命）：未纠正用户的错误前提。** 用户说"旧的 26.0.1 已经可以越狱"，报告通篇顺着此前提讨论"留在 26 等越狱"。事实是 26.0/26.0.1 越狱仅限 A12/A13（见 A4），用户两台设备在 26.x 任何版本都越不了狱。这直接改变决策结构："留在 26"对越狱的收益是**确定的 0**，而不是报告暗示的"保留一个已有能力"。

**漏洞 2：以错误事实支撑的淘汰论。** "iPhone 11 被砍 → 12 在淘汰边缘"已被证伪（见 A2）。12 系列完整支持 iOS 27。

**漏洞 3：分支/版本混淆。** "留在 26 等于提前进维护期、收不到修复"错误（见 A3）：26.7.1 与 27.0.1 同日发布，修复了在野利用的 0day。正确的问题是"停在 26.1 vs 跟进 26.7.1 vs 升 27.0.1"的三选，而不是报告的二分。

**漏洞 4：Calif 论证的三重跳跃。** 从"存在 27.0.1 演示"到"升级不丢失理论覆盖"需要三个未证实前提：工具会公开发布、发布会支持 A14/A15、发布时 27.0.1 仍未被修补且仍可到达（签名/DelayOTA 窗口）。三者叠加概率不高（见 A5）。【分析推断】

**漏洞 5："等待位"分析缺失。** 报告说"为越狱留后路就升 27 锁版本"，但没有比较两个等待位的期望值。【分析推断】历史规律（Dopamine 的封顶史：15.4.1 → 16.5.1/16.6.1 → 17.3.1，每次都停在苹果修复前最后一个可利用版本）表明：等待位的价值完全取决于"未来链条所用 bug 存活的版本区间"，事前不可知。但可做期望比较：
- 停在 26.1：已证实的可利用性 = 0；只有当未来出现基于 26.x 时代 bug 的 A14/A15 新链时才有价值。
- 锁 27.0.1：存在一条已实证（但未公开、芯片覆盖未知）的链；若 Calif 或其衍生工具发布且覆盖 A14/A15，27.0.1 就是唯一已知可利用版本。
- 两者期望都低，但 27.0.1 ≥ 26.1，且 27.0.1 额外带来 11 个月的安全补丁。报告的方向对，但理由给错了（不是"Calif 覆盖 27.0.1 所以升级无损"，而是"26.1 本来就一无所有，升级只赚不赔——除了不可逆性"）。

**漏洞 6：风险评估的时态错误。** 报告说锁版本要"承担未打补丁的安全风险"（将来时），却忽略用户**此刻**停在 26.1 已承担约 11 个月未补丁风险，包括一个已被在野利用、CISA 列入 KEV 的 0day（CVE-2026-86950，影响"所有 iOS 27 之前版本"）。[^6^][^8^] 升级到 27.0.1（或至少 26.7.1）是**降低**风险，不是增加。

**漏洞 7：时间敏感性缺失。** "升 27 锁 27.0.1"的执行窗口受 27.2 发布（预计 10 月下旬）与关签节奏约束（见 A7），报告无任何时间提示。另外报告说苹果"预计下一个 27.0.x 或 27.1"修补——普通 iPhone 没有 27.1（Duo 独占），下一个版本大概率是 27.2。[^24^]

**漏洞 8：iPad 分析未区分但结论侥幸一致。** iPadOS 27 支持 iPad mini 6 起（官方列表）[^4^]；iPadOS 26.7.1 支持 mini 5 起[^7^]；Dopamine 对 A15 的上限同为 17.3.1。故对 mini 6 的结论与 iPhone 一致：iPadOS 26 任何版本均不可越狱。报告未单独论证，结论碰巧不错。

---

## 四、C 部分：修正版决策树（针对该用户的精确建议）

```
Q0. 你的设备现在能越狱吗？
    → 不能。A14/A15 在 iOS/iPadOS 26 的任何版本（含 26.0.1/26.1）均无公开工具，
      上限是 17.3.1。"留在 26 保住越狱"对这两台设备是伪选项。

Q1. 你最看重什么？
├─ A. 安全稳定优先（推荐大多数用户）
│    → 直接升 27.0.1（或至少升 26.7.1）。
│      理由：26.1 已落后约 11 个月补丁，含在野 0day（CVE-2026-86950，KEV）。
│      停在 26.1 是所有选项中最差的。
│
├─ B. 想给未来越狱留最大概率，同时不牺牲安全
│    → 在 iOS 27.2 发布前（预计 10 月下旬，窗口约 1–2 周）：
│      1) 两台设备先备份；
│      2) OTA 选择升 27.0.1（不要选"最新版"以外的自动通道）；
│      3) 关闭自动更新，锁定 27.0.1；
│      4) 关注 opa334/Dopamine、AppleDB、Calif 的披露动态与苹果安全发布说明
│         （若某个 27.x 安全说明致谢 Calif，即说明链条已修补，锁版本意义重估）。
│    → 退路认知：升级不可逆（A12+ 的 SHSH 无效、已关签版本无法降级）；
│      DelayOTA 提供关签后约 90 天的定点升级缓冲，可作备选但勿全押。
│
└─ C. 强烈想观望 27 的稳定性/续航口碑
     → 先升 26.7.1（仍在 26 分支，补上 0day），观望 2–4 周；
       但须自知：27.0.1 的签名窗口可能在 27.2 发布后 1–2 周关闭，
       错过则"锁 27.0.1"选项消失，只剩 DelayOTA 的 90 天补救通道。

Q2. 要不要"为等越狱"做任何牺牲安全的决定？
    → 不要。A14/A15 的公开越狱已停滞近三年（封顶 17.3.1）；
      无硬件级保底（usbliter8 官方确认 A14+ 不受影响）；
      Calif 链无公开工具、无 A14/A15 覆盖证据、无发布时间表。
      把"锁在 27.0.1"当作免费的期权持有即可，不要为它放弃安全更新节奏之外的东西。

iPad mini 6：与 iPhone 完全同一决策树（iPadOS 27.0.1 / 26.7.1 同步发布，Dopamine 对 A15 上限同为 17.3.1）。
```

### 与原报告的逐条修正对照

| 原建议 | 裁定 | 修正 |
|---|---|---|
| ① "体验值得升；11 被砍说明 12 在淘汰边缘" | **部分错误** | 升级方向可取，但论据错误：iPhone 11 未被砍，12 系列不在淘汰边缘；真正的理由是安全补丁与功能周期 |
| ② "Calif 覆盖 27.0.1，升级不丢理论覆盖；留在 26 同样收不到修复" | **事实错误 + 推断过度** | Calif 仅有演示、无工具、无 A14/A15 证据；26 分支仍在维护（26.7.1）；正确理由是"26.1 已无任何可失去的越狱能力" |
| ③ "为越狱留后路就升 27 锁版本" | **方向正确、论证不全** | 可采纳，但须补充：执行窗口在 27.2 发布前、升级不可逆、锁版本只是持有一份低概率期权 |
| ④ "保持 27.0.1 或更低、关自动更新、承担安全风险" | **表述误导** | 锁版本的风险远低于现状：停在 26.1 当前就暴露在 KEV 级 0day 下；升级本身是降风险 |

---

## 五、来源

[^1^]: EveryMac, "iPhone and iPad with an Apple A14 Bionic Processor"（2026-07-13 检索）。https://everymac.com/devices/by-processor/all-apple-a14-bionic-processor-apple-devices.html —— "iPhone 12 Pro Max (US/A2342) 3.0 GHz* Apple A14 Bionic*"
[^2^]: Phonebot, "All About Apple A15 Bionic Chip"（2026-08-24）。https://www.phonebot.com.au/blog/a15-bionic-chip —— "Apple has installed the…A15 Bionic Chip in the iPad mini (6th generation)"
[^3^]: TechRadar, "Which iPhones support iOS 27 and Siri AI?"（2026-09-14）。https://www.techradar.com/phones/ios/ios-27-and-siri-ai-compatibility-explained —— 完整名单含 iPhone 11 全系、12 全系共 33 款
[^4^]: Apple Support, "Apple security releases"（2026-09-28）。https://support.apple.com/en-us/100100 —— "iOS 27.0.1 and iPadOS 27.0.1…iPhone 11 and later…and iPad mini 6th generation and later | 28 Sep 2026"；"iOS 26.7.1 and iPadOS 26.7.1…28 Sep 2026"
[^5^]: Rajatheertha, "iOS 27.0.1 Is the Latest iPhone Update"（2026-10-07）。https://www.rajatheertha.com/news/ios-27-0-1-latest-update-compatible-iphones-battery-drain-problems-fixes —— "Does iPhone XR support iOS 27? No…iPhone XS? No."
[^6^]: CRC Cloud Security Briefing（2026-10-12）。https://www.crccloud.com/resources/security-briefing/ —— "iOS 26.7.1…fixes CVE-2026-86950…exploited in an extremely sophisticated attack…on versions of iOS before iOS 27…CISA added CVE-2026-86950 to its Known Exploited Vulnerabilities catalog"
[^7^]: iThinkDiff, "iOS 26.7.1 Fixes a CoreGraphics Flaw Possibly Used in Attacks"（2026-09-28）。https://www.ithinkdiff.com/ios-26-7-1-coregraphics-exploited-vulnerability-fix/ —— "Anyone who has moved to iOS 27 can no longer downgrade to iOS 26"
[^8^]: 9to5Mac, "iOS 26.7.1 available now for iPhone with security fixes"（2026-09-28）。https://9to5mac.com/2026/09/28/ios-26-7-1-available-now-for-iphone-with-security-fixes/ —— CVE-2026-86950 安全说明全文
[^9^]: APLWire, "You can no longer downgrade to iOS 27 or iOS 26.7"（2026-10-06）。https://www.aplwire.com/2026/10/06/you-can-no-longer-downgrade-to-ios-27-or-ios-26-7/ —— "Apple has stopped signing iOS 27 and iOS 26.7"；"iOS 26.7 keeps existing iOS 26 users secure since they refuse to upgrade to iOS 27"
[^10^]: MacRumors Forums, "iOS 26.1 — Bug Fixes, Changes, and Improvements"（2025-11-03）。https://forums.macrumors.com/threads/ios-26-1-bug-fixes-changes-and-improvements.2470382/ —— "Release Date: November 3 2025, Build 23B85"
[^11^]: MacRumors, "iOS 26 Gets First Jailbreak Thanks to Dopamine"（2026-08-07）。https://www.macrumors.com/45/ —— "Dopamine 3.0…including iOS 26.0 and iOS 26.0.1 on devices with A12 or A13 chips"
[^12^]: TechSpot, "A jailbreak for iOS 26 is finally here, 326 days after launch"（2026-08-09）。https://www.techspot.com/news/113408-jailbreak-ios-26-finally-here-326-days-after.html
[^13^]: TechEngage, "iOS Jailbreak History: Every Major Tool From 2007 to 2026"（2026-09-14）。https://techengage.com/ios-jailbreaking-complete-history/ —— "There is no public jailbreak for iOS 26.1 or later, or for iPhone 12 and newer models on any iOS 26 build, and Apple's signing system generally stops you from downgrading"
[^14^]: MacMagazine（巴西）, "Primeiro jailbreak para o iOS 26 é lançado"（2026-08-10）。https://macmagazine.com.br/post/2026/08/10/primeiro-jailbreak-para-o-ios-26-e-lancado-mas-so-em-certos-iphones/ —— 兼容表："A14, M1: iOS 16.6–17.3.1；A15, A16, A17, M2: iOS 16.5.1–17.3.1"
[^15^]: TheAppleWiki, "Dopamine"（2026-10-12 版本）。https://theapplewiki.com/wiki/Dopamine —— 3.0.5："Actually enable support for iOS 26.1 beta 1 - 3 (A12/A13)"
[^16^]: Cyber Security News, "iOS 27.0.1 Untethered Jailbreak Demonstrated on iPhone 18 Pro"（2026-10-16）。https://cybersecuritynews.com/ios-27-0-1-jailbreak-demonstrated/ —— "The vulnerabilities used have not been disclosed and no tool has been published"；iPhone19,2 / T8160 / build 24A446
[^17^]: Pasquale Pillitteri, "Calif Unveils iPhone 18 Pro Jailbreak on iOS 27.0.1 at Hexacon"（2026-10-16）。https://pasqualepillitteri.it/en/news/24540/jailbreak-iphone-18-pro-ios-27-calif-hexacon
[^18^]: Decrypt, "Hackers Jailbreak iPhone 18 Pro on Stage, and It Survives a Reboot"（2026-10-17）。https://decrypt.co/381177/hackers-jailbreak-iphone-18-pro-survives-reboot —— "no public tool exists…'will be released in due time'"；iOS 27 安全说明致谢 Calif.io 两处 DoS bug
[^19^]: GBHackers, "Researchers Demonstrate iOS 27.0.1 Untethered Jailbreak on iPhone 18 Pro"（2026-10-17）。https://gbhackers.com/ios-27-0-1-untethered-jailbreak/ —— "How long iOS 27.0.1 stays exposed therefore depends largely on when Calif chooses to publish"；iOS 27 修复 120+ 漏洞
[^20^]: ONE Jailbreak, "SEP and Baseband Compatibility List for FutureRestore downgrade"（2025-09-05）。https://onejailbreak.com/blog/futurerestore-sep-baseband-compatibility-list/ —— "On A12 and later, restoring to iOS versions other than those currently signed by Apple is not supported. Additionally, SHSH blobs are useless for downgrades."
[^21^]: BetaProfiles Forum, "How to Update…Using Delayed OTAs"（2025-05-21）。https://forum.betaprofiles.com/t/how-to-update-from-ios-beta-to-an-unsigned-official-release-using-delayed-otas/19574 —— "We can utilise this 90 day delay to move to certain OS versions even after Apple have stopped signing them"
[^22^]: Yahoo Tech, "Apple Releases iOS 27.0.1 for iPhone: Here's What's Fixed"（2026-09-29）。https://tech.yahoo.com/ai/apple-intelligence/articles/apple-releases-ios-27-0-185637462.html —— 三项修复、build 24A446、无 CVE
[^23^]: MacRumors, "iPhone Duo Will Have Day-One iOS 27.1 Update"（2026-10-13）。https://www.macrumors.com/2026/10/13/iphone-duo-day-one-update/ —— Duo 10 月 23 日上市，搭载 27.1
[^24^]: Michael Tsai Blog 汇编（2026-10-14）。https://mjtsai.com/blog/ —— Gruber："iOS 27.1: October 23, exclusively on the iPhone Duo…All OS 27.2.0 releases: probably in late October"
[^25^]: APLWire, "iOS 27.0.1 is the direct new upgrade from iOS 26"（2026-10-09）。https://www.aplwire.com/2026/10/09/ios-27-0-1-is-the-direct-new-upgrade-from-ios-26-not-ios-27/ —— 26 用户可选 27.0.1 或 26.7.1；"iOS 27 brings new features and optimizations…improves the Liquid Glass design, optimizes performance, and connectivity"
[^26^]: The Hacker News, "Unpatchable 'usbliter8' Exploit Breaks Apple A12 and A13 SecureROM Boot Chain"（2026-06-20）。https://thehackernews.com/2026/06/unpatchable-usbliter8-exploit-breaks.html —— "A14 and later appear to be out of reach for this exploit path"
[^27^]: Hoplon Infosec, "usbliter8 Exploit: Apple A12/A13 SecureROM Flaw Explained"（2026-06-20）。https://hoploninfosec.com/usbliter8-exploit-apple-a12-a13-securerom-vulnerability —— "A14 and newer chips are also not affected, because Apple changed how…DART is configured"
[^28^]: 什么值得买, "iOS 27 正式版推了…按芯片看"（2026-09-17）。https://post.smzdm.com/p/a70nrpn9/ —— "9 月 1 日玄v烨爆料早期 A14 与 M1 设备同样受影响，细节未公开"（未证实传闻，与官方 PoC 范围矛盾）
