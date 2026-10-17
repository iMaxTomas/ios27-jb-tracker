# 事实核查报告：Calif 在 Hexacon 2026 演示 iOS 27.0.1 越狱相关断言

- **核查日期**：2026-10-17
- **核查方法**：网络搜索（英/法/德/中，共 7 轮 27 组查询）+ 直接抓取一手页面（hexacon.fr、calif.io、blog.calif.io、support.apple.com）
- **核查范围**：6 条断言，见下

---

## 总体结论一览

| # | 断言（摘要） | 裁决 | 置信度 |
|---|---|---|---|
| 1 | Hexacon 由法国 Synacktiv 主办的攻击性安全大会，2026-10-16/17 巴黎 Palais Brongniart | **属实** | 高 |
| 2 | 2026-10-16 Calif（Calif.io）在大会舞台演示 iPhone 18 Pro 上 iOS 27.0.1 untethered 越狱 | **属实**（演示事实确凿；技术声明未经独立复现） | 高 |
| 3 | Calif 是"接大厂安全评估的盈利团队"，官网提供 AI red teaming 与 AI 辅助漏洞挖掘 | **属实** | 高 |
| 4 | Calif 此前多次 AI 辅助挖洞记录（4 月 Safari / 5 月 M5 / 8 月屏幕共享 / 9 月 14 日 iOS 27 致谢） | **属实**（4 个子项均有一手来源；个别子项有措辞细节需注意） | 高 |
| 5 | Calif newsletter 列出 4 个 demo（含 Signal one-click、iMessage demo），称"will be released in due time" | **属实**（一手来源逐字吻合） | 高 |
| 6 | 大会今年自称"Mythos era"（与 Anthropic 模型相关） | **部分属实**（"Mythos era"精确表述仅单一二手来源；官网一手材料支持大会以 Mythos 为标志的 AI 时代框架） | 中 |

---

## 逐条核查

### 断言 1：Hexacon 大会基本信息 —— 属实（置信度：高）

**验收条件**：(a) 大会存在且为 offensive security 定位；(b) 主办方为法国 Synacktiv；(c) 日期 2026-10-16/17；(d) 地点巴黎 Palais Brongniart。

**一手来源证据**：

- hexacon.fr CFP 页逐字写道：

  > "**16th & 17th of October 2026 | Palais Brongniart, 16 Place de la Bourse, 75002 Paris**"
  > "Hexacon is a single-track security conference focused on technical offensive security talks. The conference will take place from the 16th of October 2026 to the 17th, in the heart of Paris."[^1^]

- hexacon.fr 团队页确认 Synacktiv 身份：

  > "The support of **Synacktiv, our company**, is essential in this adventure."
  > "For more than 10 years now, Synacktiv worked hard to become the French reference among offensive security shops."[^2^]

- 第三方历史佐证：SonarSource 对首届大会的回顾（2022-10-25）："This is the first edition of this event organized by the French security firm Synacktiv"[^3^]；CSOH 会议目录："Founded 2022. Single-track, exclusively offensive research... (Synacktiv, Quarkslab, Reverse Tactics)"[^4^]。

**细节说明**：hexacon.fr 自身的法律表述是 "The Hexacon team"（22 人、1 家公司）为组织方，Synacktiv 为背后公司；因此"由 Synacktiv 主办"是准确且与官网一致的通俗说法。票务信息（EUR 660–1320）见第三方活动目录[^5^]。

---

### 断言 2：2026-10-16 Calif 演示 iOS 27.0.1 untethered 越狱 —— 属实（置信度：高）

**验收条件**：(a) 2026-10-16 在 Hexacon 舞台上发生演示；(b) 演示方为 Calif（Calif.io）；(c) 目标为 iPhone 18 Pro / iOS 27.0.1；(d) 声称 untethered（重启后保留）。

**一手/近一手证据**：

- Hexacon 官方 X 账号当日发帖（2026-10-16）：

  > "A brand new iPhone jailbreak has just been unveiled on stage at Hexacon by the @calif_io team."[^6^]

- Billy Ellis（@bellis1000）现场视频帖（2026-10-16）：

  > "At #hexacon - Calif.io demos iOS 27.0.1 untethered jailbreak on iPhone 18 Pro"[^7^]

- Hexacon 官网议程：Day 1（10 月 16 日周五）10:15 **Opening Keynote — Bruce Dang, Thai Duong**（两人单位均标注为 Calif.io）[^8^]。

**独立媒体交叉验证**（均 2026-10-16/17 发布）：

- heise online（英文版，2026-10-16）："The jailbreak is also said to be untethered, meaning it survives reboots without requiring a reset."[^9^]
- Decrypt（2026-10-16/17）："Security firm Calif.io demonstrated an untethered jailbreak on an iPhone 18 Pro running iOS 27.0.1 on stage at Hexacon, the offensive security conference in Paris, on Friday."[^10^]
- CyberSecurityNews（2026-10-16）：报道了完整技术细节（iPhone19,2、T8160、Darwin 27.0.0、build 24A446、SSH 2222 端口 uid=0、neofetch 显示 67 个 dpkg 包、uptime 2 分钟）[^11^]。
- Cyberpress[^12^]、GBHackers[^13^]、Yahoo Tech[^14^] 内容一致。

**必须标注的边界**：

1. 所利用漏洞未披露、无 CVE、无公开工具；untethered 等核心技术声明**只有舞台演示与现场视频为证，未经独立复现**。Decrypt 引 heise："it's not yet clear which vulnerabilities Calif.io exploited."[^10^]
2. 演示中手机是**用正常 passcode 解锁的**，不构成"绕过锁屏"或"远程/零点击攻击"的证据（Cyberpress 明确指出此点）[^12^]。
3. "2 分钟"是 neofetch 显示的 uptime（重启后已开机时长），不是 exploit 执行耗时[^15^]。

---

### 断言 3：Calif 公司性质与官网服务 —— 属实（置信度：高）

**验收条件**：(a) Calif 真实存在且为营利性安全公司；(b) 承接大厂安全评估；(c) 官网提供 AI 模型/智能体 red teaming 与 AI 辅助漏洞挖掘服务。

**官网一手证据**（calif.io，首页）[^16^]：

> "We're the hacker team that industry titans call first. Even security companies lean on us."
> "Calif secures the AI you use every day. Red teaming and security engineering from the hackers trusted by OpenAI, Anthropic, Google, Cursor and xAI."
> "### AI red teaming — We attack models, agents, and the systems wired around them: jailbreaks, prompt injection, tool abuse, and data exfiltration. Since 2023."
> "### AI-driven vulnerability research — We deploy and tune the pipelines that discover, exploit, and patch bugs at machine speed, then hand them over running inside your own environment."
> "### Security assessments — Red teaming, infrastructure and product reviews..."

官网客户证言区包含来自 Google 等客户的评价："Our team at Google has engaged with Calif on various security assessments, during which they identified weaknesses and helped us harden our mitigations."[^16^]

**第三方公司信息**：

- Crunchbase：Calif，创始人 **An Trinh、Thai Duong**，"provides security services including red teaming, security engineering, and cryptography consulting"[^17^]。
- WSJ（经 9to5Mac/iClarified 转引，2026-05-14）："The researchers with **Calif, a Palo Alto-based security research company**..."[^18^]
- NYT（经 Quartz/Anadolu 转引，2026-09）："Calif, a security company based in **Palo Alto, California**... Calif chief executive **Thai Duong**"[^19^]
- 招聘信息（Remotive/Jobgether）："Calif is a fast growing tech startup founded by world-class experts from Google. Our client portfolio includes **AWS, Google, SnapChat, Anthropic, and Let's Encrypt**"，并提供薪资/股权/福利（营利性商业公司的明确特征）[^20^]。
- LinkedIn 公司页约 3,439 关注者[^21^]；Thai Duong 个人页注明 "Chief at Calif"，其 bio（亦见 hexacon.fr 演讲者页）：12 年 Google 安全/密码学经历，BEAST/CRIME/POODLE 发现者之一，Pwnie 奖得主[^8^]。

**需注意的瑕疵**（不影响裁决，但报告应知悉）：

- 个别低权威来源称其为 "Vietnam-based"（atlas21、gncrypto）或数据聚合站给出 "Sunnyvale、2020 年创立、营收 $515,151"（prospeo，未经证实的估算值）[^22^]。主流一手/准一手证据（WSJ、NYT、Crunchbase、官网）一致指向**总部 Palo Alto, California**；Thai Duong 为越南裔（born in Saigon），可能造成混淆。公开渠道未检索到公司注册文件（Delaware/加州备案）的直接记录——此项为小的未验证空白。
- "接大厂安全评估"的描述与客户名单（OpenAI、Anthropic、Google、Cursor、xAI、AWS、SnapChat、Let's Encrypt）及官网证言一致，准确。

---

### 断言 4：Calif 此前 AI 辅助挖洞记录 —— 属实（置信度：高）

逐子项核查：

**(a) 2026 年 4 月 Thai Duong 称 Claude 帮助分析 iOS Safari exploit —— 属实**

一手来源：Calif Newsletter（blog.calif.io，2026-04-13/15）：

> "**Learning to Jailbreak an iPhone with Claude (Part 1)** — Claude helped me take apart an iOS Safari exploit, and retune it for my Mac. It even wrote its own variant."（2026-04-13 发布）[^23^]
> "The vehicle is **Coruna**, a commercial spyware kit that leaked in full recently: a complete Safari-to-kernel chain for iOS 17... Part 1 is the first stage: CVE-2024-23222..."[^24^]

⚠️ 措辞细节：该工作是**拆解/改造已泄露的 Coruna 商业间谍软件利用链**（含将 exploit 重调适配 Mac），并非"发现"新 Safari 漏洞。断言用词"帮助分析"准确。Decrypt 亦记载此事[^10^]。

**(b) 2026 年 5 月 macOS M5 内核 exploit —— 属实**

一手来源（blog.calif.io，2026-05-14）：

> "The exploit is a data-only kernel local privilege escalation chain targeting macOS 26.4.1 (25E253). It starts from an unprivileged local user, uses only normal system calls, and ends with a root shell... targeting bare-metal M5 hardware with kernel MIE enabled."
> "Bruce Dang found the bugs on April 25th. Dion Blazakis joined Calif on April 27th. Josh Maine built the tooling, and by May 1st we had a working exploit."[^25^]

独立验证：WSJ 报道（2026-05-14）[^18^]；Apple 在 macOS Tahoe 26.5 安全说明中修复 **CVE-2026-28952**（Kernel），致谢逐字为 "Calif.io in collaboration with Claude and Anthropic Research"[^26^]；TechRadar、Tom's Hardware、SC Media、heise 均有独立报道[^27^]。

**(c) 2026 年 8 月屏幕共享漏洞 —— 属实**

一手来源（calif.io/research/password-fiction，2026-08-10）：

> "**Two pre-auth macOS remote root exploits in four hours.** On Thursday August 6, Apple shipped an emergency macOS update. It fixed exactly one vulnerability, **CVE-2026-65400**, in a feature called Screen Sharing... We started reverse engineering the emergency update and had a working exploit for the bug they fixed about four hours later"[^28^]

背景佐证：Malwarebytes（2026-08-17）确认 CVE-2026-65400 为 macOS Screen Sharing 认证绕过漏洞、被在野利用（安装 Monero 矿机），Apple 于 8 月 6 日带外修复[^29^]。
⚠️ 细节：此子项性质为**对 Apple 补丁的 N-day 逆向与 exploit 复现**；"使用 AI"主要来自 heise 的表述（"used artificial intelligence to derive an exploit from Apple's fix for a serious screen-sharing vulnerability"）[^30^]，Calif 该文摘录中未直接写明 AI 角色。

**(d) 2026-09-14 iOS 27 安全说明致谢 Calif.io + Claude + Anthropic Research —— 属实（Apple 官网一手确认）**

Apple 官方安全说明 "About the security content of iOS 27 and iPadOS 27"（support.apple.com/en-us/149034，2026-09-14）中两处逐字致谢[^31^]：

> "Impact: An app may be able to cause a denial of service ... **CVE-2026-65409: Bruce Dang of Calif.io in collaboration with Claude and Anthropic Research**"
> "Impact: An app may be able to cause unexpected system termination ... **CVE-2026-65410: Calif.io in collaboration with Claude and Anthropic Research**"

另该说明中 **CVE-2026-28968**（内核越界写入）致谢含 "Josh Maine of Calif.io"。Decrypt 的描述（"two bugs that let an app cause a denial of service or a system crash. Nothing in the notes ties either bug to the jailbreak"）与 Apple 原文一致[^10^]。

---

### 断言 5：Newsletter 4 个 demo 与 "released in due time" —— 属实（置信度：高）

一手来源：Calif Newsletter 文章 "Today, we hacked what we'd set out to hack: the iPhone."（blog.calif.io/p/iphone，2026-10-16），逐字内容[^32^]：

> "Below is a series of demos we revealed at Hexacon in Paris today. **Everything will be released in due time.**
> 1/ Signal 1-click
> 2/ But who cares about a third-party app when everyone uses iMessage, right?
> 3/ Can your iPhone do this?
> 4/ APT stands for Apple Persistent Threat"

同一内容亦由 Calif 官方 X 账号 @calif_io 以 4 条连续帖发布（2026-10-16）[^33^]。Decrypt 转述："Its newsletter post from the conference lists four demos, including a Signal one-click and an iMessage demo, and says everything 'will be released in due time.'"[^10^] 与原文逐字吻合。
⚠️ 细节：断言称"含 Signal one-click、iMessage demo"——准确（第 2 条即 iMessage demo，配文为反问句）。另有社交媒体流传的"iMessage demo 涉及恶意 PDF"说法被 Glitchwire 标注为**无法独立核实**，不应作为事实引用[^34^]。

---

### 断言 6：大会自称"Mythos era" —— 部分属实（置信度：中）

**核查结果**：

- **支持面**："Mythos" 确为 Anthropic 的模型名（Claude Mythos / Mythos Preview，2026 年 4 月以 Project Glasswing 框架向受限伙伴开放；6 月 Mythos 5 / Fable 5 发布及出口管制风波有大量独立报道）[^35^]；"Mythos era" 作为行业术语在 2026 年被广泛使用（Bitsight、Holm Security、Chainguard、HCL 等多家安全厂商内容均以 "Mythos era" 指代 Anthropic 模型开启的 AI 挖洞时代）[^36^]。
- **大会侧直接证据（一手）**：hexacon.fr 官网开幕 keynote（即 Calif 的 Bruce Dang & Thai Duong 演讲）摘要逐字写道：

  > "2026 has been the year of AI for vulnerability research. Within a period of a few months, **frontier models such like Mythos** and the likes have changed the entire research community and its experts' outlook on computer security."[^8^]

- **不足面**："大会今年自称 'Mythos era'" 这一**精确表述**仅见于单一二手来源 explainx.ai（2026-10-16，权威度 B）："The conference positions itself this year as operating in the 'Mythos era,' a reference to the AI cyber-capable models..."[^37^]。对 hexacon.fr 全站（首页、agenda、speakers、newsletter）的直接抓取**未发现 "Mythos era" 字样**；官网出现的相近表述仅有培训标题 "Attacking Instant Messaging Applications in the **LLM Era**"。hexacon_fr 官方 X 帖中亦未检索到该词。

**裁决理由**：将 "Mythos era" 归于"大会自称"缺乏一手来源支撑；但大会官方议程/演讲框架确以 Mythos（Anthropic 模型）为 2026 年 AI 安全研究的标志性参照，该短语与 Anthropic 模型的关联真实存在。故为部分属实：方向正确，精确归属（"大会自称"）证据不足。

---

## 交叉验证中发现的其它一致事实（背景增强）

- iOS 27.0.1（build 24A446）为 Apple 2026-09-28 发布的点更新，无公开 CVE；iOS 27 于 2026-09-14 发布[^31^][^38^]。
- heise 称此为"自 iOS 9 以来首个 reboot-proof 越狱"（经 GBHackers 转引）[^13^]。
- Calif 2026 年 9 月还披露了 AI 辅助构建的 WeChat 蠕虫 "WeWorm"（NYT 报道），腾讯确认并修复[^19^]——进一步印证"AI 辅助挖洞团队"的公司画像。

## 未能完全验证的范围（诚实声明）

1. 越狱的**技术真实性**（漏洞链构成、untethered 机制的普适性）：无技术报告、无 CVE、无第三方复现，仅有舞台演示与视频证据。
2. "Mythos era" 作为大会官方自我定位的**原始出处**（可能存在于 Hexacon 会前 newsletter 邮件或现场物料，公开网页未留存）。
3. Calif 的**工商注册信息**（注册州、法人实体名）未在公开检索中定位。
4. 8 月屏幕共享 exploit 中 AI 的具体角色（Calif 自述未明说，heise 单方面归因）。

---

## 引用来源

[^1^]: Hexacon 官网 — Call for Papers, https://www.hexacon.fr/conference/call-for-papers/ （访问于 2026-10-17）
[^2^]: Hexacon 官网 — Team, https://www.hexacon.fr/about/team/ （访问于 2026-10-17）
[^3^]: SonarSource Blog — "Bits from Hexacon 2022", 2022-10-25, https://www.sonarsource.com/blog/bits-from-hexacon-2022/
[^4^]: CSOH — Security & Hacker Conferences, https://csoh.org/conferences.html （页面标注 2026-04-29）
[^5^]: Luminik — "Hexacon 2026, Paris: dates and venue", https://www.luminik.io/events/hexacon/
[^6^]: X（@hexacon_fr）, 2026-10-16, https://x.com/hexacon_fr/status/2111030563020669055
[^7^]: X（@bellis1000）现场视频, 2026-10-16, https://x.com/bellis1000/status/2111028042495824337/video/2
[^8^]: Hexacon 官网 — Speakers / Agenda, https://www.hexacon.fr/conference/speakers/ ; https://www.hexacon.fr/conference/agenda/ （访问于 2026-10-17）
[^9^]: heise online — "Works on iPhone 18 Pro: AI helps create untethered jailbreak for iOS 27.0.1", 2026-10-16, https://www.heise.de/en/news/Works-on-iPhone-18-Pro-AI-helps-create-untethered-jailbreak-for-iOS-27-0-1-11490907.html
[^10^]: Decrypt — "Hackers Jailbreak iPhone 18 Pro on Stage, and It Survives a Reboot", 2026-10-16/17, https://decrypt.co/381177/hackers-jailbreak-iphone-18-pro-survives-reboot
[^11^]: CyberSecurityNews — "iOS 27.0.1 Untethered Jailbreak Demonstrated on iPhone 18 Pro", 2026-10-16, https://cybersecuritynews.com/ios-27-0-1-jailbreak-demonstrated/
[^12^]: Cyberpress — "iOS 27.0.1 Jailbreak Survives Reboot and Restores Root Access on iPhone 18 Pro", 2026-10-16, https://cyberpress.org/ios-27-0-1-jailbreak-survives-reboot/
[^13^]: GBHackers — "Researchers Demonstrate iOS 27.0.1 Untethered Jailbreak on iPhone 18 Pro", 2026-10-16, https://gbhackers.com/ios-27-0-1-untethered-jailbreak/
[^14^]: Yahoo Tech — "Hackers Jailbreak iPhone 18 Pro on Stage, and It Survives a Reboot", 2026-10-16, https://tech.yahoo.com/cybersecurity/articles/hackers-jailbreak-iphone-18-pro-193604534.html
[^15^]: Pasquale Pillitteri — "Calif Unveils iPhone 18 Pro Jailbreak on iOS 27.0.1 at Hexacon", 2026-10-16, https://pasqualepillitteri.it/en/news/24540/jailbreak-iphone-18-pro-ios-27-calif-hexacon
[^16^]: Calif 官网首页, https://calif.io/ （访问于 2026-10-17）
[^17^]: Crunchbase — Calif company profile, https://www.crunchbase.com/organization/calif-837d
[^18^]: 9to5Mac（转引 WSJ/Robert McMillan）— "New macOS vulnerabilities were exposed by Anthropic's Mythos: report", 2026-05-14, https://9to5mac.com/2026/05/14/new-macos-vulnerabilities-were-exposed-by-anthropics-mythos-report/ ；iClarified 同日报道 https://www.iclarified.com/100859/
[^19^]: Quartz（转引 NYT）— "Calif security firm built AI-powered WeChat worm WeWorm", 2026-09-08, https://tech.yahoo.com/cybersecurity/articles/calif-security-firm-built-ai-141643566.html ；Anadolu Agency, 2026-09-09, https://www.aa.com.tr/en/asia-pacific/ai-hacking-tool-builds-worm-that-can-infect-chinas-wechat-accounts-with-call-report/4052290
[^20^]: Remotive — "Offensive Security Engineer @Calif", 2026-08-14, https://remotive.com/remote/jobs/all-others/offensive-security-engineer-5547165 ；Jobgether 同岗位 https://jobgether.com/offer/6ac5bd388a27695aea358bb3-offensive-security-engineer
[^21^]: LinkedIn 公司页镜像 — "Calif | 3,439 followers", https://app.icebreaker.xyz/companies/www.linkedin.com%2Fcompany%2Fcalifio
[^22^]: Prospeo 聚合数据（低可靠性，仅作对照）, https://prospeo.io/c/calif ；Atlas21, 2026-05-15, https://atlas21.com/claude-mythos-ai-exploit-on-apple-m5-developed-in-less-than-a-week/
[^23^]: Calif Newsletter — 文章列表摘录, https://blog.calif.io/p/mad-bugs-month-of-ai-discovered-bugs （2026-04-16）
[^24^]: Calif Newsletter — "Learning to Jailbreak an iPhone with Claude (Part 1)", 2026-04-15, https://blog.calif.io/p/learning-to-jailbreak-an-iphone-with
[^25^]: Calif Blog — "First public macOS kernel memory corruption exploit on Apple M5", 2026-05-14, https://blog.calif.io/p/first-public-kernel-memory-corruption
[^26^]: LLM-Hacking — "Apple's May 2026 bulletin formally credits Claude on two macOS CVEs", 2026-05-27, https://www.llm-hacking.com/hacks/apple-may-2026-claude-credited-cves.md/ ；ByteIota, 2026-05-26, https://byteiota.com/apple-m5-mie-kernel-exploit-update-to-macos-26-5-now/
[^27^]: TechRadar/Yahoo, 2026-05-15, https://tech.yahoo.com/ai/claude/articles/anthropic-mythos-helped-build-working-181500559.html ；heise, 2026-05-15, https://www.heise.de/en/news/Security-Firm-Claude-Mythos-Discovers-macOS-Exploit-11295068.html
[^28^]: Calif Research — "Password Fiction", 2026-08-10, https://calif.io/research/password-fiction
[^29^]: Malwarebytes — "Update your Mac: Screen Sharing vulnerability exploited in the wild", 2026-08-17, https://www.malwarebytes.com/blog/bugs/2026/08/update-your-mac-screen-sharing-vulnerability-exploited-in-the-wild
[^30^]: Mallory 事件时间线（引 heise online 报道）, 2026-10-16/17, https://mallory.ai/stories/01a144a5-33e9-786d-9d6a-311be6bd2189
[^31^]: Apple Support — "About the security content of iOS 27 and iPadOS 27", 2026-09-14, https://support.apple.com/en-us/149034
[^32^]: Calif Newsletter — "iPhone"（"Today, we hacked what we'd set out to hack: the iPhone."）, 2026-10-16, https://blog.calif.io/p/iphone
[^33^]: X（@calif_io）4 条 demo 帖, 2026-10-16, https://x.com/calif_io/status/2111080247944745106
[^34^]: Glitchwire — "Calif Demonstrates Untethered iOS 27.0.1 Jailbreak at Hexacon, Hints at Signal and iMessage Exploits", 2026-10-16, https://glitchwire.com/news/calif-demonstrates-untethered-ios-2701-jailbreak-at-hexacon-hints-at-signal-and/
[^35^]: CNET — "Anthropic Pulls Claude Fable and Mythos AI Models After Feds Claim Jailbreak", 2026-06-15, https://www.cnet.com/tech/services-and-software/anthropic-claude-fable-mythos-us-export-controls/ ；Holm Security — "Vulnerability Management in the Claude Mythos Era", 2026-04-28, https://www.holmsecurity.com/blog/claude-mythos-vulnerability-management
[^36^]: Bitsight — "The CISO's 90-Day AI (i.e. Claude Mythos) Threat Response Playbook", 2026-05-20, https://www.bitsight.com/guides/the-cisos-90-day-ai-claude-mythos-threat-response-playbook ；Chainguard webinar, 2026-09-15, https://www.chainguard.dev/events/board-ready-security-metrics-for-the-mythos-era
[^37^]: explainx.ai — "Calif.io Jailbreaks iOS 27.0.1 on iPhone 18 Pro: What Is Claimed vs. Verified", 2026-10-16, https://explainx.ai/blog/calif-io-ios-27-0-1-untethered-jailbreak-iphone-18-pro-hexacon-ai-claimed-vs-verified-2026
[^38^]: ONE Jailbreak — "Calif Demos Untethered iOS 27.0.1 Jailbreak on iPhone 18 Pro", 2026-10-16, https://onejailbreak.com/blog/calif-demos-untethered-ios-27-0-1-jailbreak-on-iphone-18-pro/ ；NightSquawk（引 Apple 安全发布页）, 2026-10-10, https://nightsquawk.tech/blog/exploited-flaws-apple-fortimail/
