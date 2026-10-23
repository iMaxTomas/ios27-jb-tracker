# 「Calif 发售日攻破 iPhone Duo」社区与媒体舆情汇总

**事件时间**：2026-10-23（iPhone Duo 全球发售日）10:27 UTC，Calif（@calif_io）发推宣布 "we popped the iPhone Duo on release day"
**报告撰写**：2026-10-23（事发当日，媒体报道仍在发酵中，多家主流媒体专题稿尚未落地）
**方法**：≥14 轮中英文独立搜索 + fxtwitter API 一手核实两条推文 JSON

---

## 0. 事件本体（一手核实，已证实）

通过 fxtwitter API 直接读取两条推文原始数据[^fetch_urls:455#0^][^fetch_urls:455#1^]：

- **主推文**（status/2113578130543882697，10-23 10:27 UTC）："Just in case it wasn't clear: we popped the iPhone Duo on release day, adding a much needed feature, inspired by the php2shell hacker @lcfr_hax! What else there to hack? Wrong answers only." 截至抓取：627 赞、53 转发、53 回复、6 条引用、约 3.57 万浏览。
- **被引用的 6 秒演示视频推文**（status/2113570095549337977，10-23 09:55 UTC）：正文仅 "CC @pwnlambda"，附 720×1280 竖屏视频（6 秒）。截至抓取：**793,745 次观看**、3,748 赞、336 转发、128 条引用、111 回复。
- **@lcfr_hax 梗的背景（已证实）**：两天前（10-21）Calif 刚发推"正名"，称独立研究者 @lcfr_hax 是 PHP2Shell 漏洞（CVE-2026-109618/109619）的首个发现者并开发出可用 exploit，其工作促使 PHP 基金会出资修复整个漏洞类别[^web_search:463#10^]。PHP2Shell 是本周安全圈最大新闻（10-21 补丁落地、多家厂商观测到在野利用尝试）[^web_search:463#6^][^web_search:463#7^]。Calif 在 Duo 推文里引用此人，是借"php2shell"双关炫耀"在 Duo 上拿到 shell"（**推断，非官方解释**）。
- 推文语气是典型 Calif 营销风格（"What else there to hack? Wrong answers only."），未提供任何漏洞细节、CVE 或工具。

---

## 1. 媒体报道

### 1.1 针对 Duo 发售日攻破的报道（截至检索时）

**重要负面结果**：Duo 推文发布仅数小时，**The Verge、9to5Mac、MacRumors、ZDNET、Forbes 均尚未检索到针对 Duo 被攻破的专题报道**；heise 已有的两篇越狱报道均针对 10-16 Hexacon 演示[^web_search:477#11^][^web_search:477#5^]。当日与 Duo 相关的英文主流媒体稿集中在**缺货/首发无现货**主题（MacRumors→PCMag 转引[^web_search:482#1^]、9to5Mac/iClarified[^web_search:486#4^]）。中文媒体当日热搜同样以"首发秒空、排队、限购 2 台"为主[^web_search:503#6^]，未见 Duo 越狱专题。**该部分的媒体反应预计在未来 24–72 小时内出现，当前属舆情真空期。**

### 1.2 前哨报道：10-16 Hexacon iPhone 18 Pro / iOS 27.0.1 untethered 越狱（Duo 事件的直接铺垫）

各媒体角度与新增信息：

| 媒体 | 角度 | 新增信息 |
|---|---|---|
| **heise online**（德国） | "Dank KI（多亏了 AI）"：强调 AI 辅助找到漏洞链；"自 iOS 9 以来首个 rebootfest 越狱" | 覆盖范围至 iOS 27.0.1、iPhone 18 Pro 全系；猜测团队对比 iOS 27.2 beta 与 27.0.1 找已修复差异（heise 自己标注为猜测）；苹果未回应[^web_search:477#11^][^web_search:454#6^] |
| **Decrypt / Yahoo Tech** | 能力=商业间谍软件同级；梳理 Calif×Anthropic 合作史（4 月 Safari 分析、5 月 M5 内核 exploit） | 苹果 iOS 27 安全说明（9-14）致谢 Calif.io "in collaboration with Claude and Anthropic Research" 两个崩溃 bug，但**未与越狱关联**；苹果不评论[^web_search:477#0^][^web_search:482#4^] |
| **Cyberpress** | 技术细节还原 | iPhone19,2 / T8160 芯片 / Darwin 27.0.0（XNU 13432.2.10）/ build 24A446 / SSH 2222 端口 / uid=0 / 67 个 dpkg 包；强调演示用正常密码解锁、**不等于锁屏绕过或远程攻击**[^web_search:482#11^] |
| **ONE Jailbreak**（越狱垂直站） | 社区祛魅 | 主讲人为 Bruce Dang（前苹果 iOS 安全/PCC）与 Thai Duong（XorNinja）；提醒 2022 年 Todesco 也在 Hexacon 演示过 iOS 16.1 beta 越狱——"会议演示证明漏洞类别存在，不等于明天就有工具"[^web_search:504#12^][^web_search:482#20^] |
| **glitchwire** | 挖出 Calif newsletter 四项演示清单 | Signal 一键攻击、iMessage 攻击（"who cares about a third-party app…"）、"can your iPhone do this?"、"APT = Apple Persistent Threat"；结尾 "Everything will be released in due time"；另还原 Calif 此前对 CoreGraphics CVE-2026-86950 的 PDF 路径研究[^web_search:454#17^] |
| **iClarified / RedmondPie / iphonea2 / soydemac 等苹果垂直站** | 科普+风险提示 | 一致确认无公开工具、无日期；"iOS is up to date" 仍显示[^web_search:504#11^][^web_search:504#17^] |
| **IT之家 / 搜狐 / 新浪 / 网易(IT时代网) / 爱思助手**（中文） | 编译 Decrypt 稿+科普"完美越狱"概念 | 爱思助手角度最贴近果粉："引爆果粉圈"，但明确"仅为学术演示，无终端用户工具，发布时间和版本均未定"[^web_search:495#3^][^web_search:454#8^][^web_search:454#9^][^web_search:504#6^][^web_search:454#13^] |
| **微博长文（@geoshow 账号 ttarticle）** | 中文圈最深技术还原 | 称越狱工具名为 **Chollima**，流程覆盖沙盒逃逸→内核读写→**SPTM 绕过**→持久化；拿到 root 后装了 **Sileo 和 tweak（Gravity Lite）**；重启后锁屏出现 Calif 标志与 "From Calif with LOVE" 字样、设置里多 Calif 图标、关于本机仍显示 "iOS is up to date"[^web_search:504#4^]（单一来源+配图，**中可信**） |
| **kocpc（台湾）** | 推文评论区社区情绪还原（详见 §2） | —[^web_search:454#23^] |

### 1.3 背景类相关报道

- **Forbes**（David Phelan）：Duo 发售时间线报道（9-30），确认 10-23 发售，无越狱内容[^web_search:454#1^]。
- **MacRumors**：8-7 报道 opa334 发布 Dopamine 3.0（iOS 26 首个越狱，A12/A13），构成"公开越狱滞后 326 天"的背景[^web_search:473#3^]。
- **36氪**（5-15）：中文重磅报道 Calif 用 Mythos 5 天攻破 M5 硬件防护，"苹果防线全线血崩"——MIE 公信力讨论的中文源头[^web_search:503#0^]。

---

## 2. 越狱社区反应

### 2.1 推文评论区（Hexacon 推文，kocpc 一手还原）[^web_search:454#23^][^web_search:501#9^]

- 演示推文 **3,600+ 赞、400+ 转发**；
- **opa334（Lars Fröder，Dopamine/TrollStore 作者）现身评论区**，称 "Hexacon FOMO"（没能去现场的遗憾）——这是知名开发者目前唯一被记录到的公开评论；
- **评论区三大高频问题**：① Lockdown Mode 能不能挡？② 漏洞是否已上报苹果？③ 是否 90 天后披露？——Calif 均未回答；
- 有人追问 iPhone 17 Pro Max 是否也被攻破、是否只能停在 27.0.1 不能升 27.2；
- 有人直接呼吁 Calif 开 Patreon，"越狱社区愿意掏钱支持"。

### 2.2 关于"是否会有可用工具"——社区共识明确：**不会有**

- **【已证实】** Calif newsletter 仅说 "Everything will be released in due time"，无日期、未承诺工具形态[^web_search:454#17^][^web_search:482#0^]；iClarified 指出这句话无法判断是"可用越狱、技术研究还是 exploit 代码"[^web_search:504#11^]。
- **【背景给定，未独立核实】** 任务背景称成员 Stefan Esser（i0n1c）表示"将披露漏洞但不做终端用户工具"。旁证：Esser 历史上多次演示越狱但拒绝发布工具（iOS 8.4 beta 等），曾因 Pangu 使用其培训漏洞公开斥责对方为"小偷"——"只秀不发"是其一贯立场[^web_search:490#2^][^web_search:490#4^]。
- **【已证实】** The Apple Wiki 越狱状态页（2026-10-23 更新）已列出 iPhone Duo（最新固件 27.1）：**"No Jailbreak tool"**[^web_search:490#19^]。
- **【观点】** onejailbreak 编辑：Calif 不是"发 Cydia/Sileo 包的传统越狱团队"，而是做 AI 红队和漏洞研究的公司，其链条在漏洞市场价值极高，"不会免费送人"[^web_search:482#20^]；pasqualepillitteri 分析同样指出"无公开工具的演示是最常见情形"[^web_search:454#6^]。
- **【观点】** opa334 今年 7 月就 AI 辅助越狱（Relaxin 事件）公开表态：不反对别人基于 Dopamine（MIT 协议）开发，但认为"把学习过程外包给 AI 然后发布，却不承担长期维护责任是不负责任的"，并质疑"AI 民主化安全研究"的叙事[^web_search:473#16^]——可作为其对 Calif 式 AI 打法的间接态度参照。
- CoolStar、Pwn20wnd、johnny 等其他知名开发者：**未检索到任何公开评论**（负面结果）。
- r/jailbreak：Reddit 直连与本环境的 URL 审计均被拒，**未能取得热帖一手数据**（局限说明）；间接证据显示社区情绪"kocpc：像过年一样"[^web_search:501#9^]。

---

## 3. 安全业界反应

### 3.1 "发售日即被攻破"与 MIE 公信力

- **【已证实】** 苹果 2025-09 推出 MIE（Memory Integrity Enforcement），自称"消费级操作系统历史上最重要的内存安全升级"，内部攻击团队实测**无法重建已知 exploit 链绕过 MIE**[^web_search:466#3^]；Corellium/Cellebrite 8 月发文称 "MIE 让依赖越狱的 iOS 测试过时"[^web_search:466#17^][^web_search:466#12^]。
- **【已证实】** 但 Calif 今年 5 月就用 Anthropic Mythos Preview **在 5 天内绕过 M5 的 MIE 防护**做出 macOS 内核 exploit[^web_search:454#6^][^web_search:503#0^]；此次 Hexacon（A19/iPhone 18 Pro）+ Duo 发售日（A20 Pro）连续击破，构成对 MIE 叙事的二次打击。Duo 搭载与 iPhone 18 Pro 同款 A20 Pro[^web_search:503#5^]，防护代际一致（**推断：Duo 沿用同类硬件防护**）。
- **【观点】** 业界主流评价并非"苹果防线崩溃"，而是"攻防耐心战的常态"：Decrypt 指出此类链条与商业间谍软件能力同级、价值数百万美元[^web_search:482#0^]；pasqualepillitteri 认为"AI 已成为攻防双方共用的加速器"[^web_search:492#5^]。
- **【传闻，未证实】** glitchwire 称社交媒体流传 iMessage 演示为"带陷阱的 PDF，在 iOS 26.5 上打开后把消息外发"，该站明确标注无法独立验证[^web_search:454#17^]。

### 3.2 苹果的回应

- **【已证实】** 截至检索时苹果**没有任何公开回应**；Decrypt、heise、gbhackers 均确认苹果惯例是"补丁落地前不确认安全问题"[^web_search:482#0^][^web_search:454#6^][^web_search:482#22^]。下一个观察窗口是 iOS 27.1（Duo 预装版本）正式版与 27.2 的安全说明中是否出现致谢 Calif 的 CVE[^web_search:482#17^]。

---

## 4. 舆情中的事实增量（二次爆料，按可信度分级）

| 爆料 | 来源 | 可信度 |
|---|---|---|
| Duo 演示视频 6 秒、竖屏、CC @pwnlambda（Calif 成员/关联研究者） | fxtwitter 一手 JSON[^fetch_urls:455#1^] | **已证实** |
| 18 Pro 演示机=iPhone19,2 / T8160 / build 24A446 / SSH:2222 / uid=0 / 67 个 dpkg 包 / uptime 2 分钟 | Cyberpress、pasqualepillitteri、iClarified 多源一致[^web_search:482#11^][^web_search:492#4^][^web_search:504#11^] | **已证实**（多媒体交叉） |
| "2 分钟"是开机 uptime 而非 exploit 耗时 | pasqualepillitteri[^web_search:492#4^] | 已证实（澄清性事实） |
| 越狱链覆盖至 iOS 27.0.1、iPhone 18 Pro 全系 | heise[^web_search:454#6^] | 较高（单一权威来源） |
| 工具名 "Chollima"；装 Sileo+Gravity Lite；重启后锁屏 Calif logo/"From Calif with LOVE"；SPTM 绕过 | 微博长文[^web_search:504#4^]；pangu8 亦称 "Chollima Jailbreak"[^web_search:486#5^] | **中低**（pangu8 为已知低质量 SEO 站；微博长文有附图但单源） |
| AI 参与找链；可能用 27.2 beta 对比 diff | heise（后者 heise 自标为猜测）[^web_search:477#3^] | 部分已证实/部分**传闻** |
| CVE-2026-65410（AVEVideoEncoder）、CVE-2026-65409（Foundation）致谢 Calif×Claude×Anthropic | 苹果 iOS 27 安全说明，Decrypt/NeoTeo 转引[^web_search:482#14^][^web_search:482#20^] | 已证实，但**与越狱无官方关联** |
| 苹果已修补另一个 Cryptex/TSS 服务器端签名 bug（27.2 beta 3 半越狱路径），与 Calif 演示不是同一条链 | ONE Jailbreak[^web_search:482#14^] | 中 |
| iMessage 演示=恶意 PDF 外发消息（iOS 26.5） | glitchwire 引述与会者社媒 | **传闻，未证实** |
| 90 天披露时间表 | 仅为评论区提问，Calif 未答[^web_search:501#9^] | **无依据** |

---

## 5. 骗子动向

### 5.1 实证先例（用于定调）

- **nekoJB Online 骗局（已实证）**：Corellium Labs 完整解剖——假越狱全程 HTML/JS 演戏，诱导安装含 **root 证书的 .mobileconfig 描述文件**，实现 MITM、流量劫持与凭证窃取；通过 zJailbreak"付费会员制"敛财；**超 1 万台设备中招（含企业设备）**[^web_search:466#13^][^web_search:503#8^][^web_search:503#12^]。
- **Duo 相关骗局已有前科（已实证）**：9 月底 Malwarebytes 披露假冒"iPhone Duo 预购"钓鱼页，触发 **DarkSword** 漏洞链攻击未打补丁的旧 iPhone，窃取加密钱包、钥匙串、照片等；AARP、CNET 均发消费预警[^web_search:454#7^][^web_search:490#1^]。

### 5.2 当前 Duo 越狱骗局状态

- **【已证实】** 假冒/灰色越狱站**已把 iPhone Duo 列入"支持列表"**：palera1n.com（非官方域名）的兼容性页面已写入 "iPhone Duo"[^web_search:490#14^]；pangu8 上线 "iOS 27–27.2 Darksword 半越狱"在线流程（生成"专属 App"→装描述文件→装证书的 classic 骗局动线）[^web_search:486#5^]；zjailbreak 兜售 "Dopamine17 Online 无需电脑越狱"[^web_search:482#21^]。
- **【负面结果】** 截至 10-23 检索时，**尚未发现以"Duo 越狱工具"命名、仿冒 Calif 的新骗局被安全厂商实证的报道**；但按 nekoJB 模式（真越狱稀缺→骗局填充真空）与 Duo 预售钓鱼前科，出现高仿 "Chollima/DuoJB" 下载站只是时间问题。Corellium 已在借 Hexacon 热度预警："任何声称能越狱 iOS 27 的在线工具都应视为恶意软件"[^web_search:482#22^][^web_search:503#10^]。
- 社区自查机制：The Apple Wiki 诈骗越狱黑名单页持续更新[^web_search:466#11^]；applefirmwares.com 状态页明确 "iOS 26/27 无公开越狱，任何声称者皆为骗局"[^web_search:466#21^]。

---

## 6. 结论速览（分级）

**已证实**
1. Calif 于 Duo 发售日（10-23 10:27 UTC）官宣攻破 iPhone Duo，配 6 秒视频（79 万+观看），全程零技术细节、零工具、零 CVE[^fetch_urls:455#0^][^fetch_urls:455#1^]。
2. 此为 10-16 Hexacon iOS 27.0.1 untethered 越狱（iOS 9 以来首个 reboot 存活越狱）的延续营销；heise/Decrypt/Cyberpress 等多媒体证实演示真实性[^web_search:477#11^][^web_search:482#0^]。
3. 社区核心共识：**不会有终端用户工具**；Apple Wiki 状态页已标 Duo "No Jailbreak tool"[^web_search:490#19^]。
4. 苹果零回应（符合其一贯政策）[^web_search:482#0^]。
5. 越狱骗局生态已就位（nekoJB 实证、Duo 预售钓鱼实证、假站已列 Duo 支持），但仿冒 Duo 越狱工具的专项骗局**尚未见实证**。

**传闻**
- 工具名 "Chollima"、Sileo/Gravity Lite/锁屏 Calif logo 等细节（微博单源+低质站互引）。
- AI 用 27.2 beta diff 找漏洞（heise 自标猜测）。
- iMessage 恶意 PDF 演示（glitchwire 未验证）。

**观点**
- MIE 公信力受实质性质疑（5 月被绕、10 月连破两代硬件），但业界主流定性为"AI 加速的攻防常态"而非防线崩溃。
- "发售日攻破"更多是营销叙事——Duo 与 18 Pro 同 A20 Pro 芯片、同 iOS 27 代码基线，技术上大概率是同一链条的延伸（推断）。

**局限**：① Duo 推文发布仅数小时，主流英文媒体专题稿尚未落地，§1.1 为舆情真空期的负面结果；② r/jailbreak 与 heise.de 直连被环境审计拒绝，社区热帖为二手还原；③ 任务背景中 Stefan Esser "披露漏洞但不做工具"的表态未检索到独立出处，按背景给定采信。
