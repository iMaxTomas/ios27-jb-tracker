# 事实核查报告（维度 03）：Calif iOS 27.0.1 越狱是否为"自 iOS 9 以来首个重启级（untethered）越狱"

- **核查日期**：2026-10-17
- **核查方式**：17 次独立网络检索 + 原文页面逐字抓取
- **待核查断言**：报告称 2026-10-16 Calif 在 Hexacon 演示的 iOS 27.0.1 越狱是"自 iOS 9 以来首个重启级（untethered）越狱"

---

## 一、总裁决

**裁决：证伪（严格读法）/ 部分属实（宽松读法）。置信度：高（约 90%）。**

- 断言的字面表述（沿用 heise 的 "first reboot-proof jailbreak since iOS 9"）**不成立**：**Fugu14**（Linus Henze，2021-10-24 公开开源，iOS 14.3–14.5.1，arm64e 设备含 iPhone 12）是一个公开的、可重启不丢的 untethered 越狱，且被 Linus Henze 本人及其会议主办方明确定义为"自 iOS 9 以来首个公开可用的 untethered 越狱"[^14^][^15^]。因此 iOS 27.0.1 的这次演示至多能称为"**自 Fugu14（2021，iOS 14）以来**首个 untethered 越狱（且仅为演示、未发布工具）"。
- 若作最宽松解读（"在最新在售硬件 + 最新公开发售固件上的 untethered 演示"），断言接近事实，但这已不是报告原文的表述。
- 报告的转述**忠实于 heise 的一手说法**（GBHackers 明确标注 "Heise reports..."），问题出在 heise 的原始判断本身有误/不严谨，报告未做交叉核实即照录。

---

## 二、事件本身核查（2026-10-16，Hexacon，巴黎）

多家独立媒体报道一致，基本事实成立：

1. **演示事实**：2026-10-16，安全公司 Calif（Calif.io）在巴黎 Hexacon 2026（Synacktiv 主办的攻击性安全会议，10月16–17日）现场演示了 iPhone 18 Pro（iPhone19,2 / T8160 芯片）运行 iOS 27.0.1（build 24A446）的 untethered 越狱：重启后越狱环境自动恢复，SSH（端口 2222）以 root（uid=0）登录，neofetch 显示 67 个 dpkg 包、uptime 约 2 分钟[^1^][^2^]。
2. **演示者身份**：开场主题演讲者为 Bruce Dang 与 Thai Duong（XorNinja）；Billy Ellis 在 X 上发布了现场视频[^3^][^4^]。
3. **未发布工具**：未披露漏洞、无 CVE、未发布公开工具；Calif 称材料"will be released in due time"[^2^][^3^]。
4. **AI 角色**：heise 报道 AI 协助发现了漏洞链；Apple 9月14日 iOS 27 安全公告曾致谢 "Calif.io in collaboration with Claude and Anthropic Research"（两个与越狱无关的崩溃类漏洞）[^3^][^5^]。

**结论**：事件本身属实；争议仅在"历史性定位"（首个性）的表述。

---

## 三、两种原始表述逐字对照

### 3.1 heise（源头）

- 德文标题：《Dank KI: Rebootfester Jailbreak für iOS 27.0.1 bis zum iPhone 18 Pro, untethered》（2026-10-16）[^6^]
- 德文导语逐字摘录（经 heise 内容聚合镜像核对）：
  > "Das Team Calif.io hat den ersten rebootfesten Jailbreak seit iOS 9 entwickelt. Er läuft bis zur jüngsten Version und auch auf den neuesten iPhones. KI half."[^7^]
- 英文版导语逐字摘录：
  > "The Calif.io team has developed the first reboot-proof jailbreak since iOS 9. It works on the latest version and the newest iPhones."[^8^]

### 3.2 GBHackers（转引 heise，2026-10-17）

> "Heise reports it is the first reboot-proof jailbreak since iOS 9, and that AI helped find the exploit chain."[^9^]

GBHackers 处理得当——明确标注了消息来源归属（"Heise reports"），把判断责任归于 heise。

### 3.3 Decrypt（2026-10-16/17）

> "Security firm Calif.io demonstrated an untethered jailbreak on an iPhone 18 Pro running iOS 27.0.1 on stage at Hexacon, the offensive security conference in Paris, on Friday. It's the first time an untethered, permanent jailbreak is successful in such devices."[^3^]

### 3.4 差异分析：哪个更严谨？

| 维度 | heise / GBHackers 转述 | Decrypt |
|---|---|---|
| 断言强度 | 强断言："自 iOS 9 以来首个" | 弱断言："首次在此类设备上成功" |
| 可证伪性 | 高（可被单一反例推翻） | 低（"such devices" 指代模糊，可退缩解释为"最新一代 iPhone"） |
| 准确性 | **有明确反例（Fugu14），不成立** | 模糊但较难直接判错；不过 Fugu14 当年就运行在当时的旗舰 iPhone 12 上，严格说也站不住 |

**判定**：heise 的表述更具体、更"专业口吻"，但**具体而错**——这是更糟糕的失误，因为它把可核查的历史断言写错了。Decrypt 的表述更含糊、新闻严谨性较差，却反而更难被判错。对报告而言：报告照录了 heise 的强断言（"自 iOS 9 以来首个"），属于**忠实转述了一个错误源头**，且未做一行交叉验证。严谨的写法应为："heise 称其为自 iOS 9 以来首个 reboot-proof 越狱；但需注意 2021 年公开的 Fugu14（iOS 14.3–14.5.1）即为 untethered 越狱，故该说法仅在'最新硬件+最新固件'的限定下成立。"

---

## 四、iOS 9 之后至今（2026）公开越狱史及类型标注

类型定义（社区通行四分法）[^16^]：

- **untethered（完美/重启级）**：重启后越狱状态自动保留，无需任何操作；
- **semi-untethered（半完美）**：重启后越狱失效，但可用设备上的 App 重新激活，无需电脑；
- **semi-tethered（半不完美）**：重启后可正常开机进未越狱系统，但恢复越狱必须连电脑重跑工具；
- **tethered（不完美）**：每次开机都必须连电脑引导。

| 工具 | 发布 | 覆盖版本/设备 | 类型 |
|---|---|---|---|
| Pangu 9.0–9.0.2 | 2015-10 | iOS 9.0–9.0.2 全设备 | **untethered**[^10^] |
| Pangu 9.1 | 2016-03 | iOS 9.1（64 位） | **untethered**[^11^] |
| Pangu 9.2–9.3.3 | 2016-07-24 | iOS 9.2–9.3.3（64 位） | semi-untethered[^11^][^12^] |
| Phoenix 9.3.5/9.3.6 | 2017 | 32 位 iOS 9.3.5/9.3.6 | semi-untethered[^12^] |
| p0laris | 2022-04-20 | 32 位 iOS 9.3.5/9.3.6 | semi-untethered（untether 曾开发、后无限期搁置）[^13^] |
| Yalu / yalu+mach_portal / yalu102 | 2016-12 ~ 2017-01 | iOS 10.0–10.2 | semi-untethered[^12^][^17^] |
| extra_recipe+yaluX | 2017-05 | iOS 10.0–10.1.1（iPhone 7/7+） | semi-untethered[^17^] |
| doubleH3lix / Meridian / g0blin / Saïgon / h3lix | 2017-2018 | iOS 10.x | semi-untethered（h3lix 为 32 位）[^12^] |
| Electra | 2018 | iOS 11.0–11.4.1 | semi-untethered[^16^] |
| unc0ver（初期） | 2018-2019 | iOS 11–12.1.2 | semi-untethered[^16^] |
| Chimera | 2019-04-30 | iOS 12.0–12.5.x（A7–A12） | semi-untethered[^18^] |
| **checkra1n（checkm8）** | 2019-11 | A5–A11，iOS 12–14.8.1 | **semi-tethered**[^19^][^20^] |
| unc0ver 5.x（iOS 13）/ 6.x–8.x（iOS 14） | 2020-2021 | iOS 13.0–13.5 / 14.0–14.8 | semi-untethered[^16^] |
| Odyssey | 2020 | iOS 13.0–13.7 | semi-untethered[^16^] |
| Taurine | 2021-04-01 | iOS 14.0–14.3 | semi-untethered[^21^] |
| **Fugu14（+ unc0ver v7 集成）** | **2021-10-24** | **iOS 14.3–14.5.1，arm64e（A12–A14）** | **untethered（"不完整"开发者级，需 Mac+Xcode 手动部署）**[^14^][^15^][^22^] |
| palera1n（checkm8） | 2022-10 | A8–A11，iOS 15–16.x（及部分 iPadOS 17/18） | semi-tethered[^16^][^20^] |
| Fugu15 | 2022-10-31 | iOS 15.0–15.4.1（arm64e） | **semi-untethered**（perma-signed，非 untethered）[^23^][^24^] |
| XinaA15 | 2022-12-07 | iOS 15.0–15.4.1（A12–A15/M1） | semi-untethered[^25^] |
| Dopamine（原 Fugu15 Max）1.x/2.x | 2023–2025 | iOS 15.0–16.6.1（A8–A16、M1–M2） | semi-untethered[^16^][^26^] |
| meowbrek2 | 2024-01 | iOS 15.0–15.8.6（A8–A11） | semi-untethered[^27^] |
| Dopamine 2.5 Beta（DarkSword） | 2026-05 | 扩展至 iOS 16.7–16.7.16、iOS 17–18.7.1（A9–A11） | semi-untethered（实验性）[^28^] |
| **Calif iOS 27.0.1 演示** | 2026-10-16 | iPhone 18 Pro，iOS 27.0.1 | **untethered（仅会议演示，无公开工具）**[^1^][^3^] |

（"Dopamine 3.0" 见任务清单：仅在中文聚合站出现且类型标注混乱，未找到权威一手来源，本报告不对其定性。）

**全表结论**：2016 年 7 月（Pangu 9.2–9.3.3）之后，公开越狱几乎全部是 semi-untethered 或 semi-tethered；**唯一例外是 2021-10-24 的 Fugu14（untethered，公开开源）**。

---

## 五、关键反例：Fugu14 —— 对"自 iOS 9 以来首个"的证伪

1. **Fugu14 是公开、开源、untethered 的越狱**：The Apple Wiki 逐字："Fugu14 is an incomplete untethered jailbreak for iOS 14.3 to 14.5.1. It released on 24 October 2021."[^14^]
2. **作者本人及会议主办方的定性**（POC 2022 讲者简介，逐字摘录）：
   > "...as well as the developer of Fugu (iOS 13 checkm8 based jailbreak), **Fugu14 (iOS 14 untethered jailbreak - the first publicly available untethered jailbreak since iOS 9)** and Fugu15 (iOS 15 permasigned jailbreak)..."[^15^]

   即：社区公认的准确表述是"**Fugu14 是自 iOS 9 以来首个公开可用的 untethered 越狱**"。
3. **可用性佐证**：unc0ver v7.0.0（2021-10-24）集成 Fugu14，使 A12–A14 设备（含 iPhone 12 系列）在 iOS 14.4–14.5.1 上实现重启不丢的 untethered 越狱；随后 AltStore v1.4.8 提供了一键捆绑安装[^22^][^29^]。德文维基亦记载："Im November 2021 erschien von Linus Henze Fugu14, der bis dato letzte untethered Jailbreak für iOS-Geräte"（截至该条目撰写时，Fugu14 是 iOS 设备最后一个 untethered 越狱）[^30^]。
4. **可以替 heise 找的辩护及其局限**：Fugu14 被官方称为 "incomplete"（不完整：开箱无 tweak 注入、部署门槛高、面向开发者）。若 heise 的本意是"首个**完整、用户级**的 untethered"或"首个针对**当下在售最新硬件+最新固件**的 untethered"，则更接近事实——但这不是其字面表述。且 Calif 这次同样只是**会议演示、未发布工具**，按"演示 vs 公开工具"的同一标准，Fugu14 反而比 Calif 的演示"更公开"（代码在 GitHub 上人人可用）。

**因此**：
- "自 iOS 9 以来没有公开的 untethered 越狱" —— **不准确**（Fugu14 为公开例外）；
- "自 iOS 9 以来几乎没有公开的 untethered 越狱、且 2021 年 Fugu14 之后再无 untethered（直至本次演示）" —— **准确**。

---

## 六、checkm8 系越狱（checkra1n / palera1n）为何不算 untethered

- checkm8 是 A5–A11 BootROM 中的硬件级漏洞，Apple 无法用软件更新修复；但它**只在启动瞬间（DFU 模式）可利用**，利用效果不持久：设备完全重启后即回到未利用状态[^20^]。
- 因此 checkra1n/palera1n 属 **semi-tethered**：设备可自行正常开机进入未越狱系统，但要恢复越狱状态必须重新连接电脑、进 DFU、重跑工具[^19^][^20^]。
- "永久（unpatchable）"描述的是**硬件漏洞不可修复**，而非**越狱状态重启持久**——两者常被混淆。对比 untethered 的四格行为表：untethered 重启后"无需电脑、无需重跑 App、越狱仍生效"，checkra1n/palera1n 三项皆不满足后两项中的关键项[^16^]。

---

## 七、逐项任务结论

1. **越狱史梳理**：见第四节表格；核心事实链：2016-07 之后至 2021-10 之间公开工具全为 semi-untethered/semi-tethered；2021-10 Fugu14 为 untethered 公开例外；其后至 2026-10 之前再无公开 untethered。
2. **"自 iOS 9 以来没有公开的 untethered 越狱"是否准确**：**不准确**。严格读法下被 Fugu14（2021-10-24，公开开源，untethered，iOS 14.3–14.5.1）证伪[^14^][^15^]。另注意区分"公开工具 vs 演示"：本次 Calif 仅为演示；而若以演示计，历史上也有研究者演示过更高代际的 untethered（未发布）。两种口径下"首个"均站不住。
3. **heise vs Decrypt 表述**：heise 更具体但**具体而错**；Decrypt 更模糊但较难判错。报告照录 heise 强断言，属忠实转述错误源头、未加限定语，**不严谨**。建议改写为带归属和限定的表述（见 3.4 节末）。
4. **checkm8 为何不算 untethered**：BootROM 漏洞仅在 DFU 启动瞬间可利用、重启即失效，恢复越狱须连电脑重跑 → semi-tethered，而非 untethered[^19^][^20^]。

---

## 引用来源

[^1^]: Cyber Security News, "iOS 27.0.1 Untethered Jailbreak Demonstrated on iPhone 18 Pro", 2026-10-16. https://cybersecuritynews.com/ios-27-0-1-jailbreak-demonstrated/
[^2^]: CyberPress, "iOS 27.0.1 Jailbreak Survives Reboot and Restores Root Access on iPhone 18 Pro", 2026-10-16. https://cyberpress.org/ios-27-0-1-jailbreak-survives-reboot/
[^3^]: Decrypt, "Hackers Jailbreak iPhone 18 Pro on Stage, and It Survives a Reboot", 2026-10-16/17. https://decrypt.co/381177/hackers-jailbreak-iphone-18-pro-survives-reboot
[^4^]: Billy Ellis (@bellis1000) on X, 2026-10-16. https://x.com/bellis1000/status/2111028042495824337
[^5^]: explainX, "Calif.io Jailbreaks iOS 27.0.1 on iPhone 18 Pro: What Is Claimed vs. Verified", 2026-10-16. https://explainx.ai/blog/calif-io-ios-27-0-1-untethered-jailbreak-iphone-18-pro-hexacon-ai-claimed-vs-verified-2026
[^6^]: heise online, "Dank KI: Rebootfester Jailbreak für iOS 27.0.1 bis zum iPhone 18 Pro, untethered", 2026-10-16（标题经 Mallory 聚合页核对）. https://www.heise.de/news/ （正文页受访问限制，未能直接抓取）
[^7^]: aimbit.de（heise 内容聚合镜像，逐字收录 heise 导语）, 2026-10-16. https://aimbit.de/news
[^8^]: heise online (English), "AI helps create untethered jailbreak for iOS 27.0.1", 2026-10-16（搜索快照逐字）. https://www.heise.de/en/news/Works-on-iPhone-18-Pro-AI-helps-create-untethered-jailbreak-for-iOS-27-0-1-11490907.html
[^9^]: GBHackers, "Researchers Demonstrate iOS 27.0.1 Untethered Jailbreak on iPhone 18 Pro", 2026-10-17. https://gbhackers.com/ios-27-0-1-untethered-jailbreak/
[^10^]: TechCrunch, "Apple's iOS 9.1 Shuts Down The Pangu Hacking Team's Jailbreak", 2015-10-21. https://techcrunch.com/2015/10/21/apples-ios-9-1-shuts-down-the-pangu-hacking-teams-jailbreak/
[^11^]: The Apple Wiki, "Pangu9"（"iOS 9.0 through 9.1 (untethered), 9.2 through 9.3.3 ... (semi-untethered)"）. https://theapplewiki.com/wiki/Pangu9
[^12^]: iDownloadBlog, "Properly rebooting your iOS 9.2-10.2 device after jailbreaking with Pangu or Yalu"（semi-untethered 定义与 Pangu/Yalu 定性）, 2016-07-24. https://www.idownloadblog.com/2016/07/24/properly-rebooting-your-ios-9-3-3-device-after-jailbreaking-with-pangu/
[^13^]: iDownloadBlog, "New p0laris jailbreak for legacy iOS 9.x firmware released", 2022-04-20；The Apple Wiki, "P0laris"（untether 无限期搁置）. https://www.idownloadblog.com/2022/04/20/p0laris-ios-9-jailbreak/ ; https://theapplewiki.com/wiki/P0laris
[^14^]: The Apple Wiki, "Fugu14"（"incomplete untethered jailbreak for iOS 14.3 to 14.5.1 ... 24 October 2021"）. https://theapplewiki.com/wiki/Fugu14
[^15^]: Power of Community 2022 讲者页（Linus Henze 简介，"Fugu14 ... the first publicly available untethered jailbreak since iOS 9"）. https://powerofcommunity.net/2022/
[^16^]: iOSJailbreak.pro, "Untethered Jailbreak Explained: Does One Exist in 2026?"（四类越狱定义与行为对照表）, 2026-06-13；并见 iClarified 越狱史归档（"iOS 11-14: The era of the semi-untethered app jailbreak"）. https://iosjailbreak.pro/untethered-jailbreak-explained-2026.html ; https://www.iclarified.com/jailbreak
[^17^]: The Apple Wiki, "Yalu"（yalu 系列为 semi-untethered；extra_recipe+yaluX 发布史）. https://theapplewiki.com/wiki/Yalu
[^18^]: The Apple Wiki, "Chimera"（"semi-untethered jailbreak for A7-A11 ... 12.0-12.5.8"）. https://theapplewiki.com/wiki/Chimera
[^19^]: TechAcrobat, "checkra1n Linux Guide"（"checkra1n is generally described as a semi-tethered jailbreak ... run checkra1n again"）, 2026-10-04. https://www.techacrobat.com/checkra1n-linux-jailbreak/
[^20^]: TechAcrobat, "Checkm8 BootROM Exploit"（"The raw checkm8 exploit is non-persistent ... Checkra1n and Palera1n Are Normally Semi-Tethered Workflows"）, 2026-09-29. https://www.techacrobat.com/checkm8-jailbreak-exploit/
[^21^]: IPAOMTK, "Taurine Jailbreak iOS 14"（"Taurine is a semi-untethered open-source jailbreak ... iOS 14 up to iOS 14.3"）. https://ipaomtk.com/taurine-jailbreak/
[^22^]: iOSHacker, "Fugu14 Untethered Jailbreak Released, unc0ver Updated With Preliminary Support", 2021-10-25；iphonetweak.fr, "Unc0ver 7.0 : le jailbreak iOS 14.5.1 est désormais untethered avec fugu14", 2021-10-25. https://ioshacker.com/news/fugu14-untethered-jailbreak-released-unc0ver-updated-with-preliminary-support ; https://iphonetweak.fr/2021/10/25/unc0ver-7-0-jailbreak-ios-14-5-1-desormais-untethered-fugu14
[^23^]: The Apple Wiki, "Fugu15"（"Fugu15 is a semi-untethered jailbreak for iOS 15"）. https://theapplewiki.com/wiki/Fugu15
[^24^]: iDownloadBlog, "Linus Henze releases Fugu15 jailbreak for iOS 15", 2022-10-31（"semi-untethered and perma-signed"）. https://www.idownloadblog.com/2022/10/31/linus-henze-releases-fugu15-jailbreak-ios-15/
[^25^]: The Apple Wiki, "XinaA15"（"XinaA15 is a semi-untethered jailbreak"）. https://theapplewiki.com/wiki/XinaA15
[^26^]: The Apple Wiki, "Open Source Jailbreaking Tools"（Dopamine: "a rootless semi-untethered jailbreak"）. https://theapplewiki.com/wiki/Open_Source_Jailbreaking_Tools
[^27^]: The Apple Wiki, "Meowbrek2"（"semi-untethered jailbreak for any arm64 devices running iOS 15.0-15.8.6"）. https://theapplewiki.com/wiki/Meowbrek2
[^28^]: UltFone, "Jailbreak iOS 27 Beta: Best Tools"（Dopamine 2.5 Beta 3, 2026-05, DarkSword, 扩展至 iOS 17–18.7.1 on A9–A11；semi-untethered）, 2026-06-04. https://www.ultfone.com/ios-27/jailbreak-ios-27.html
[^29^]: iDownloadBlog, "Linus Henze updates Fugu14 untether for reliability as Pwn20wnd updates unc0ver jailbreak to v7.0.1", 2021-10-28. https://www.idownloadblog.com/tag/ios-14-5-1/
[^30^]: 德语维基百科, "Jailbreak (iOS)"（Fugu14 为"迄今为止最后一个 untethered iOS 越狱"）. https://de.wikipedia.org/wiki/Jailbreak_(iOS)
