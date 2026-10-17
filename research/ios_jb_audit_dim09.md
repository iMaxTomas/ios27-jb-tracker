# iOS 27 越狱相关报告事实核查（维度 09）

- 核查日期：2026-10-17
- 核查方法：网络深度检索，共执行 23 次独立搜索查询（覆盖英文/中文/德文转述来源），交叉比对安全厂商博客、主流科技媒体、苹果官方安全发布页及漏洞数据库。
- 事件背景：2026-10-16，安全公司 Calif（Calif.io）在巴黎 Hexacon 2026 会议上演示了 iPhone 18 Pro / iOS 27.0.1 的 untethered（重启持久）越狱。[^2^][^4^]

---

## 断言 1：声称能越狱 iOS 27 的网站/App 基本是骗局

**裁决：属实（高置信度，置信度 90%）**，但附两点限定（见下）。

### 证据

（a）安全厂商 Corellium（隶属 Cellebrite）对假越狱 "nekoJB Online"（自称支持 iOS 18.7–26）做了完整实验剖析，结论：

> "nekoJB Online isn't a jailbreak. It's a scam. A trap designed to harvest data and exploit trust."[^5^]
> —— Cellebrite/Corellium 博客，2026-06-08，https://cellebrite.com/en/blog/nekojb-jailbreak-scam/

其手法正是**恶意配置描述文件**：安装 .mobileconfig 描述文件、web clip 和根证书，实现流量拦截与潜在凭据窃取：

> "Fake jailbreak scams can use .mobileconfig profiles, web clips, and root certificates to mislead users while enabling traffic interception and possible credential harvesting, according to Corellium."[^6^]
> —— NHI Mgmt Group（转述 Corellium 原文），2026-08-18，https://nhimg.org/articles/fake-ios-jailbreak-scams-are-turning-profiles-into-credential-traps/

（b）Cellebrite 在 iOS 27 相关博客中直接警告：

> "Fake jailbreak tools like nekoJB are circulating and often carry malware."
> "Tools circulating online that claim to jailbreak iOS 27 are often malware in disguise."[^1^]
> —— Cellebrite 博客，2026-09-28，https://cellebrite.com/en/blog/ios-risk-for-mobile-app-developers-and-security-teams/

（c）广告/点击欺诈手法有媒体记录的先例（checkm8 时代的 checkrain[.]com 假冒站，Cisco Talos 披露）：

> "the malicious checkrain site encourages visitors to download an application that clicks on risky advertisements and installs iOS video games... 'The chain used in this processes through several ad-tracking, verification, geolocation and, finally, campaign delivery'"[^8^]
> —— CyberScoop 引述 Cisco Talos，2019-10-15，https://cyberscoop.com/ios-jailbreak-checkm8-app-fraud/

> "the site is trying to trick users into installing unrelated third-party apps for click fraud, according to researchers at Cisco's Talos security group."[^7^]
> —— PCMag，2019-10-15，https://www.pcmag.com/news/scammer-uses-fake-iphone-jailbreak-for-fraud-scheme

（d）截至核查日，不存在任何公开的 iOS 27 越狱工具（侧面印证"网上声称可下载者均为假冒"）：

> "There's no public kernel iOS 27 jailbreak as of late August 2026... While the claims of a working tool or browser-based iOS 27 jailbreak are not true kernel-level jailbreaks."[^36^]
> —— 4uKey（Tenorshare），2026-08-28；且 Calif 演示后"Status pages still show no public jailbreak tool for any iOS 27 firmware"[^3^]（ONE Jailbreak，2026-10-16）

### 限定条件

1. "付费调查（survey scam）"这一具体手法在 iOS 27 语境下未见本次检索中的安全厂商专项记录；已记录的手法为**恶意配置描述文件/根证书（2025–2026，Corellium 实证）**与**广告点击欺诈（2019，Talos 实证）**。"付费调查"属假越狱生态中广为人知的通用手法，但本报告无法为其在 iOS 27 骗局中的存在给出逐字来源，标记为**部分未证实**。
2. "基本是骗局"是概率性概括。Calif 演示证明 iOS 27.0.1 越狱技术上存在[^2^][^4^]，但无任何可下载工具，因此面向公众的"网站/App 下载即越狱"确属骗局；概括方向正确。

---

## 断言 2：Cellebrite 称"iOS 27 无法在任何现有物理设备上越狱"、"iOS 18 以上没有可靠公开越狱"；Calif 演示挑战该说法

**裁决：属实（高置信度，置信度 95%）**，含一处需注意的利益冲突与一处措辞细节。

### 证据

（a）Cellebrite 官方博客原文（TL;DR 逐字）：

> "iOS 27 cannot be jailbroken on any current physical device and there is no reliable public jailbreak for anything above iOS 18."[^1^]
> —— Cellebrite，"Why Jailbreak Is Over with iOS 26"，2026-09-28，https://cellebrite.com/en/blog/ios-risk-for-mobile-app-developers-and-security-teams/

该博客 FAQ 部分再次确认：

> "No. There is no public jailbreak for iOS 27 on any current physical device — conservatively, nothing above iOS 18 has a reliable public jailbreak."[^1^]

（b）GBHackers 对演示挑战该说法的表述（逐字）：

> "Cellebrite had written just weeks earlier that iOS 27 could not be jailbroken on any current physical device. It also said there was no reliable public jailbreak for anything above iOS 18. This demo challenges that claim."[^2^]
> —— GBHackers，2026-10-16，https://gbhackers.com/ios-27-0-1-untethered-jailbreak/

（c）演示事实（多源互证）：2026-10-16 巴黎 Hexacon（Synacktiv 主办），Calif 团队（开场主题演讲人 Bruce Dang 与 Thai Duong）在 iPhone 18 Pro（iOS 27.0.1，build 24A446，当时最新公开版本）上现场演示重启后自动恢复 root、SSH 登录 uid=0：

> "On stage, the researchers restarted the iPhone 18 Pro... The jailbreak then came back on its own without the phone being connected to a computer... The id command returned uid=0, which confirms full root access."[^4^]
> —— Cyber Security News，2026-10-16，https://cybersecuritynews.com/ios-27-0-1-jailbreak-demonstrated/

> "Heise reports it is the first reboot-proof jailbreak since iOS 9, and that AI helped find the exploit chain."[^2^]

### 限定条件

1. **利益冲突**：Cellebrite 该博客本质上是 Corellium 虚拟化 iOS 环境的营销文（"Corellium is the only way to fully analyze and verify iOS internals"）[^1^]，其"越狱已死"的论断有商业动机，并非中立研究结论。
2. **措辞细节**：演示挑战的是"iOS 27 无法在任何现有物理设备上越狱"这一半句（已被事实推翻）；而"iOS 18 以上没有可靠**公开**越狱"这半句在字面意义上仍然成立——Calif 未发布任何工具[^2^][^3^]。报告若将两者混为一谈则不严谨。
3. 时间表述"iOS 27 发布前数周"：Cellebrite 博客日期 2026-09-28，实为 iOS 27 发布（9 月 14 日）**之后**两周、演示（10 月 16 日）之前约 2.5 周。报告原文若写"发布前数周"则时间线有误；正确表述应为"演示前数周"（GBHackers 原文即为 "just weeks earlier"，相对演示而言）[^2^]。

---

## 断言 3：间谍软件厂商在买这类完整越狱链；Calif 没有社区放出的强动机

**裁决：前提属实，结论为合理推断（前提置信度 95%，推断置信度 75%）**，但存在一条反向证据需披露。

### 前提核查：商业漏洞市场

（a）零日经纪商公开报价（iOS 完整链价格量级，多源一致）：

> "Up to $2,000,000 iOS zero click full compromise chain with persistence."[^13^]
> —— Zerodium 价目表，CERT-EU 威胁备忘录引述，https://cert.europa.eu/publications/threat-intelligence/threat-memo-190910-1/pdf

> "Crowdfense now offers up to $9 million for zero-click full-chain smartphone exploits... Crowdfense offers $5 million to $7 million for iOS zero-days"[^14^][^15^]
> —— mapshock.com（2026-08-22）与 BrightDefense（2026-09-02）引述 SecurityWeek/Crowdfense

> "the firm is willing to pay up to $9 million for zero-click exploits that work via SMS or MMS and up to $7 million for iPhone zero-days."[^16^]
> —— cybersecurity-help.cz

（b）间谍软件行业对 iOS 链的需求（实证）：

> "CSVs [commercial surveillance vendors], in particular, were found to be behind 75% of known zero-day exploits... targeting Google products and the Android ecosystem, as well as 55% (amounting to 11 out of 20 vulnerabilities) targeting iOS and Safari."[^16^]
> —— Google TAG 报告转述

> "Russia's APT29 has been observed using exploits with identical or striking similarities to those used by NSO Group and Intellexa, suggesting potential acquisition of tools between state-backed actors and controversial surveillance software vendors."[^17^]
> —— SecurityWeek 引述 Google TAG，2024-08-29，https://www.securityweek.com/google-catches-russian-apt-re-using-exploits-from-spyware-merchants-nso-group-intellexa/

前提成立：一条可用的最新版 iOS 完整越狱链在黑灰市场价值数百万美元，商业间谍软件厂商是主要买家。

### Calif 的披露行为历史（核查"无放出动机"）

Calif 是帕洛阿尔托的商业攻击性安全公司（提供红队与 AI 辅助漏洞研究服务）[^39^]。其既往行为模式：

- 2026 年 5 月 M5/MIE（macOS 内存完整性强制）攻破事件中，Calif **未发布工具**，而是"专程从帕洛阿尔托驱车前往苹果库比蒂诺总部，当面汇报长达 55 页的技术报告……计划在苹果完成修补后再对外公布技术细节"[^12^]（163.com 转述，2026-05-15）；CEO Thai Duong 对《华尔街日报》表态细节亦见于 digit.in[^11^]。
- 2026 年 9 月 CVE-2026-86950 补丁发布后，Calif 逆向补丁并发布的是 **crash-only PoC**（仅触发崩溃、不含武器化代码）："The harness crashes iOS and macOS through the ImageIO thumbnail path and does not demonstrate code execution."[^18^][^19^][^34^]

即：Calif 的历史模式是"私下报告厂商 + 补丁后发布分析/非武器化 PoC"，而非向社区放出完整工具。结合数百万美元的市场价格，"缺乏免费放出的经济动机"推断成立。

### 反向证据（必须披露）

ONE Jailbreak 报道称："Ellis later said the presenters mentioned that the work would be released 'in due time'."[^3^]（Billy Ellis 转述，2026-10-16）；explainx 亦记录 "Calif says demos 'will be released in due time' (as cited by Decrypt)"[^10^]。GBHackers 明确指出这是二手说法："That is a secondary claim, not a Calif statement."[^2^]

**结论**：报告"Calif 没有社区放出的强动机"是合理推断（商业模式 + 既往披露史支持），但不能表述为既成事实——已有二手报道称 Calif 暗示"适时发布"，且其"发布"更可能指补丁后的技术细节/受限 PoC，而非可直接使用的越狱工具。

---

## 断言 4：苹果预期在"下一个 27.0.x 或 27.1"修补

**裁决：部分属实（推断方向正确，版本通道判断有误）（置信度 80%）**。

### 前提核查：苹果对越狱级漏洞的历史响应速度

- **iOS 12.4.1 案例（2019）**：SockPuppet 漏洞在 iOS 12.4 中被意外重新引入，unc0ver v3.5.0 于 2019-08-18 加入 iOS 12.4 支持[^21^]；苹果于 **2019-08-26（约 8 天后）** 发布 iOS 12.4.1 封堵[^20^]（SecureMac，2019-08-26，https://www.securemac.com/news/the-great-ios-jailbreak-of-2019-and-why-you-should-update-to-12-4-1-right-now）。
- **iOS 16.5.1 案例（2023，三角测量行动）**：卡巴斯基 2023-06-01 披露后，苹果于 2023-06-21 发布 iOS 16.5.1，修复内核漏洞 CVE-2023-32434（"An app may be able to execute arbitrary code with kernel privileges... actively exploited against versions of iOS released before iOS 15.7"）及 WebKit 漏洞[^22^]（NVD/SentinelOne 条目）。
- **近期案例（2026）**：TSS/服务器端签名漏洞被用于 iOS 27 半越狱环境后，"Apple closed the server path"——在数日内从服务器端封禁，无需系统更新[^3^]。

历史模式支持"苹果会对公开的越狱级漏洞链快速响应（数日至数周）"的推断。

### 版本通道核查（发现错误）

报告所称"27.1 修补"对普通 iPhone **不成立**：iOS 27.1 是 iPhone Duo 折叠屏专属版本，普通 iPhone 被跳过：

> "since the iOS 27.1 build is made with iPhone Duo elements in mind, that version is being skipped for the non-foldable iPhone models."[^25^]
> —— macsonly.com 引述 AppleInsider，2026-09-21

> "When iPhone Duo becomes available on October 23, it will run iOS 27.1."[^24^][^27^]
> —— Apple 开发者公告（2026-09-16）转述；Xcode 27.1 RC 已于 10 月 5 日发布[^24^]

普通 iPhone 的下一个更新通道是 27.0.x（如 27.0.2）或 **iOS 27.2**（27.2 beta 1 于 9 月 16 日发布，截至 10 月 13 日已到 beta 4，build 24B5110a）[^23^][^26^]。截至核查日（10 月 17 日），苹果未对 Calif 演示发表任何公开回应，演示所用漏洞未披露、无 CVE[^2^][^10^]——在漏洞细节未知的情况下，苹果**能否**在下一版本中精准修补本身存疑。

**结论**：快速修补的历史先例真实；但正确表述应为"27.0.2 或 27.2"，而非"27.1"（普通 iPhone 无 27.1）。且由于漏洞未披露，修补时间存在不确定性，断言属合理推测而非确定预期。

---

## 断言 5："留在旧版本等越狱"的安全风险：iOS 26.1 相比 27.0.1 缺少的修复

**裁决：方向属实，但"iOS 27 修复含在野利用零日"表述不准确（置信度 85%）**。

### iOS 27 修复规模（属实）

> "The release addresses approximately 126 vulnerabilities within the operating system, including flaws affecting the kernel, sandboxing mechanisms, WebKit, authentication services"[^28^]
> —— GBHackers，2026-09-16，https://gbhackers.com/apple-releases-ios-27-security-update/

> "Apple released iOS 27 on September 14 with fixes for more than 120 vulnerabilities, including 20 kernel vulnerabilities"[^9^]（另见 ZDNet："120+ security fixes"）

### "含在野利用零日"（不准确）

苹果在 iOS 27 发布时的安全公告**未提及任何漏洞已在野被利用**：

> "Apple makes no mention of any of these security defects being exploited in the wild."[^29^]
> —— SecurityWeek，2026-09-15，https://www.securityweek.com/apple-patches-200-vulnerabilities-with-new-ios-27-macos-golden-gate-27-releases/

> "Apple's advisories do not indicate that any iOS 27 vulnerabilities have been actively exploited in the wild at the time of release."[^28^]

2026 年 9 月确有在野零日，但修补在 **iOS 26.7.1**（26 分支）而非 iOS 27：

> "Apple is aware of a report that this issue may have been exploited in an extremely sophisticated attack against specific targeted individuals on versions of iOS before iOS 27."[^30^][^31^]
> —— Apple 公告，CVE-2026-86950（CoreGraphics 越界写），2026-09-28 随 iOS 26.7.1 修复；BleepingComputer/The Register 2026-09-29。苹果安全发布页同时注明 iOS 27.0.1 "This update has no published CVE entries"[^32^]

（附注：zerohour.day 提到 iOS 27 修复了一个可由恶意 iMessage 触发、绕过 BlastDoor 的零点击漏洞 CVE-2026-86869，但"use before the fix is not confirmed"——修复前是否被利用未获确认[^34^]。）

### iOS 26.1（2025-11 初）相比 27.0.1 实际缺失的修复链

1. **iOS 26.2 → 26.7.1 全部安全更新**：仅 26.7 一个版本就含 80+ 修复（其中 75 个与 iOS 27 共享）[^35^]；26.6.1 修复 28+1 个漏洞[^29^]。
2. **2026 年两个已确认在野利用的零日**：CVE-2026-20700（dyld，2026 年 2 月修复）与 CVE-2026-86950（CoreGraphics，2026-09-28 修复）[^30^][^37^]；另有 Coruna 漏洞套件相关的 4 个被利用漏洞修复[^30^]。
3. **iOS 27 独有的约 50 项修复**（126 − 75 个共享项）及硬件级安全特性。
4. iOS 27.0.1 本身无 CVE 修复[^32^]，但 26.1 用户也不在其直接受益范围外——26.1 的暴露面是上述 1–3 项的累积。

**结论**：报告的核心判断——"为等越狱而停留在 iOS 26.1 意味着暴露在包括已确认在野利用零日在内的大量未修复漏洞下"——成立且证据充分；但若报告称"iOS 27 修复了在野利用的零日"，则与苹果官方公告不符（在野零日修在 26.7.1；iOS 27 发布时无已确认在野利用）。另注：留在 26.1 也无法获得 Calif 演示的越狱（该越狱针对 27.0.1），"留旧版等越狱"在当前时间线下两头落空。

---

## 总评

| # | 断言 | 裁决 | 置信度 |
|---|------|------|--------|
| 1 | iOS 27 假越狱基本是骗局 | 属实（"付费调查"手法未获 iOS 27 专项记录） | 90% |
| 2 | Cellebrite 断言与 Calif 演示的挑战 | 属实（注意 Cellebrite 营销动机；"发布前数周"应为"演示前数周"） | 95% |
| 3 | 间谍软件市场背景 + Calif 无放出动机 | 前提属实；结论为合理推断（存在"in due time"二手反向报道） | 75–95% |
| 4 | 苹果将在"27.0.x 或 27.1"修补 | 部分属实（历史响应速度真实；普通 iPhone 无 27.1，应为 27.0.2/27.2） | 80% |
| 5 | 留旧版的安全风险 | 方向属实（"iOS 27 含在野零日修复"不准确，零日修在 26.7.1） | 85% |

---

## 引用来源

[^1^]: Cellebrite, "Why Jailbreak Is Over with iOS 26", 2026-09-28, https://cellebrite.com/en/blog/ios-risk-for-mobile-app-developers-and-security-teams/
[^2^]: GBHackers, "Researchers Demonstrate iOS 27.0.1 Untethered Jailbreak on iPhone 18 Pro", 2026-10-16, https://gbhackers.com/ios-27-0-1-untethered-jailbreak/
[^3^]: ONE Jailbreak, "Calif Demos Untethered iOS 27.0.1 Jailbreak on iPhone 18 Pro", 2026-10-16, https://onejailbreak.com/blog/calif-demos-untethered-ios-27-0-1-jailbreak-on-iphone-18-pro/
[^4^]: Cyber Security News, "iOS 27.0.1 Untethered Jailbreak Demonstrated on iPhone 18 Pro", 2026-10-16, https://cybersecuritynews.com/ios-27-0-1-jailbreak-demonstrated/
[^5^]: Cellebrite/Corellium, "nekoJB Jailbreak Scam? Investigating the iOS 18.7 Fake Jailbreak Trap", 2026-06-08, https://cellebrite.com/en/blog/nekojb-jailbreak-scam/
[^6^]: NHI Mgmt Group, "Fake iOS jailbreak scams are turning profiles into credential traps", 2026-08-18, https://nhimg.org/articles/fake-ios-jailbreak-scams-are-turning-profiles-into-credential-traps/
[^7^]: PCMag, "Scammer Uses Fake iPhone Jailbreak for Fraud Scheme", 2019-10-15, https://www.pcmag.com/news/scammer-uses-fake-iphone-jailbreak-for-fraud-scheme
[^8^]: CyberScoop, "Scammers are dangling an iOS jailbreak to trick victims into downloading a malicious app", 2019-10-15, https://cyberscoop.com/ios-jailbreak-checkm8-app-fraud/
[^9^]: Mallory, "Calif Demonstrates Reboot-Persistent iOS 27.0.1 Jailbreak on iPhone 18 Pro", 2026-10-16, https://mallory.ai/stories/01a144a5-33e9-786d-9d6a-311be6bd2189
[^10^]: explainx, "Calif.io Jailbreaks iOS 27.0.1 on iPhone 18 Pro: What Is Claimed vs. Verified", 2026-10-16, https://explainx.ai/blog/calif-io-ios-27-0-1-untethered-jailbreak-iphone-18-pro-hexacon-ai-claimed-vs-verified-2026
[^11^]: Digit, "Anthropic Mythos helped researchers break Apple M5 shield on macOS: Report", 2026-05-15, https://www.digit.in/news/general/anthropic-mythos-helped-researchers-break-apple-m5-shield-on-macos-report.html
[^12^]: 163.com, "苹果的安全系统一直难以破解 Mythos帮助研究人员找到了入侵途径", 2026-05-15, https://www.163.com/dy/article/KSVN9AT30511BLFD.html
[^13^]: CERT-EU Threat Memo, "Android exploits commanding higher price than ever before", https://cert.europa.eu/publications/threat-intelligence/threat-memo-190910-1/pdf
[^14^]: mapshock, "Zero-Day Market Dynamics: Broker Pricing, Government Procurement, and Disclosure Norms", 2026-08-22, https://mapshock.com/briefings/zero-day-market-dynamics-broker-pricing-government
[^15^]: BrightDefense, "80+ Zero-Day Exploit Statistics", 2026-09-02, https://www.brightdefense.com/resources/zero-day-exploit-statistics/
[^16^]: cybersecurity-help.cz, "Exploit broker offers millions of dollars for iOS, Android zero-day exploits", https://www.cybersecurity-help.cz/blog/3916.html
[^17^]: SecurityWeek, "Google Catches Russian APT Reusing Exploits From Spyware Merchants NSO Group, Intellexa", 2024-08-29, https://www.securityweek.com/google-catches-russian-apt-re-using-exploits-from-spyware-merchants-nso-group-intellexa/
[^18^]: Cyber Kendra, "CVE-2026-86950: The CoreGraphics Glyph Bug That Hit iPhones", 2026-10-01, https://www.cyberkendra.com/2026/10/cve-2026-86950-the-coregraphics-glyph-bug-that-hit-iphones.html
[^19^]: GajShield KB, "CVE-2026-86950 PoC Shows How a Malicious PDF Font Can Corrupt Apple Memory", 2026-10-01, https://kb.gajshield.com/a/1534/
[^20^]: SecureMac, "The Great iOS Jailbreak of 2019—and why you should update to 12.4.1 right now!", 2019-08-26, https://www.securemac.com/news/the-great-ios-jailbreak-of-2019-and-why-you-should-update-to-12-4-1-right-now
[^21^]: TheAppleWiki, "Unc0ver" (v3.5.0, 2019-08-18), https://theapplewiki.com/wiki/Unc0ver
[^22^]: SentinelOne Vulnerability Database, "CVE-2023-32434", https://www.sentinelone.com/vulnerability-database/cve-2023-32434/
[^23^]: Wikipedia, "iOS 27"（版本历史表）, https://en.wikipedia.org/wiki/IOS_27
[^24^]: blakecrosley.com, "Xcode 27.1 Beta: Your App in the iPhone Duo Simulator", 2026-10-11, https://blakecrosley.com/blog/xcode-27-1-beta-iphone-duo-simulator
[^25^]: macsonly.com, "macOS 27.2 Beta 1 is Out... 27.1 Skipped for the iPhone Duo", 2026-09-21, https://macsonly.com/article/2026/macos-27-2-beta-1-is-out
[^26^]: Zeera Wireless, "Apple Released iOS 27.2 Beta Before iOS 27.1", 2026-09-17, https://zeerawireless.com/blogs/news/ios-27-2-beta-features-why-before-ios-27-1
[^27^]: 9to5Mac, "iOS 27.1 release date: Here's when to expect the new iPhone update", 2026-09-11, https://9to5mac.com/2026/09/11/ios-27-1-release-date-heres-when-to-expect-the-new-iphone-update/
[^28^]: GBHackers, "Apple Releases iOS 27 Security Update to Fix Over 120 Vulnerabilities", 2026-09-16, https://gbhackers.com/apple-releases-ios-27-security-update/
[^29^]: SecurityWeek, "Apple Patches 200 Vulnerabilities With New iOS 27, macOS Golden Gate 27 Releases", 2026-09-15, https://www.securityweek.com/apple-patches-200-vulnerabilities-with-new-ios-27-macos-golden-gate-27-releases/
[^30^]: BleepingComputer, "Apple patches CoreGraphics zero-day flaw exploited in attacks", 2026-09-29, https://www.bleepingcomputer.com/news/security/apple-patches-coregraphics-zero-day-flaw-exploited-in-attacks/
[^31^]: The Register, "Apple patches CoreGraphics zero-day already exploited in targeted attacks", 2026-09-29, https://www.theregister.com/security/2026/09/29/apple-patches-coregraphics-zero-day-already-exploited-in-targeted-attacks/5299721
[^32^]: Apple, "Apple security releases", https://support.apple.com/en-us/100100
[^33^]: blakecrosley.com, "iOS 27.0 Release Notes", 2026-10-01, https://blakecrosley.com/blog/ios-27-release-notes
[^34^]: zerohour.day, "CVE-2026-86950"（含 CVE-2026-86869 说明）, https://zerohour.day/cves/CVE-2026-86950
[^35^]: MacRumors Forums, "iOS 27 and iOS 26.7 Fix More Than 100 Security Bugs", 2026-09-14, https://forums.macrumors.com/threads/2489301/
[^36^]: 4uKey (Tenorshare), "Jailbreak iOS 27: Latest Status, Tools & Safer Options", 2026-08-28, https://www.4ukey.com/unlock-iphone/ios-27-jailbreak.html
[^37^]: SOC Prime, "CVE-2026-20700: Apple Patches Zero-Day Exploited in Sophisticated Cyber Attacks", 2026-02-12, https://socprime.com/blog/cve-2026-20700-vulnerability/
[^38^]: HEAL Security, "New usbliter8 SecureROM Exploit Enables iOS 27 Jailbreak on iPhone 11 Pro", 2026-07-27, https://healsecurity.com/new-usbliter8-securerom-exploit-enables-ios-27-jailbreak-on-iphone-11-pro/
[^39^]: Pasquale Pillitteri, "Calif Unveils iPhone 18 Pro Jailbreak on iOS 27.0.1 at Hexacon", 2026-10-16, https://pasqualepillitteri.it/en/news/24540/jailbreak-iphone-18-pro-ios-27-calif-hexacon
