# 事实核查报告：Calif 在 Hexacon 2026 演示 iOS 27.0.1 越狱（iPhone 18 Pro）

- 核查日期：2026-10-17
- 核查方式：英文为主的网络搜索，共 19 次独立查询；优先采用原始来源（heise online、Decrypt、CyberSecurityNews、Cyberpress、Billy Ellis / calif_io 的 X 帖子、ONE Jailbreak），并以独立技术数据库（EveryMac、AppleDB、The Apple Wiki、Geekbench 报道、真实用户内核 panic 日志）做旁证。
- 背景事件：2026-10-16，安全公司 Calif.io 在巴黎 Hexacon 2026（Synacktiv 主办，10月16–17日）舞台上现场演示了 iPhone 18 Pro / iOS 27.0.1 的 untethered 越狱。该事件本身被 heise、Decrypt、CyberSecurityNews、Cyberpress、ONE Jailbreak 等多家独立媒体报道，并有现场观众（Billy Ellis）拍摄的视频为证，**事件真实性无争议**；以下为逐条断言核查。

---

## 逐条裁决

### 断言 1：演示机型为 iPhone 18 Pro，内部型号 iPhone19,2，芯片 T8160，iOS 27.0.1 build 24A446，Darwin Kernel 27.0.0 / XNU 13432.2.10

**裁决：属实（置信度：高）**

- CyberSecurityNews 对演示画面的逐帧还原（原始技术细节来源）：
  > "The uname output showed Darwin Kernel 27.0.0 built on XNU 13432.2.10 for the T8160 chip, with the model listed as iPhone19,2. The sw_vers command confirmed iOS 27.0.1, build 24A446" [^1^]（2026-10-16）
- Cyberpress 独立复述同一组数据：
  > "The `uname` output identified Darwin Kernel 27.0.0, built on XNU 13432.2.10 for the T8160 chip, alongside the device identifier iPhone19,2. The `sw_vers` command reported iOS 27.0.1, build 24A446." [^2^]（2026-10-16）
- 旁证（与苹果惯例/真实数据完全吻合）：
  - **iPhone19,2 = iPhone 18 Pro**：EveryMac 规格页明确标注 "Identifiers: iPhone 18 Pro - ... iPhone19,2" [^3^]；AppleDB 同 [^4^]；Geekbench 跑分记录亦以 iPhone19,2 对应 iPhone 18 Pro [^5^]。
  - **T8160 = A20 Pro 的 CPID**：A19/A19 Pro 的 CPID 为 T8150 [^6^]；芯片数据库明确列出 "A20 Pro (Borneo, T8160)" [^7^]，The Apple Wiki 代号表亦将 T8160 列于 A20 一代（代号 Banda/Borneo）[^8^]。完全符合苹果 T 系列编号惯例。
  - **build 24A446 = iOS 27.0.1**：维基百科 iOS 27 版本表 "Latest version: 27.0.1 | 24A446 | September 28, 2026" [^9^]；独立开发者对 Apple releases feed 的核对亦确认 27.0.1 即 24A446 [^10^]。
  - **XNU 13432.2.10 确为 iOS 27.0.1 的真实内核**：一条独立的 iPhone 15（iPhone15,4）用户 panic 日志显示 `"os_version":"iPhone OS 27.0.1 (24A446)"` 且内核为 `Darwin Kernel Version 27.0.0: Thu Aug 13 21:41:29 PDT 2026; root:xnu-13432.2.10~2/RELEASE_ARM64_T8120` [^11^]；另一条 iPhone 13 Pro（T8110）日志同样为 xnu-13432.2.10 [^12^]。The Apple Wiki 内核表显示 iOS 27.0 beta/RC 均为 Darwin 27.0.0、xnu-13432.x 分支 [^13^]。即演示中 uname 显示的 XNU 版本与真实 27.0.1 固件逐字一致——这是强有力的真实性旁证（伪造者极少能把内核 build 号编对）。
  - 注：现实中 Darwin 主版本号自 iOS 26 起与 iOS 主版本号错位（iOS 26 = Darwin 25），而本事件中 iOS 27 对应 Darwin 27.0.0，已由 The Apple Wiki 内核表（27.0 beta 起即 Darwin 27.0.0）独立证实，属苹果版本编排变化，不构成矛盾 [^13^]。

### 断言 2：演示流程——重启 → Apple logo → 锁屏 → 正常输入密码 → SSH 端口 2222 以 root 登录（uid=0）→ neofetch 显示 dpkg 安装 67 个包、uptime 约 2 分钟

**裁决：属实（置信度：高）**

- CyberSecurityNews：
  > "It booted to the Apple logo, reached the lock screen, and was unlocked with the passcode. ... the team connected to the phone over SSH on port 2222 and logged in as root. The id command returned uid=0 ... neofetch listed 67 packages installed through dpkg with an uptime of only two minutes." [^1^]
- Cyberpress 逐字印证：
  > "they subsequently connected to the device through SSH on port 2222 and authenticated as root. Running the `id` command returned `uid=0` ... `neofetch` displayed 67 packages installed through `dpkg` and an uptime of approximately two minutes, consistent with the recent reboot." [^2^]
- Decrypt 对 Billy Ellis 现场视频的描述独立印证"重启—解锁—root SSH"流程：
  > "Video from the stage, posted by jailbreak researcher Billy Ellis, shows the team rebooting the phone, unlocking it with the regular passcode and opening an SSH session as root ... The terminal output listed iOS 27.0.1 and an uptime of about two minutes." [^14^]（2026-10-16）
- Billy Ellis 原始 X 帖（视频来源）：
  > "At #hexacon - Calif.io demos iOS 27.0.1 untethered jailbreak on iPhone 18 Pro" — @bellis1000，2026-10-16 9:34 AM [^15^]
- 小注：SSH 走 2222 端口符合越狱社区惯例（iproxy 常将本机 2222 转发到设备 SSH 端口），无异常。

### 断言 3："两分钟"是重启后的 uptime，不是 exploit 执行耗时（社交媒体上存在误读）

**裁决：属实（置信度：高）**

- 误读实例（原始出处）：International Cyber Digest 的 X 帖称
  > "Its Hexacon demo shows the phone on iOS 27.0.1, Apple's latest, giving up a root shell within 2 minutes of boot." [^16^]（2026-10-16）
- ONE Jailbreak 亦沿用了这一表述："Reports from people in the room said the phone gave up a root shell roughly two minutes after boot." [^17^]
- 澄清：pasqualepillitteri 明确指出
  > "The two minutes circulating in social media posts are the uptime neofetch reported at the moment of the session. They measure how long the phone had been on after the reboot, not how long the exploit takes to run, which remains unknown." [^16^]
- Decrypt 与 CyberSecurityNews 的原始描述均只说 "uptime of about/only two minutes"，从未称其为 exploit 耗时 [^1^][^14^]。gbhackers 亦指出 International Cyber Digest 将其概括为 "a root shell within two minutes of boot" 属二次传播 [^18^]。

### 断言 4：演示在"已解锁"的手机上进行，不涉及绕过锁屏，不证明陌生人能进入锁定的 iPhone

**裁决：属实（置信度：高）**

- pasqualepillitteri：
  > "The demo also never touched the lock screen. The phone was unlocked with the passcode the way any owner would do it, so the video does not prove that a stranger holding a locked iPhone could get in." [^16^]
  并在 FAQ 中重申："In the Paris demo the passcode was entered normally, so the second point was not proven." [^16^]
- Cyberpress 从另一角度佐证（不涉及远程入口）：
  > "Root access alone also does not reveal the initial exploitation vector. The presentation therefore should not be interpreted as evidence of a remotely deliverable or zero-click attack." [^2^]
- 三家报道对视频流程的描述（重启→锁屏→输入机主密码）与此断言自洽 [^1^][^14^][^17^]。

### 断言 5：漏洞未披露、无 CVE、无公开工具；heise 称适用于"直至 iOS 27.0.1 + iPhone 18 Pro 系列"；能否用于更早芯片未知

**裁决：属实（置信度：高）**

- 未披露/无工具：Cyberpress："the researchers have not disclosed the underlying vulnerabilities or released a public jailbreak tool" [^2^]；gbhackers："No CVEs have been assigned or confirmed." [^18^]；heise（经 explainx 转引）："it's not yet clear which vulnerabilities Calif.io exploited" [^19^]。
- 适用范围：heise 的表述（经两家独立转述相互印证）：
  - pasqualepillitteri："heise online writes that the jailbreak works up to and including iOS 27.0.1 and across the iPhone 18 Pro family." [^16^]
  - explainx："heise describes the talk as the first jailbreak for iOS 27, working on versions up to iOS 27.0.1 including the iPhone 18 Pro family." [^19^]
  - heise 德文版摘要（官方 teaser）："Das Team Calif.io hat den ersten rebootfesten Jailbreak seit iOS 9 entwickelt. Er läuft bis zur jüngsten Version und auch auf den neuesten iPhones. KI half." [^20^]
- 更早芯片未知：pasqualepillitteri FAQ："Heise mentions compatibility with the iPhone 18 Pro family and with iOS up to 27.0.1, but nobody knows whether the chain also runs on earlier chips, since hardware defenses change from one generation to the next." [^16^]

### 断言 6：heise 称 AI 帮助找到了 exploit 链但未说明具体方式；"对比 iOS 27.2 beta 与 27.0.1"只是 heise 的猜测

**裁决：属实（置信度：高）**

- explainx 直接引用 heise 英文版：
  > "heise: AI 'once again helped find the exploit chain.' ... speculates that the team could have done the same here by comparing iOS 27.2 betas with iOS 27.0.1. It flags that Apple has published no documentation on this and that the search 'would presumably require a lot of tokens.' That is speculation, not a finding." [^19^]
- heise 英文版原文片段（搜索索引可见）："Apple has yet to publish any documentation on this, however, so the search would presumably require a lot of tokens." [^21^]
- pasqualepillitteri 独立转述同一判断："Heise reports that artificial intelligence once again helped find the exploit chain, without explaining how. The outlet speculates that the team may have compared the iOS 27.2 betas with 27.0.1 ... That is a conjecture, not a confirmed fact." [^16^]
- 旁证（Calif 确有 AI 辅助漏洞研究的历史，但不证明本次细节）：Decrypt 报道 Apple 9月14日 iOS 27 安全说明曾致谢 Calif.io "in collaboration with Claude and Anthropic Research" 的两个崩溃类漏洞（与本次越狱无关）；Calif 5月曾称 Claude Mythos Preview 助其一周内置出 macOS 内核 exploit [^14^][^22^]。
- 旁证：iOS 27.2 当时确已在 beta（beta 4 build 24B5110a，2026-10-13），故"对比 beta"在时间线上成立 [^9^][^23^]。

### 断言 7：Apple 未发表任何声明；ONE Jailbreak 称工作将"in due time"发布（注意：这是转述，未必是 Calif 原话）

**裁决：部分属实（置信度：高）——主体正确，但括号内的谨慎表述需要修正**

- Apple 未表态：属实。Decrypt："Apple has not publicly commented on the demo as of Friday, and says it does not confirm security issues until patches are available." [^14^]；pasqualepillitteri："Apple, for its part, has issued no statement." [^16^]
- "in due time" 的归属：ONE Jailbreak 的确只是二手转述——"Ellis later said the presenters mentioned that the work would be released in due time." [^17^]
- **但**"未必是 Calif 原话"这一句不准确：Calif 官方 X 账号 @calif_io 当天自己发的帖子标题即为
  > "Demos we presented at Hexacon in Paris today. Everything will be released in due time." [^24^]（2026-10-16）
  Decrypt 亦直接引用了 Calif 自己的 newsletter："Its newsletter post from the conference lists four demos ... and says everything 'will be released in due time.'" [^14^]
- 因此："in due time" 既是 ONE Jailbreak 的转述，**也确实出现在 Calif 自己的公开帖文中**。断言主体成立，括号中的保留意见过度谨慎，应修正为"ONE Jailbreak 是二手转述，但该措辞与 Calif 本人帖文一致"。

---

## 补充发现（供报告作者参考，非断言核查项）

1. heise 称此为"自 iOS 9 以来首个 rebootfester（重启后仍有效的）越狱" [^20^]；该历史定位仅 heise 一家表述，未独立验证。
2. Decrypt 报道 Apple 的 iOS 27 安全说明（9月14日）曾致谢 "Calif.io in collaboration with Claude and Anthropic Research" 两个 DoS/崩溃漏洞，但明确与该越狱无关 [^14^]。
3. ONE Jailbreak 指出社区曾猜测是否复用本月稍早被 Apple 封堵的 TSS/服务端签名 bug（该 bug 曾实现 iOS 27 半越狱），但本次演示被描述为当前公开版上的 untethered，路径不同 [^17^]。
4. Calif 的 Hexacon newsletter 共列出四个演示（Signal one-click、iMessage、"Can your iPhone do this?"、"APT stands for Apple Persistent Threat"），均称 "will be released in due time" [^14^][^24^][^25^]。
5. 演示演讲者为 Bruce Dang 与 Thai Duong（XorNinja），Calif CEO Thai Duong 为前 Google 安全研究员（BEAST/CRIME/POODLE 共同发现者）——团队资历侧面增强可信度 [^17^][^25^]。
6. 限制说明：heise 原文页面（heise.de 德/英两版）在核查环境中无法直接打开（多次超时/被拒），对 heise 观点的引用均通过 explainx、pasqualepillitteri 两家独立转述 + 搜索引擎索引中的 heise 原文片段交叉确认，内容一致。

---

## 引用来源

[^1^]: CyberSecurityNews, "iOS 27.0.1 Untethered Jailbreak Demonstrated on iPhone 18 Pro", 2026-10-16. https://cybersecuritynews.com/ios-27-0-1-jailbreak-demonstrated/
[^2^]: Cyberpress, "iOS 27.0.1 Jailbreak Survives Reboot and Restores Root Access on iPhone 18 Pro", 2026-10-16. https://cyberpress.org/ios-27-0-1-jailbreak-survives-reboot/
[^3^]: EveryMac, "iPhone 18 Pro (US/A3472) Specs (… iPhone19,2)". https://everymac.com/systems/apple/iphone/specs/apple-iphone-18-pro-united-states-a3472-specs.html
[^4^]: AppleDB, "iPhone 18 Pro — Identifier iPhone19,2, iPhone19,3, iPhone19,7; SoC A20 Pro". https://appledb.dev/device/iPhone-18-Pro-series.html
[^5^]: PC Guide, "iPhone 18 Pro benchmarks … Apple A20 Pro chip" ("The iPhone19,2 identifier refers to the iPhone 18 Pro"), 2026-09-16. https://www.pcguide.com/news/iphone-18-pro-benchmarks-reveal-impressive-scores-for-apple-a20-pro-chip-even-outperforming-desktop-cpus/
[^6^]: Wikipedia, "Apple A19"（A19/A19 Pro Part Number T8150）, 2025-09-09. https://en.wikipedia.org/wiki/Apple_A19
[^7^]: 果粉查询芯片数据库, "A20 Pro (Borneo, T8160) 芯片参数". https://www.guofenchaxun.com/devices/silicon/chip_a/a20-pro-borneo-t8160
[^8^]: The Apple Wiki, "Codenames"（A20 一代 T8160 / Banda / Borneo）, 2026-10-07. http://theapplewiki.com/wiki/User:Ilikeiphone123/Playground/Codenames
[^9^]: Wikipedia, "iOS 27 — Version history"（27.0.1 = 24A446, 2026-09-28）. https://en.wikipedia.org/wiki/IOS_27
[^10^]: Blake Crosley, "iOS 27.0 Release Notes"（Apple releases feed: 27.0.1 build 24A446, 2026-09-28）, 2026-10-01. https://blakecrosley.com/blog/ios-27-release-notes
[^11^]: otvet.mail.ru 用户 panic 日志（iPhone15,4, iPhone OS 27.0.1 (24A446), xnu-13432.2.10~2/RELEASE_ARM64_T8120）, 2026-10-13. https://otvet.mail.ru/question/270601050?reply
[^12^]: Reddit r/iOS27, iPhone 13 Pro panic 日志（xnu-13432.2.10~2/RELEASE_ARM64_T8110）. https://www.reddit.com/r/iOS27/comments/1wp5mqd/
[^13^]: The Apple Wiki, "Kernel"（iOS 27.0 beta 起 Darwin 27.0.0 / xnu-13432.x；27.0 RC = xnu-13432.1.9）, 2026-10-13. http://theapplewiki.com/wiki/Kernel
[^14^]: Decrypt, "Hackers Jailbreak iPhone 18 Pro on Stage, and It Survives a Reboot", 2026-10-16. https://decrypt.co/381177/hackers-jailbreak-iphone-18-pro-survives-reboot
[^15^]: Billy Ellis (@bellis1000) on X, "At #hexacon - Calif.io demos iOS 27.0.1 untethered jailbreak on iPhone 18 Pro", 2026-10-16. https://x.com/bellis1000/status/2111028042495824337
[^16^]: Pasquale Pillitteri, "Calif Unveils iPhone 18 Pro Jailbreak on iOS 27.0.1 at Hexacon"（含 International Cyber Digest 原帖引用与 uptime 澄清）, 2026-10-16. https://pasqualepillitteri.it/en/news/24540/jailbreak-iphone-18-pro-ios-27-calif-hexacon
[^17^]: ONE Jailbreak, "Calif Demos Untethered iOS 27.0.1 Jailbreak on iPhone 18 Pro", 2026-10-16. https://onejailbreak.com/blog/calif-demos-untethered-ios-27-0-1-jailbreak-on-iphone-18-pro/
[^18^]: GBHackers, "Researchers Demonstrate iOS 27.0.1 Untethered Jailbreak on iPhone 18 Pro", 2026-10-17. https://gbhackers.com/ios-27-0-1-untethered-jailbreak/
[^19^]: explainX, "Calif.io Jailbreaks iOS 27.0.1 on iPhone 18 Pro: What Is Claimed vs. Verified", 2026-10-16. https://explainx.ai/blog/calif-io-ios-27-0-1-untethered-jailbreak-iphone-18-pro-hexacon-ai-claimed-vs-verified-2026
[^20^]: heise online, "Dank KI: Rebootfester Jailbreak für iOS 27.0.1 bis zum iPhone 18 Pro, untethered", 2026-10-16. https://www.heise.de/news/Untethered-Jailbreak-fuer-iOS-27-0-1-auf-iPhone-18-Pro-der-Reboots-ueberlebt-11490726.html
[^21^]: heise online (English), "Works on iPhone 18 Pro: AI helps create untethered jailbreak for iOS 27.0.1", 2026-10-16. https://www.heise.de/en/news/Works-on-iPhone-18-Pro-AI-helps-create-untethered-jailbreak-for-iOS-27-0-1-11490907.html
[^22^]: Mallory, "Calif Demonstrates Reboot-Persistent iOS 27.0.1 Jailbreak on iPhone 18 Pro", 2026-10-16. https://mallory.ai/stories/01a144a5-33e9-786d-9d6a-311be6bd2189
[^23^]: The Apple Wiki, "List of iPhones"（iPhone 18 Pro Max 最新 beta 固件 27.2 beta 3 (24B5099f)）, 2026-09-30. http://theapplewiki.com/wiki/List_of_iPhones
[^24^]: Calif (@calif_io) on X, "Demos we presented at Hexacon in Paris today. Everything will be released in due time.", 2026-10-16. https://x.com/calif_io/status/2111080240978223545
[^25^]: Glitchwire, "Calif Demonstrates Untethered iOS 27.0.1 Jailbreak at Hexacon, Hints at Signal and iMessage Exploits", 2026-10-16. https://glitchwire.com/news/calif-demonstrates-untethered-ios-2701-jailbreak-at-hexacon-hints-at-signal-and/
