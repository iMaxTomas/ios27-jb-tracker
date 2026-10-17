# 《iOS 27.0.1 越狱传闻报告》事实核查审核报告

**审核日期：2026-10-17 | 方法：10 维度并行核查 + 交叉验证 | 被审对象：用户提供的 Hexacon 越狱事件报告**
# 第 1 章 审核概述与总体裁决

## 1.1 审核对象与背景

被审核报告围绕 2026-10-16 巴黎 Hexacon 大会（Synacktiv 主办）上的一次演示展开：安全公司 Calif 在 iPhone 18 Pro 上演示了 iOS 27.0.1 的 untethered（重启后越狱状态仍保留）越狱——重启、密码解锁后可通过 SSH 以 root（uid=0）登录，dpkg 显示已装 67 个软件包[^1^][^2^]。报告据此回答用户的实际问题：手持 iPhone 12 Pro Max 与 iPad mini 6（均运行 iOS 26.1），是否有必要升级 27.0.1。原报告的结论为"建议升级并锁定版本"，其论据包括"自 iOS 9 以来首个 untethered 越狱""iOS 27 砍掉 iPhone 11""苹果预计在下一个 27.0.x 或 27.1 修补漏洞"等。本章给出审核全貌，细节论证见第 2–6 章。

## 1.2 审核方法

本次审核将报告拆解为 10 个维度并行核查——事件真实性、技术旁证、历史断言、底层漏洞生态、越狱工具现状、芯片适用性、系统版本事实、功能与性能论据、修补节奏、决策框架——合计执行 150 余次独立检索。来源按三级使用：T1 为一手页面直接抓取（hexacon.fr 议程、calif.io 官网、apple.com 兼容列表与安全说明）；T2 为主流媒体（heise、Decrypt、WSJ 等）；T3 为社区来源（r/jailbreak、X 帖子，仅作旁证并标注性质）。每项断言按"属实 / 部分属实 / 无法证实 / 证伪"四级裁决并附置信度；跨维度冲突均在一手来源层面解决或明确标注为传闻后，方汇入总表。

## 1.3 总体裁决

**总体裁决：核心事件属实，升级建议的方向大体成立；但报告含 3 处事实错误（E1–E3）、6 处需修正（P1–P6）、3 处重大遗漏（O1–O3）。对用户设备而言最关键的发现是——"留在 26 等越狱"的收益为零。**

事件层面上，大会、Calif 公司背景与演示流程均经官方渠道与六家媒体交叉确认；最强旁证是演示中 XNU 内核 build 字符串与真实用户的 27.0.1 panic 日志逐字一致[^3^]。但须强调："演示属实"不等于漏洞公开，更不等于公开工具将发布。

| 类别 | 条目数 | 代表条目 |
|------|--------|----------|
| 核心事件属实（T1 级来源确认） | 演示流程、公司背景、技术细节多源一致 | Hexacon 演示全流程、Calif 公司背景及 Apple 官方致谢、iOS 27 无公开越狱工具 |
| 事实错误（证伪） | 3（E1–E3） | E1"自 iOS 9 首个 untethered"被 2021 年 Fugu14 证伪 [^4^]；E2"iOS 27 砍 iPhone 11"被官方兼容列表证伪 [^5^]；E3（部分证伪）实际修补通道为 27.0.2/27.2，锁版本窗口仅约 1–2 周 |
| 需修正（部分属实） | 6（P1–P6） | 性能数字均为"up to"营销上限；Siri AI 需 iPhone 15 Pro+ 且暂无中文；usbliter8 工具链现状描述已过时 |
| 重大遗漏 | 3（O1–O3） | O1 用户前提不成立：A14/A15 在任何 26.x 上均无越狱 [^6^]；O2 混淆"停在 26.1"与"留在 iOS 26 分支"：26.7.1 与 27.0.1 同日发布、分支仍在维护 [^7^]；O3 忽视升级不可逆性与执行窗口 |

**解读**：裁决分布揭示一个结构性特征——报告的事实层可信度高，错误与遗漏几乎全部集中于"适用性层"，即"这对用户的两台设备意味着什么"。此外一项不挂 O 编号的重要发现须单独提示：停在 26.1 意味着暴露于约 11 个月的补丁缺口，其中含 2 个在野 0day（已被实际利用的未修补漏洞）[^8^]。E2 与 O1 叠加直接抽掉了原论证的地基：iPhone 12 Pro Max（A14）既未被 iOS 27 淘汰，也无法在 iOS 26 的任何版本上越狱，"留在 26 保住越狱可能性"的收益是确定的零；而停在 26.1 是所有选项中最差的——既身处补丁缺口之内，又脱离 iOS 26 分支的后续维护（26.7.1 与 27.0.1 同日发布，分支仍在并行维护）[^7^]。因此审核后的建议方向与原报告一致（升级 27.0.1 并锁版本），但成立理由被整体替换，且行动窗口收窄至约 1–2 周。后续章节依次展开：第 2 章事件层核查、第 3 章错误证伪、第 4 章生态适用性、第 5 章决策审查、第 6 章结论与洞察。

---

# 第 2 章 事件层核查：Hexacon 越狱演示的真实性

本章只回答一个问题：**"2026 年 10 月 16 日，Calif 在 Hexacon 舞台上演示 iOS 27.0.1 完美越狱"这一事件是否真实发生**。核查方法为：优先抓取一手页面（hexacon.fr、calif.io、support.apple.com），辅以至少两家独立媒体的同日报道交叉印证；对无法直接抓取的来源（heise 原文）采用三方独立转述比对。结论先行：**事件层属实，且有多重独立旁证；但"演示属实"不等于"漏洞可用"，更不等于"工具将发布"**——这三层必须分开裁决。

## 2.1 大会与团队背景

Hexacon 是由法国攻击性安全公司 Synacktiv 支持的单轨安全技术大会，官网 CFP 页逐字确认时间为 2026 年 10 月 16–17 日、地点为巴黎 Palais Brongniart，定位为"focused on technical offensive security talks"[^9^][^10^]。演示方 Calif（Calif.io）是演讲当日开场 keynote 的署名单位，Hexacon 官方 X 账号当日发帖证实"A brand new iPhone jailbreak has just been unveiled on stage at Hexacon by the @calif_io team"[^1^]，现场观众、越狱研究者 Billy Ellis 发布的手拍视频为第二独立来源[^11^]。公司背景核查结果如下表。

| 核查项 | 证据 | 裁决 |
|---|---|---|
| 公司性质 | 官网自述"Red teaming and security engineering from the hackers trusted by OpenAI, Anthropic, Google, Cursor and xAI"[^12^]；招聘页列薪资/股权/福利 | 营利性安全公司，属实 [^12^][^13^] |
| 总部 | WSJ 称 "a Palo Alto-based security research company"[^14^]；NYT 同 | Palo Alto，属实 [^14^][^15^] |
| 创始人/主讲人 | Crunchbase：An Trinh、Thai Duong[^16^]；Thai Duong 为 BEAST/CRIME/POODLE 共同发现者、前 Google 安全研究员 | 属实 [^16^][^17^] |
| AI red teaming 业务 | 官网明示"We attack models, agents... Since 2023"及"AI-driven vulnerability research" | 属实 [^12^] |
| Apple 官方致谢记录 | Apple iOS 27 安全说明（2026-09-14）致谢 Calif.io 与 Claude、Anthropic Research 合作发现两枚漏洞（CVE-2026-65409 署名含 Bruce Dang，CVE-2026-65410），另有 CVE-2026-28968 致谢 Josh Maine of Calif.io | 属实（T1 一手）[^18^] |
| 既往 AI 辅助挖洞 | 4 月拆解 Coruna Safari 链[^19^]；5 月 M5 内核 exploit（CVE-2026-28952）[^20^][^21^]；8 月 Screen Sharing CVE-2026-65400 N-day 复现[^22^][^23^] | 属实（各有一手来源） |

该表的关键含义有二。其一，Calif 不是匿名论坛账号，而是一家被 Apple 官方安全说明逐字致谢过、客户名单包含多家一线 AI 厂商的实体公司——这大幅降低了"舞台造假"的先验概率，因为造假对其商业信誉的代价极高。其二，既往记录显示其工作模式为"AI 辅助的漏洞研究 + 向厂商私下报告 + 补丁后披露"，与本次"只演示、不放工具"的表现一致。需透明标注的瑕疵：个别低权威来源称其为 "Vietnam-based"，与 WSJ/NYT/Crunchbase 的主流证据矛盾，不予采信；其工商注册信息未在公开渠道定位，属小的未验证空白[^24^]。

## 2.2 演示技术细节逐条核验

技术细节最早由 CyberSecurityNews 对演示画面逐帧还原[^25^]，Cyberpress 独立复述同一组数据[^26^]，Decrypt 依据现场视频描述了完整流程[^27^]。逐条裁决如下。

| # | 断言 | 关键证据 | 裁决 |
|---|---|---|---|
| T1 | 机型 iPhone 18 Pro = iPhone19,2 | EveryMac/AppleDB/Geekbench 均将 iPhone19,2 对应 iPhone 18 Pro | 属实 [^28^][^29^][^30^] |
| T2 | 芯片 T8160 | 芯片数据库列"A20 Pro (Borneo, T8160)"；与 A19 Pro 的 T8150 编号惯例衔接 | 属实 [^31^][^32^][^33^] |
| T3 | 系统 iOS 27.0.1，build 24A446 | 维基版本表与 Apple releases feed 独立核对：27.0.1 = 24A446（2026-09-28） | 属实 [^34^][^35^] |
| T4 | 内核 Darwin 27.0.0 / XNU 13432.2.10 | 真实用户 panic 日志显示 iOS 27.0.1 (24A446) 内核为 xnu-13432.2.10~2，与演示 uname 输出逐字一致 | 属实（强旁证）[^3^][^36^][^37^] |
| T5 | 重启→密码解锁→SSH 2222 端口 root 登录（uid=0） | CyberSecurityNews、Cyberpress 逐字印证；Decrypt 对现场视频的描述独立印证 | 属实 [^25^][^26^][^27^] |
| T6 | dpkg 安装 67 个包、uptime 约 2 分钟 | 两家媒体一致记录 neofetch 输出 | 属实 [^25^][^26^] |
| T7 | "两分钟"指 exploit 耗时 | 原始报道均只说 uptime 约两分钟；pasqualepillitteri 明确澄清其为重启后开机时长，exploit 耗时未知 | **误读，证伪** [^38^][^39^] |
| T8 | 演示涉及绕过锁屏 | 手机用机主密码正常解锁，"the demo never touched the lock screen" | **证伪**（演示不涉及锁屏）[^38^][^26^] |

表中 T4 值得单独强调：XNU 13432.2.10 这一内核 build 号与两条独立来源的真实用户 panic 日志逐字一致：iPhone 15 的日志明确标注 os_version 为 27.0.1 (24A446)，iPhone 13 Pro 的日志内核号与之逐字一致[^3^][^36^]。伪造一段终端输出容易，但把尚未公开内核源码的具体 build 号编对极难——这构成演示真实性最有力的间接证据。同时 T7、T8 两条澄清了社交媒体上两类典型误读："两分钟拿到 root"实为重启后的 uptime（neofetch 显示的已开机时长）而非漏洞利用耗时[^38^]；"重启后仍越狱"也不意味着能绕过锁屏——演示全程使用机主密码，不构成任何远程或零点击攻击能力的证据[^26^]。untethered（重启后越狱状态仍然保留，无需重新连接电脑执行利用）一词亦应据此理解：它描述的是持久性，不是攻击入口。

## 2.3 边界与未知：三层命题的区分

事件层属实之后，必须明确哪些命题**不**随之成立。第一，漏洞层面：所利用的漏洞链未披露、无 CVE 编号、无公开工具，untethered 等核心技术声明仅有舞台演示与现场视频为证，未经任何第三方独立复现[^26^][^40^][^41^]。第二，发布层面：Calif 官方 X 账号与 newsletter 当天的原话是"Everything will be released in due time"[^42^][^43^]——这是一句无时间表、无范围承诺的措辞，结合其既往"补丁后才发 crash-only PoC"的披露模式，不能据此推断越狱工具将对公众可用。第三，适用范围：heise 称该链适用于"直至 iOS 27.0.1 及 iPhone 18 Pro 系列"，但能否移植到 A14/A15 等更早芯片"nobody knows"，因各代硬件防护不同[^38^]。此外，"AI 帮助发现漏洞链"目前仅有 heise 一家归因，且 heise 自己未说明 AI 的具体角色；"对比 27.2 beta 与 27.0.1"找补丁差异的说法更是 heise 的明示猜测（"That is speculation, not a finding"）[^41^]。方法透明度声明：heise 原文页面在本核查环境中无法直接抓取，其观点均经 explainx 与 pasqualepillitteri 两家独立转述、加搜索引擎索引中的 heise 原文片段三方交叉确认，内容一致，不影响上述裁决[^44^][^2^]。

---

# 第 3 章 证伪与修正：原报告的事实错误

上一章已确认：Hexacon 演示这一事件层属实。但事件属实不等于转述无误。本章对原报告的逐条断言做裁决，将其分为两类：E1–E3 为可被一手来源直接推翻的**事实错误**，P1–P6 为方向大体成立但需限定条件的**修正项**。裁决方法为：每条断言回溯至最高层级来源（Apple 官方页面、会议官方资料、法院文书为 T1；heise、Decrypt、MacRumors 等主流媒体为 T2；社区来源为 T3 并标注性质），至少两个独立来源交叉确认后方可判定证伪。

## 3.0 事实错误裁决总表

| 编号 | 原报告断言 | 裁决 | 关键反证 |
|------|-----------|------|----------|
| E1 | "自 iOS 9 以来首个重启级（untethered）越狱" | 证伪 | Fugu14（2021-10-24，iOS 14.3–14.5.1，开源）被 POC2022 官方简介定义为"the first publicly available untethered jailbreak since iOS 9" [^45^][^4^] |
| E2 | "iOS 27 砍掉 iPhone 11，仅支持 iPhone 12+" | 证伪 | Apple 官方 iOS 27 页面兼容列表与 iOS 26 完全相同，iPhone 11 全系在列 [^5^][^7^] |
| E3 | "预计苹果在下一个 27.0.x 或 27.1 修补" | 部分证伪 | 普通 iPhone 无 27.1（iPhone Duo 折叠屏独占）；实际通道为 27.0.2 或 27.2 [^46^][^47^] |

三条错误中，E1、E2 属"可核查事实被写错"，E3 属"推断依据的版本通道事实错误"。三者性质不同但后果一致：它们都是原报告后续论证链的承重节点——E1 支撑"历史性稀缺"的叙事，E2 支撑"12 系列已在淘汰边缘"的升级紧迫性，E3 支撑"锁 27.0.1 等越狱"的时间窗口估计。修正这三处后，原报告的升级建议需要重新论证，而非仅改措辞。以下逐条说明验证过程。

## 3.1 E1："自 iOS 9 以来首个 untethered"——Fugu14 反例与 heise 的源头错误

原报告这一断言照录自 heise 的强表述。heise 德文导语逐字为"Das Team Calif.io hat den ersten rebootfesten Jailbreak seit iOS 9 entwickelt"，英文版同义："the first reboot-proof jailbreak since iOS 9"[^48^][^2^]。该断言的可证伪性很高，且确有反例：Linus Henze 的 **Fugu14** 于 2021-10-24 公开开源发布，覆盖 iOS 14.3–14.5.1、arm64e 设备（A12–A14，含当时的旗舰 iPhone 12），属 untethered（重启后越狱状态自动保留）[^45^]；POC2022 会议官方讲者简介将其明确定性为"the first publicly available untethered jailbreak since iOS 9"[^4^]，unc0ver v7.0.0 同日集成 Fugu14 并经 AltStore 提供一键安装，可用性无争议[^49^][^50^]。heise 的错误在于把"会议演示"与"公开发布的工具"混用同一把历史标尺：若按"演示"计，Fugu14 当年是先公开演示后发布工具；若按"公开工具"计，Calif 本次尚无工具，反而不如 Fugu14"公开"。因此严谨表述应为：**这可能是首个针对当前最新正式版系统与在售最新硬件的 untethered 越狱演示**——加上"最新正式版系统"与"演示"双重限定后方才成立。准确的史实是：2016 年 7 月后公开越狱几乎全为 semi-untethered 或 semi-tethered，2021 年 Fugu14 是唯一公开的 untethered 例外，其后至本次演示前再无公开 untethered[^51^]。

## 3.2 E2："iOS 27 砍 iPhone 11"——官方列表直接证伪，论证链断裂

Apple 官网 iOS 27 页面（2026-10-17 抓取）逐字列出兼容设备含 "iPhone 11 Pro Max, iPhone 11 Pro, iPhone 11, iPhone SE (2nd generation and later)"，Apple 安全发布页对 iOS 27 的适用设备标注 "iPhone 11 and later"[^5^][^52^]；MacRumors 明确写道 "no iPhones have been dropped this year"[^7^]，The Verge 发布日报道同口径[^53^]。被砍的是 iPhone XS/XR，且发生在 iOS 26 一代，与 iOS 27 无关。此错误直接抽掉原报告两段推论的地基：其一，"11 被砍说明 12 已在淘汰边缘"的前提不存在——按苹果 5–7 年大版本支持惯例，iPhone 12 距支持底线尚隔两代机型；其二，"留在 26 等于提前进维护期"亦不成立——Apple 同日（2026-09-28）发布 iOS 26.7.1 与 27.0.1，26 分支仍在并行维护并获得与 27 大部分同源的修复[^52^][^52^]。"淘汰边缘"的方向性担忧可作为推断保留，但不得再以"iOS 27 砍 11"为论据。

## 3.3 E3：修补通道判断错误——普通 iPhone 无 27.1

原报告称苹果"预计于下一个 27.0.x 或 27.1 修补"。核查显示 iOS 27.1 是 iPhone Duo 折叠屏专属版本，"that version is being skipped for the non-foldable iPhone models"[^47^]；iPhone Duo 于 2026-10-23 发售并搭载 27.1，Xcode 27.1 RC 已于 10 月 5 日发布[^46^]。普通 iPhone 的实际修补通道是 27.0.2 或 iOS 27.2——后者 beta 1 于 9 月 16 日发布，截至 10 月 13 日已至 beta 4（build 24B5110a），预计 10 月下旬转正[^34^][^54^]。此修正有直接的决策含义：原报告隐含的"锁 27.0.1 等公开越狱"窗口被压缩至约 1–2 周；且由于 Calif 未披露漏洞、无 CVE，苹果能否在下一版本精准修补本身存疑，修补时间属推测而非确定预期[^41^]。

## 3.4 P1–P6 需修正项

| 编号 | 原报告断言 | 裁决 | 修正要点 |
|------|-----------|------|----------|
| P1 | "启动快 30%/照片快 70%/AirDrop 快 80%" | 需修正 | 三个数字均为 "up to" 营销上限，且分测三台不同设备（30% 测自 iPhone 11 Pro Max、70% 测自 iPhone 15、80% 测自 iPhone 16 Plus）；Tom's Guide 实测 AirDrop 仅约 5% [^55^][^56^] |
| P2 | "Siri AI + 跨 App + Call Context" | 需修正 | 功能真实，但英文 Beta 首发、需 iPhone 15 Pro+、国行首发无 AI、暂无中文——对用户两台设备完全不适用 [^57^][^58^][^59^] |
| P3 | usbliter8"无公开越狱工具" | 已过时 | 2026-07 起社区 usbliter8-fun 实现 iPhone 11 Pro 上 iOS 27.0b2 tethered 研究级越狱（破坏性）；另原始 write-up 已被美法院命令下架（Magnet 诉 Del Gaudio 案）[^60^][^61^][^62^] |
| P4 | "Calif 链大概率需适配 A14/A15" | 方向合理的推断 | 属推断而非事实；且 A14（12 Pro Max）缺 PPL bypass、A15（mini 6）缺 SPTM bypass，两者所需绕过不同，不能笼统并述 |
| P5 | "大会自称 Mythos era" | 措辞需限定 | 精确措辞仅见单一二手来源；官网 keynote 确以 Mythos 模型为核心，方向正确但不宜加引号当作原话 |
| P6 | "链价值极高，Calif 无放出动机" | 前提属实、结论应为不确定 | 市场价格属实（Zerodium $2M+、Crowdfense 最高 $7–9M），但存在 "in due time" 二手反向信号，强暗示"永远不公开"越界 [^63^][^64^][^39^] |

六项修正的共同模式是：原报告把"官方营销上限""功能存在""合理推断"写成了"对用户设备的确定承诺"。P1 中值得为原报告说明的是，30% 的基准测试机型恰为比 A14 更老的 iPhone 11 Pro Max，苹果亦将 CPU 调度器改进回溯至 iPhone 11，故"老机型受益方向真实"成立；但无任何一项官方测试在 A14 上进行，且独立复测显示实际收益依场景可低至个位数百分比，报告不得将上限数字表述为 iPhone 12 Pro Max 的保证值[^55^][^56^][^65^]。P2 的限制是硬件、语言、地区三重叠加：Siri AI 以英文 Beta 首发，需 A17 Pro/8GB 内存起（iPhone 15 Pro 及更新），中文不在 10 月新增语言名单，中国大陆首发无新一代 AI——对 iPhone 12 Pro Max 与 iPad mini 6（iPad 侧门槛为 M1+ 或 A17 Pro mini）完全不适用[^57^][^59^][^66^][^67^]。P3 的过时源于生态快速演进：usbliter8-fun 虽为破坏性研究项目而非日用工具，但"无公开越狱"的字面表述已不成立；同时 Magnet Forensics 诉前工程师 Del Gaudio 案中，联邦法官已于 2026-07-21 签发禁令要求删除原始 write-up 与 PoC，原始材料现仅能经存档访问，案件尚无终局裁决[^68^][^69^]。P4、P5、P6 则是证据层级问题：三者方向均可保留，但 P4 应明确标注为推断（且 A14 属 PPL 保护设备、A15 属 SPTM 保护设备，移植障碍各异），P5 应去掉直接引语式的归属，P6 应把结论从"经常永远不公开"的强暗示降级为"不确定"——Calif 的既往模式（私下报告 + 补丁后发布 crash-only PoC）与数百万美元的市场价格支持"缺乏免费放出动机"，但"will be released in due time"这一被多方记录的表态构成必须披露的反向证据[^70^][^14^][^40^][^41^]。

---

# 第 4 章 越狱生态与用户设备适用性

比第 3 章裁决的事实错误更影响决策的是一个前提错配：用户的整个"留在旧版等越狱"框架建立在"iOS 26.0.1 已经可以越狱"之上。本章核查该前提对用户两台设备——iPhone 12 Pro Max（A14，iOS 26.1）与 iPad mini 6（A15，iPadOS 26.1）——是否成立。核查方法为交叉比对 Dopamine 官方仓库与官网、theapplewiki 兼容表、iClarified 兼容矩阵、Apple 官方安全公告及多家媒体综述，核心结论获五方独立印证，置信度为高。

## 4.1 iOS 26.x 越狱现状：仅限 A12/A13 的"局部可越"

2026 年 8 月 7 日，opa334（Lars Fröder）发布 Dopamine 3.0，首次将 iOS 26.0–26.0.1 纳入支持范围，但**仅限 A12/A12X/A12Z/A13 芯片**：theapplewiki 逐字记载"Support for iOS 16.6-18.7.1 and iOS 26.0-26.0.1 was unexpectedly added for A12/A13 devices, thanks to the release of Momentarius"[^71^]；Dopamine GitHub 官方仓库描述为"iOS 15.0 - 18.7.1, 26.0 - 26.0.1 (A12/A13)"[^72^]。需要修正一处常见的清单误导：iPhone XS/XR 虽为 A12，但苹果官方 iOS 26 支持列表为"iPhone 11 and later"，XS/XR 根本无法运行 iOS 26[^52^]——因此 iPhone 侧实际受益者只有 A13 机型（iPhone 11 系列与 SE 第 2 代），A12 的支持落在可运行 iPadOS 26 的 iPad 第 8 代、Air 3、mini 5 上[^73^][^74^]。

iOS 26.1 正式版（2025-11-03 发布）起则**全芯片无工具**：theapplewiki《Jailbreak/26.x》表中 26.1 及之后的 26.2、26.2.1 均标注"No Tool Available"[^6^][^6^]。唯一例外是 Dopamine 3.0.5 曾支持 26.1 beta 1–3（同样仅限 A12/A13），正式版不在其列[^75^]。第三方综述结论一致："There is no public jailbreak for iOS 26.1 or later, or for iPhone 12 and newer models on any iOS 26 build."[^73^]

"新旧倒挂"——老芯片反而领先——的机制有两层，均与 SecureROM（BootROM）漏洞 usbliter8 无直接因果关系。其一，Dopamine 3.0 走的是纯软件链：ClearSword/DarkSword 内核漏洞 + momentarius PPL 绕过，而 momentarius（PPL，Page Protection Layer，页保护层）只做了 A12/A13 版本[^71^][^76^]。其二，A15 及更新芯片在 iOS 17 起以 SPTM（Secure Page Table Monitor，安全页表监视器）取代 PPL，公开界在 ≥17.4 没有任何 SPTM 绕过，Dopamine 发布说明原文即"obviously only works up to 17.3.1 on real devices due to lack of an SPTM bypass on >=17.4"[^76^]。至于 2026 年 6 月公开的 BootROM 漏洞 usbliter8，它利用 Synopsys DWC2 USB 控制器的硬件缺陷，同样只覆盖 A12/A13——技术媒体（Infosecurity Magazine）转述 Paradigm Shift 的分析明确指出"A14 and later chips appear to configure DART correctly in SecureROM, making the same route unexploitable"[^77^]。两个"恰好只覆盖 A12/A13"的巧合叠加，造就了 A14+ 用户被排除在外的窗口期。

## 4.2 关键错配：用户两台设备在任何 26.x 上均无越狱

Dopamine 官网将支持范围写得泾渭分明："Supported on iOS 15.0 - 17.3.1 (A14 - A17 / M1 - M2)"——A14/A15 的公开越狱上限是 iOS 17.3.1，不含任何 iOS 18/26 版本[^78^]。iClarified 兼容矩阵逐字印证："iPhone 12 lineup | A14 | iOS 17.3.1 (Dopamine)"、"iPhone 13 lineup / SE (3rd Gen) | A15 | iOS 17.3.1 (Dopamine)"[^74^]；theapplewiki 26.x 表中 iPad mini 6 在 26.0/26.0.1 两栏均为"No"[^6^]。

| 设备（芯片） | 26.0 | 26.0.1 | 26.1（当前版本） | 27.0.x | 最新可用工具/版本 |
|---|---|---|---|---|---|
| iPhone 12 Pro Max（A14） | 无工具 [^78^] | 无工具 [^78^] | 无工具 [^73^] | 无工具 [^73^] | Dopamine，上限 iOS 17.3.1 [^74^] |
| iPad mini 6（A15） | 无工具（wiki 标 "No"）[^6^] | 无工具（wiki 标 "No"）[^6^] | 无工具 [^6^] | 无工具 [^73^] | Dopamine，上限 iPadOS 17.3.1 [^74^] |
| 参照：iPhone 11 系列/SE2（A13） | Dopamine 3.0 [^71^] | Dopamine 3.0 [^71^] | 无工具（仅 beta 1–3）[^75^] | 无工具 [^73^] | Dopamine 3.0.11，上限 26.0.1 [^6^] |

解读：矩阵揭示的错配是双重的。第一重是**芯片错配**——"26.0.1 已可越狱"只有加上"仅限 A12/A13"才成立，用户的 A14/A15 恰好落在被排除的第一代与第二代芯片上；参照行显示 A13 设备在 26.0/26.0.1 上的"Yes"与用户设备整行的"无工具"形成鲜明对照。第二重是**版本错配**——即便假设芯片不是障碍，用户当前所在的 26.1 正式版本身对任何芯片都没有工具，Dopamine 对 26.1 的支持止步于 beta 阶段。换言之，"留在 26.1 等越狱"等待的是一个在任何时间截面都不存在的工具组合。

"降回 26.0.1 等越狱"也不可行：苹果已于 2025 年 11 月停止签署 iOS 26.0.1，降级通道已关闭（ipsw.me 可查当前在签版本），26.0.1 标记为 Unsigned[^79^][^80^][^81^]；且 A12 及更新设备的 SHSH blob（苹果的固件签名凭证）备份机制无法用于恢复降级。更根本的是，即便降级成功，A14/A15 在 26.0.1 上依然无越狱——两条路同时封死。

## 4.3 A14 与 A15 的防御差异：不能一概而论的两代设备

用户两台设备虽同属"无越狱"阵营，但在页表防护上分属两代，这一差异决定了未来任何越狱链在它们身上需要补齐的环节不同。术语说明：PPL 是苹果 2017 年引入的内核页表写保护层；SPTM 是其硬件继任者，结合 TXM 提供更小攻击面；MIE（Memory Integrity Enforcement，内存完整性强制）是 2025 年随 A19/A19 Pro 与 M5 首发的硬件内存安全体系[^82^][^83^][^84^]。

| 维度 | iPhone 12 Pro Max（A14） | iPad mini 6（A15） |
|---|---|---|
| PAC（指针认证） | 有（A12 起全系具备）[^85^] | 有 [^85^] |
| 页表防护 | PPL（无 SPTM 硬件）[^86^] | SPTM/TXM（iOS 17 起取代 PPL）[^82^][^87^] |
| MIE | 无（MIE 始于 A19/M5）[^84^][^88^] | 无 [^84^][^88^] |
| usbliter8 适用性 | 不适用（SecureROM 正确配置 DART）[^77^] | 不适用 [^77^] |
| Dopamine 公开上限 | iOS 17.3.1 [^78^][^74^] | iPadOS 17.3.1 [^78^][^74^] |
| iOS 27 上缺失的 bypass | PPL bypass（momentarius 不含 A14）[^71^] | SPTM bypass（公开界 ≥17.4 空白）[^71^] |
| 升级 27 支持 | 支持（无 Apple Intelligence）[^89^][^90^] | 支持（无 Apple Intelligence）[^91^][^67^] |

解读：Apple Platform Security 官方文档明确"SPTM … replaces Page Protection Layer"，硬件上仅 A15/M2 及更新 SoC 支持[^82^][^92^]，学术分析同样确认"SPTM has replaced the PPL mitigation on A15 / M2 or newer SoCs"[^86^]——因此 A14 是"最后的纯 PPL 旗舰"之一，A15 则是第一代 SPTM 设备。这张表的核心信息是：两台设备的"无越狱"虽结论相同，但技术成因各异，未来若出现新链，A14 需要的是 iOS 27 上的 PPL bypass（momentarius 未覆盖 A14，官方未解释原因，推断与 17.4+ 的 PPL 静默加固有关[^93^]），A15 需要的是公开界完全不存在的 SPTM bypass——后者是更硬的墙。同时两台设备均无 MIE，这在移植评估中是一个被低估的有利因素。

关于 Calif 在 Hexacon 2026 演示的 iPhone 18 Pro（T8160/A20 Pro）iOS 27.0.1 untethered 链能否惠及这两台设备，本章给出平衡评估（**标注：推断**，因 Calif 未披露任何漏洞细节[^25^][^41^]）。有利面：A14/A15 无 MIE，若 Calif 链在 A20 Pro 上包含 MIE 对抗环节，该环节在旧机上直接不需要，内存破坏原语可靠性反而更高；且纯软件链跨代移植是历史常态（Fugu14/Fugu15 均覆盖多代芯片）[^94^][^95^]。不利面：移植必须补齐上表所列的两块 bypass 空白，而 Calif 的 SPTM 手段若按系统版本实现则可能直接下移至 A15，若依赖 T8160 特有行为则无法下移——最大不确定性在 untether 持久化机制[^41^]。可证实的结论只有：适配性未知，但"大概率不适配"的绝对化判断与历史基线相悖。

## 4.4 假越狱骗局与 Cellebrite 声明的被挑战

在"无公开工具"的真空期，骗局趁虚而入。安全厂商 Corellium 对自称支持"iOS 18.7–26"的"nekoJB Online"做了完整实验剖析，结论逐字为"nekoJB Online isn't a jailbreak. It's a scam. A trap designed to harvest data and exploit trust."[^96^]——其手法是诱导安装 .mobileconfig 恶意配置描述文件、web clip 与根证书，实现流量拦截与凭据窃取[^97^]。鉴于截至核查日不存在任何公开 iOS 27 越狱工具，凡声称"下载即越狱 iOS 26.1/27"的网站或 App 均可按骗局对待[^98^][^39^]。

另一端，取证巨头 Cellebrite 在 2026 年 9 月 28 日的博客中断言"iOS 27 cannot be jailbroken on any current physical device and there is no reliable public jailbreak for anything above iOS 18"[^99^]；约 2.5 周后 Calif 的现场演示直接推翻了前半句，GBHackers 明确写道"This demo challenges that claim"[^40^]。需作两点保留：其一，Cellebrite 该文本质是 Corellium 虚拟化产品的营销文，"越狱已死"的论断存在商业动机[^99^]；其二，被推翻的只是"物理不可越"，"无可靠**公开**越狱"在字面上仍然成立——Calif 未发布任何工具，而这才是与用户设备直接相关的状态。

---

# 第 5 章 升级决策审查与修正版建议

## 5.1 原报告建议的逻辑漏洞

公平地说，原报告的结论方向——不建议停在 26.1、想留后路就升 27.0.1 锁版本——在审核后依然成立。但它抵达这个结论的路径经不起核查：三条核心论据（Calif 链理论覆盖不丢失、留在 26 等于提前进维护期、苹果预计在 27.0.x 或 27.1 修补）均含事实错误，属于典型的"结论对、论证错"。以下三处漏洞直接改变决策结构。

**漏洞一（O1）：未纠正用户的错误前提，"留在 26 保越狱"对这两台设备是伪选项。** 用户以"旧的 26.0.1 已经可以越狱"为决策前提，原报告通篇顺着此前提展开。事实是：Dopamine 3.0 的 iOS 26.0/26.0.1 越狱仅限 A12/A13 芯片，A14–A17 设备的公开越狱上限仍停留在 iOS 17.3.1，"iPhone 12 及更新机型在任何 iOS 26 build 上均无公开越狱"[^73^][^100^]。iPadOS 侧 Dopamine 对 A15 的上限同为 17.3.1。即用户两台设备在 26.x 任何版本（含 26.0.1、26.1、26.7.1）都越不了狱——"留在 26"的越狱收益是确定的 0，而非原报告暗示的"保留一项已有能力"。硬件层面同样无保底：SecureROM（固化于芯片、无法通过系统更新修补的启动只读代码）漏洞 usbliter8 官方确认 A14 及更新芯片不受影响[^101^][^102^]；中文社区"A14/M1 同样受影响"的说法与官方 PoC 范围矛盾，属未证实传闻[^103^]。

**漏洞二（O2）：混淆"停在 26.1"与"留在 iOS 26 分支"。** 原报告称"留在 26 同样收不到修复、等于提前进维护期"，已被证伪：2026-09-28 苹果与 iOS 27.0.1 **同日**发布 iOS/iPadOS 26.7.1，修复在野 0day CVE-2026-86950（CoreGraphics 越界写，可致任意代码执行），CISA 于次日将其列入 KEV 目录（Known Exploited Vulnerabilities，美国网络安全局维护的"已确认在野利用"漏洞清单）[^104^][^105^][^8^]。媒体直言 26.7.x 就是"为拒绝升 27 的用户保持安全"的平行分支[^106^]。因此真实的安全选项是三选——停在 26.1、跟进 26.7.1、升 27.0.1——而非原报告的升/不升二分；其中"停在 26.1"这个具体版本才是真正收不到修复的那个。

**漏洞三（O3）：未讲清升级的不可逆性与执行窗口。** 对 A12 及更新设备，SHSH blob（苹果签名凭证，旧设备可借此刷回旧版）实质上无效，恢复到苹果当前签署版本以外的系统不受支持[^107^]；苹果已于 10 月初停止签署 iOS 27.0 与 26.7[^106^]，"已升 27 者无法再降回 26"[^105^]。升 27.0.1 是单向门——这不是反对升级的理由，但任何负责任的建议都必须明示。同时，普通 iPhone 没有 27.1（该版本为 iPhone Duo 折叠屏独占，10 月 23 日随机器上市），下一版本大概率是 27.2，预计 10 月下旬发布，其后苹果通常一两周内关闭 27.0.1 签名[^108^][^109^]。原报告"预计下一个 27.0.x 或 27.1 修补"的表述在版本通道上是错的，且完全遗漏了"锁 27.0.1 的窗口就在现在到 27.2 发布前后"这一时间敏感性。补救通道存在但有限：DelayOTA（延迟 OTA，利用企业设备延迟升级机制）可在关签后约 90 天内定点升到已关签版本[^110^]，可作备选但不宜全押。

## 5.2 安全账本：26.1 的 11 个月补丁缺口

原报告将锁版本的风险表述为将来时（"承担未打补丁的风险"），却忽略了用户此刻停在 26.1（2025-11-03 发布）已实际承担约 11 个月的补丁缺口[^111^]。下表为该缺口的构成：

| 缺失项 | 内容 | 严重程度 | 来源 |
|---|---|---|---|
| iOS 26.2–26.6.1 累积修复 | 仅 26.6.1 一个版本即修复 28+1 个漏洞 | 高 | 属实 [^112^] |
| iOS 26.7 | 80+ 项修复，其中 75 项与 iOS 27 共享 | 高 | 属实 [^113^] |
| CVE-2026-20700（dyld） | 2026 年 2 月修复，已确认在野利用 | 在野 0day | 属实 [^114^] |
| CVE-2026-86950（CoreGraphics） | 2026-09-28 随 26.7.1 修复；苹果确认"可能已被用于针对特定个人的高度复杂攻击"，影响 iOS 27 之前所有版本；CISA KEV 在列 | 在野 0day（KEV） | 属实 [^104^][^8^][^115^] |
| Coruna 漏洞套件相关修复 | 4 个被利用漏洞的修复 | 在野利用 | 属实 [^115^] |
| iOS 27 独有修复 | iOS 27 共 126 个 CVE（含约 20 个内核级条目），扣除与 26.7 共享的 75 项后约 50 项独有 | 中–高 | 属实 [^18^][^116^] |

需要澄清一点口径：两个在野 0day 均修在 26 分支（CVE-2026-20700 于 2026 年 2 月的 26.x 版本、CVE-2026-86950 于 26.7.1），而非 iOS 27 独有修复——iOS 27 发布时苹果未提及任何漏洞已在野利用[^112^]，27.0.1 本身更无公开 CVE 条目[^52^]。但这不改变结论的实质：无论走哪条路径（升 26.7.1 或升 27.0.1），用户都能补上缺口；唯一补不上缺口的动作就是"什么都不做"。升级是**降低**风险而非增加风险，原报告的时态错误把一笔正在亏损的资产说成了未来才可能发生的成本。

## 5.3 越狱期权的理性定价与修正版决策树

既然"留在 26"的收益为零，唯一需要认真定价的问题是：为"锁 27.0.1"这份越狱期权值得付出什么？证据显示这份期权价值很低：A14/A15 的公开越狱上限停在 iOS 17.3.1、17.4 起无任何公开越狱，已停滞近三年[^73^]；iOS 27 上 A14 缺公开 PPL（Page Protection Layer，页保护层，A14 及更早设备的内核页表防护机制）绕过、A15 缺 SPTM（Secure Page Table Monitor，安全页表监视器，A15/M2 起取代 PPL）绕过，两个环节公开界均为空白[^117^][^71^]；Calif 链无公开工具、无发布时间表、无 A14/A15 覆盖证据，且其历史模式是私下报告厂商、补丁后发布非武器化分析——漏洞市场报价与放出动机一项论据经第 3 章修正后方向不变（详见 3.4 的 P6），免费放出可用工具的经济动机薄弱[^73^][^100^][^25^][^27^][^40^]。理性策略是把"锁在 27.0.1"当作一份**免费持有的低概率期权**——它不要求你牺牲安全（升级本身就补齐了补丁），但也不值得为它放弃安全更新节奏之外的任何东西。

```
Q0  你的设备现在能越狱吗？
    └─ 不能。A14/A15 在任何 26.x 均无公开工具（上限 17.3.1）。
       "停在 26.1 保越狱"是伪选项，且 26.1 已暴露于 KEV 级在野 0day。

Q1  你的首要目标是什么？
    ├─ A. 安全与稳定优先（推荐大多数用户）
    │     → 直接升 27.0.1（OTA 界面当前同时提供 27.0.1 与 26.7.1 两个选项 [^118^]）；
    │       若对 27 初期口碑存疑，至少先升 26.7.1 补上在野 0day，观望 2–4 周。
    │
    └─ B. 想给未来越狱留最大概率，同时不牺牲安全
          → 在 iOS 27.2 发布前（预计 10 月下旬，窗口约 1–2 周）：
            ① 两台设备完整备份；② OTA 选择 27.0.1；
            ③ 关闭自动更新，锁定该版本；
            ④ 跟踪 opa334/Dopamine、AppleDB、Calif 披露动态及苹果安全发布说明
               （若 27.x 安全说明致谢 Calif 且涉及内核/沙盒/PPL/SPTM 级组件，才说明链条已被修补，
               期权价值归零需重估；注意 Calif 此前已因 DoS 类 bug 获得过致谢，须看条目内容而非署名）。
          → 退路认知：升级不可逆（SHSH 无效、旧版已关签）；
            DelayOTA 提供关签后约 90 天的定点升级缓冲，可作备选但勿全押。

两路都不包含"停在 26.1"——它是所有选项中唯一确定最差的一个。
iPad mini 6 与 iPhone 12 Pro Max 适用同一决策树
（iPadOS 27.0.1/26.7.1 同步发布；Dopamine 对 A15 上限同为 17.3.1）。
```

体验账本对两路均不构成阻碍，方向反而支持升级：性能论据经第 3 章修正后结论不变——iOS 27 的老机型优化真实存在，但相关性能数字为"up to"营销上限而非 12 Pro Max 的保证值（详见 3.4 的 P1）[^55^][^56^][^65^]；iOS 26 的大规模发热/耗电投诉在 26.1 并未根治（明显好转要等到 26.2 前后），继续停留反而延续体验问题[^119^][^120^][^121^]。需提示 iPad 侧：mini 6 可升 iPadOS 27，但无 Apple Intelligence（iPad 侧门槛为 M1 及更新或 iPad mini A17 Pro），升级获得的是系统功能与安全更新，不含 AI 层[^66^][^67^][^122^]。

## 5.4 原报告建议与审核后建议对照

| 原报告建议 | 裁定 | 审核后修正 |
|---|---|---|
| ① 体验值得升；"11 被砍说明 12 在淘汰边缘" | 方向可取、论据证伪 | 升级理由成立，但 iPhone 11 未被砍、12 系列不在淘汰边缘；真正的理由是安全补丁缺口与功能周期 |
| ② "Calif 链覆盖 27.0.1，升级不丢理论覆盖；留在 26 同样收不到修复" | 事实错误＋推断过度 | Calif 仅有演示、无工具、无 A14/A15 证据；26 分支仍在维护（26.7.1 同日发布）；正确理由是"26.1 本来就一无所有，升级在越狱维度只赚不赔" |
| ③ 为越狱留后路就升 27 锁版本 | 方向正确、论证不全 | 可采纳，但须补充：执行窗口在 27.2 发布前（约 1–2 周）、升级不可逆、锁版本只是持有一份低概率期权 |
| ④ 保持 27.0.1 或更低、关自动更新、"承担安全风险" | 表述误导 | 锁版本的风险远低于现状：停在 26.1 此刻就暴露在 KEV 级在野 0day 下，升级本身是降风险 |

对照表揭示了原报告建议部分的整体形态：四条建议的结论方向有三条可以保留，但没有一条的论证能原样成立。对读者而言，最重要的修正不是"升不升"——答案依然是升——而是"为什么升"和"什么时候升"：理由是 11 个月的安全补丁缺口而非设备淘汰焦虑，时机是 27.2 发布前的签名窗口而非任意时间。

## 5.5 行动清单

1. **本周内（10 月 17 日起，27.2 发布前）**：两台设备分别完成完整备份（iCloud 或电脑本地加密备份）。
2. **iPhone 12 Pro Max**：OTA 升级至 27.0.1（设置→通用→软件更新，当前可自由选择 27.0.1 或 26.7.1）[^118^]；安全优先型用户升级后即可正常使用，想留越狱期权者升级后立即关闭自动更新。
3. **iPad mini 6**：同步执行，升级 iPadOS 27.0.1，决策逻辑与 iPhone 完全一致；不要期待 Apple Intelligence 功能[^67^][^122^]。
4. **时间窗口与不可逆性提示**：27.0.1 的签名预计随 27.2 发布（10 月下旬）后一两周内关闭，错过则"锁 27.0.1"选项消失，仅剩 DelayOTA 约 90 天的补救通道[^110^][^109^]；升级后无法降级（A12+ 设备 SHSH blob 无效；27.0/26.7 已关签，且 26.7.1 的 OTA 通道亦可随时关闭——升上 27 即不可回退），此为单向门，执行前请确认已知情[^105^][^107^]。
5. **持续跟踪**：关注苹果安全发布页（若 27.x 安全说明致谢 Calif 且涉及内核/沙盒/PPL/SPTM 级组件，才说明链条已被修补；注意 Calif 此前已因 DoS 类 bug 获得过致谢，须看条目内容而非署名）、Dopamine/opa334 与 AppleDB 动态；切勿安装任何声称可越狱 iOS 27 的网站工具或描述文件——此类工具已被实证为恶意软件陷阱。

---

# 第 6 章 结论：审核后的三句话与跨维度洞察

## 6.1 给用户的三句话

**第一句：事件是真的，报告的事实核查基本功在线。** Calif 在 Hexacon 演示的 iPhone 18 Pro / iOS 27.0.1 untethered（重启后仍有效）越狱，经主办方议程、六家媒体、Calif 一手帖文与真实内核 panic 日志交叉确认[^27^][^44^][^42^][^43^]——但"演示属实"不等于"漏洞公开"，更不等于"工具将发布"。

**第二句：报告的升级结论方向没错，但支撑论据有三处硬错误，且漏掉了对你最关键的一件事。** 三处硬错误：Fugu14（2021）早已是公开 untethered 越狱，"自 iOS 9 以来首个"不成立[^45^][^4^]；iOS 27 并未砍掉 iPhone 11[^5^][^7^]；修补通道实为 27.0.2/27.2 而非"27.0.x 或 27.1"[^108^][^109^]。漏掉的那件事见第三句。

**第三句：你的两台设备（A14/A15）在 iOS 26 任何版本都无法越狱，"留在 26 等越狱"的收益是确定的零——请按第 5 章修正版决策树及 5.5 行动清单行动。** Dopamine 的 iOS 26 支持仅限 A12/A13，A14/A15 公开越狱上限停在 17.3.1，而 26.1 已暴露于含 KEV 级在野 0day 的 11 个月补丁缺口[^71^][^72^][^74^][^104^][^105^]。

## 6.2 跨维度六大洞察

| # | 洞察 | 依据维度 | 置信度 |
|---|---|---|---|
| 1 | 报告"事实层"可信度高，"适用性层"才是用户的真实风险点 | dim01/02 × dim05/10 | 高 |
| 2 | 越狱生态"新旧倒挂"，A14/A15 处于最不利位置 | dim04/05/06 | 高 |
| 3 | 决策真结构是"确定性安全收益 vs 低概率越狱期权" | dim05/09/10 | 高 |
| 4 | 27.0.1 是限时窗口，随 27.2 发布而关闭 | dim02/09/10 | 中高 |
| 5 | AI 辅助挖洞是本次事件被低估的结构性信号 | dim01/02/04 | 中（趋势判断） |
| 6 | 论据质量需修正：发热论据被强化，性能数字被弱化 | dim08 | 高 |

解读：六条洞察构成"错因—处境—决策—时限—趋势—校准"链条。洞察 1/2/3/6 有多维度交叉印证，可作行动依据；4/5 属推断，仅用于时限与预期管理。

**洞察 1：事实层 vs 适用性层。** 报告对"发生了什么"的转述几乎全对，错误集中在"这对用户意味着什么"（E2 与遗漏 O1 皆属此类）。只核对事件本身的读者会误判报告整体可靠，这是本次审核最重要的阅读方法提示。

**洞察 2：新旧倒挂。** A12/A13 因 Dopamine 3.0 的纯软件链（momentarius PPL 绕过仅覆盖 A12/A13）且不受 SPTM 限制，反而可越狱至 26.0.1；A14 缺 PPL bypass、A15 缺 SPTM bypass，公开越狱上限停在 iOS 17.3.1、17.4 起无任何公开越狱，已停滞近三年[^71^][^76^][^77^]。"等越狱"的预期基线应设为极低概率，而非报告暗示的"锁版本留门即可"。

**洞察 3：确定性安全 vs 低概率期权。** 报告假设"26 与 27 都在 Calif 链理论覆盖内"，但 26.x 对 A14/A15 无任何现实路径，27.0.1 反而是唯一有实证演示的版本。"升 27.0.1 锁版本"不是折中而是理性占优解：免费持有低概率期权，同时补齐确定的安全缺口（iOS 27 修复 126 个 CVE）[^18^][^116^]。

**洞察 4：限时窗口。** 苹果历史封堵速度快至数日——iOS 12.4.1 约 8 天封堵 SockPuppet、16.5.1 约三周修复三角测量内核漏洞；27.2 已至 beta 4、预计 10 月下旬发布，其后 27.0.1 停签，"锁版本"策略永久失效[^123^][^124^][^108^][^109^]。行动窗口约 1–2 周，截止期就在眼下。

**洞察 5：AI 辅助挖洞的结构性趋势。** Calif 多次 AI 辅助突破获 Apple 安全说明致谢；Magnet 卷宗显示取证厂商早有非公开能力[^18^][^68^][^69^][^24^]。前沿模型压缩开发周期，报告却仅视 AI 为疑点而低估其趋势价值，公开工具期望应再下调。

**洞察 6：论据质量修正。** 发热耗电论据更扎实（iOS 26 投诉属实、26.1 未根治）[^125^][^126^][^127^]；30/70/80% 性能数字更虚（三机分测的"up to"上限），但 30% 测自 iPhone 11 Pro Max，A14 受益方向真实[^55^][^56^][^65^]。修正的是论据质量而非结论方向。

## 6.3 审核置信度与局限

本审核整体置信度为高：核心裁决均经一手来源（Apple 官网、Hexacon 官方、法院文件、Dopamine 仓库）或三方以上独立印证。须如实交代四处局限：其一，heise 原文未能直接抓取，相关表述经两家独立转述加搜索索引片段三方交叉确认，内容一致，但存在转述失真的残余风险[^38^][^41^]；其二，"A14/M1 也受 usbliter8 影响"的社区传闻与 Paradigm Shift 官方结论（A14+ 在 SecureROM 正确配置 DART）直接矛盾，无法证实，按传闻处理[^77^][^103^]；其三，Calif 链的芯片覆盖与发布计划无任何公开信息，"是否适配 A14/A15"在漏洞披露前不可判定[^38^]；其四，27.0.1 停签日期系按历史惯例外推，实际窗口可能浮动，建议以"10 月下旬前完成行动"的保守口径执行[^108^][^109^]。

---

# 参考文献

[1] Hexacon 官方 X 账号（@hexacon_fr）演示确认帖. X. 2026-10-16. https://x.com/hexacon_fr/status/2111030563020669055
[2] Works on iPhone 18 Pro: AI helps create untethered jailbreak for iOS 27.0.1. heise online. 2026-10-16. https://www.heise.de/en/news/Works-on-iPhone-18-Pro-AI-helps-create-untethered-jailbreak-for-iOS-27-0-1-11490907.html
[3] 用户 panic 日志（iPhone15,4, iPhone OS 27.0.1 (24A446), xnu-13432.2.10~2/RELEASE_ARM64_T8120）. otvet.mail.ru. 2026-10-13. https://otvet.mail.ru/question/270601050?reply
[4] Power of Community 2022 讲者页（Linus Henze 简介：Fugu14 为 "the first publicly available untethered jailbreak since iOS 9"）. powerofcommunity.net. https://powerofcommunity.net/2022/
[5] iOS 27 官方页面（兼容列表含 iPhone 11 全系与 iPhone SE 2 及后续）. Apple. https://www.apple.com/os/ios/
[6] Jailbreak/26.x（26.1 起 "No Tool Available"；iPad mini 6 各栏为 "No"）. The Apple Wiki. https://theapplewiki.com/wiki/Jailbreak/26.x
[7] Which iPhones Support Every iOS 27 Feature? MacRumors. 2026-09-09. https://www.macrumors.com/2026/09/09/which-iphones-support-every-ios-27-feature/
[8] iOS 26.7.1 available now for iPhone with security fixes. 9to5Mac. 2026-09-28. https://9to5mac.com/2026/09/28/ios-26-7-1-available-now-for-iphone-with-security-fixes/
[9] Hexacon — Call for Papers. hexacon.fr. https://www.hexacon.fr/conference/call-for-papers/
[10] Hexacon — Team. hexacon.fr. https://www.hexacon.fr/about/team/
[11] Billy Ellis（@bellis1000）Hexacon 现场手拍视频. X. 2026-10-16. https://x.com/bellis1000/status/2111028042495824337
[12] Calif 官网首页. calif.io. https://calif.io/
[13] Offensive Security Engineer @Calif（招聘页）. Remotive. 2026-08-14. https://remotive.com/remote/jobs/all-others/offensive-security-engineer-5547165
[14] New macOS vulnerabilities were exposed by Anthropic's Mythos: report（转引 WSJ/Robert McMillan）. 9to5Mac. 2026-05-14. https://9to5mac.com/2026/05/14/new-macos-vulnerabilities-were-exposed-by-anthropics-mythos-report/
[15] Calif security firm built AI-powered WeChat worm WeWorm（转引 NYT）. Quartz. 2026-09-08. https://tech.yahoo.com/cybersecurity/articles/calif-security-firm-built-ai-141643566.html
[16] Calif company profile. Crunchbase. https://www.crunchbase.com/organization/calif-837d
[17] Hexacon — Speakers / Agenda. hexacon.fr. https://www.hexacon.fr/conference/speakers/
[18] About the security content of iOS 27 and iPadOS 27. Apple Support. 2026-09-14. https://support.apple.com/en-us/149034
[19] Learning to Jailbreak an iPhone with Claude (Part 1). Calif Newsletter. 2026-04-15. https://blog.calif.io/p/learning-to-jailbreak-an-iphone-with
[20] First public macOS kernel memory corruption exploit on Apple M5. Calif Blog. 2026-05-14. https://blog.calif.io/p/first-public-kernel-memory-corruption
[21] Apple's May 2026 bulletin formally credits Claude on two macOS CVEs. LLM-Hacking. 2026-05-27. https://www.llm-hacking.com/hacks/apple-may-2026-claude-credited-cves.md/
[22] Password Fiction. Calif Research. 2026-08-10. https://calif.io/research/password-fiction
[23] Update your Mac: Screen Sharing vulnerability exploited in the wild. Malwarebytes. 2026-08-17. https://www.malwarebytes.com/blog/bugs/2026/08/update-your-mac-screen-sharing-vulnerability-exploited-in-the-wild
[24] Calif 聚合数据页（低可靠性，仅作对照）. Prospeo. https://prospeo.io/c/calif
[25] iOS 27.0.1 Untethered Jailbreak Demonstrated on iPhone 18 Pro. CyberSecurityNews. 2026-10-16. https://cybersecuritynews.com/ios-27-0-1-jailbreak-demonstrated/
[26] iOS 27.0.1 Jailbreak Survives Reboot and Restores Root Access on iPhone 18 Pro. Cyberpress. 2026-10-16. https://cyberpress.org/ios-27-0-1-jailbreak-survives-reboot/
[27] Hackers Jailbreak iPhone 18 Pro on Stage, and It Survives a Reboot. Decrypt. 2026-10-16. https://decrypt.co/381177/hackers-jailbreak-iphone-18-pro-survives-reboot
[28] iPhone 18 Pro (US/A3472) Specs (iPhone19,2). EveryMac. https://everymac.com/systems/apple/iphone/specs/apple-iphone-18-pro-united-states-a3472-specs.html
[29] iPhone 18 Pro — Identifier iPhone19,2; SoC A20 Pro. AppleDB. https://appledb.dev/device/iPhone-18-Pro-series.html
[30] iPhone 18 Pro benchmarks … Apple A20 Pro chip. PC Guide. 2026-09-16. https://www.pcguide.com/news/iphone-18-pro-benchmarks-reveal-impressive-scores-for-apple-a20-pro-chip-even-outperforming-desktop-cpus/
[31] Apple A19（A19/A19 Pro Part Number T8150）. Wikipedia. 2025-09-09. https://en.wikipedia.org/wiki/Apple_A19
[32] A20 Pro (Borneo, T8160) 芯片参数. 果粉查询芯片数据库. https://www.guofenchaxun.com/devices/silicon/chip_a/a20-pro-borneo-t8160
[33] Codenames（A20 一代 T8160 / Banda / Borneo）. The Apple Wiki. 2026-10-07. http://theapplewiki.com/wiki/User:Ilikeiphone123/Playground/Codenames
[34] iOS 27 — Version history（27.0.1 = 24A446, 2026-09-28）. Wikipedia. https://en.wikipedia.org/wiki/IOS_27
[35] iOS 27.0 Release Notes（Apple releases feed）. Blake Crosley. 2026-10-01. https://blakecrosley.com/blog/ios-27-release-notes
[36] iPhone 13 Pro panic 日志（xnu-13432.2.10~2/RELEASE_ARM64_T8110）. Reddit r/iOS27. https://www.reddit.com/r/iOS27/comments/1wp5mqd/
[37] Kernel（iOS 27.0 beta 起 Darwin 27.0.0 / xnu-13432.x）. The Apple Wiki. 2026-10-13. http://theapplewiki.com/wiki/Kernel
[38] Calif Unveils iPhone 18 Pro Jailbreak on iOS 27.0.1 at Hexacon（含 uptime 澄清与 heise 转述）. Pasquale Pillitteri. 2026-10-16. https://pasqualepillitteri.it/en/news/24540/jailbreak-iphone-18-pro-ios-27-calif-hexacon
[39] Calif Demos Untethered iOS 27.0.1 Jailbreak on iPhone 18 Pro. ONE Jailbreak. 2026-10-16. https://onejailbreak.com/blog/calif-demos-untethered-ios-27-0-1-jailbreak-on-iphone-18-pro/
[40] Researchers Demonstrate iOS 27.0.1 Untethered Jailbreak on iPhone 18 Pro. GBHackers. 2026-10-17. https://gbhackers.com/ios-27-0-1-untethered-jailbreak/
[41] Calif.io Jailbreaks iOS 27.0.1 on iPhone 18 Pro: What Is Claimed vs. Verified. explainX. 2026-10-16. https://explainx.ai/blog/calif-io-ios-27-0-1-untethered-jailbreak-iphone-18-pro-hexacon-ai-claimed-vs-verified-2026
[42] iPhone（"Today, we hacked what we'd set out to hack: the iPhone."）. Calif Newsletter. 2026-10-16. https://blog.calif.io/p/iphone
[43] Calif（@calif_io）Hexacon 演示帖（4 条）. X. 2026-10-16. https://x.com/calif_io/status/2111080247944745106
[44] Dank KI: Rebootfester Jailbreak für iOS 27.0.1 bis zum iPhone 18 Pro, untethered. heise online. 2026-10-16. https://www.heise.de/news/Untethered-Jailbreak-fuer-iOS-27-0-1-auf-iPhone-18-Pro-der-Reboots-ueberlebt-11490726.html
[45] Fugu14（incomplete untethered jailbreak for iOS 14.3 to 14.5.1, 24 October 2021）. The Apple Wiki. https://theapplewiki.com/wiki/Fugu14
[46] Xcode 27.1 Beta: Your App in the iPhone Duo Simulator. blakecrosley.com. 2026-10-11. https://blakecrosley.com/blog/xcode-27-1-beta-iphone-duo-simulator
[47] macOS 27.2 Beta 1 is Out... 27.1 Skipped for the iPhone Duo. macsonly.com. 2026-09-21. https://macsonly.com/article/2026/macos-27-2-beta-1-is-out
[48] heise 内容聚合镜像（逐字收录 heise 导语）. aimbit.de. 2026-10-16. https://aimbit.de/news
[49] Fugu14 Untethered Jailbreak Released, unc0ver Updated With Preliminary Support. iOSHacker. 2021-10-25. https://ioshacker.com/news/fugu14-untethered-jailbreak-released-unc0ver-updated-with-preliminary-support
[50] Linus Henze updates Fugu14 untether for reliability as Pwn20wnd updates unc0ver jailbreak to v7.0.1. iDownloadBlog. 2021-10-28. https://www.idownloadblog.com/tag/ios-14-5-1/
[51] Untethered Jailbreak Explained: Does One Exist in 2026? iOSJailbreak.pro. 2026-06-13. https://iosjailbreak.pro/untethered-jailbreak-explained-2026.html
[52] Apple security releases. Apple Support. https://support.apple.com/en-us/100100
[53] Apple releases iOS 27 with Siri AI overhaul. The Verge. 2026-09-14. https://www.theverge.com/news/994756/apple-ios-27-launch-iphone-siri-ai-update
[54] Apple Released iOS 27.2 Beta Before iOS 27.1. Zeera Wireless. 2026-09-17. https://zeerawireless.com/blogs/news/ios-27-2-beta-features-why-before-ios-27-1
[55] iOS 27 Performance Claims Explained: 30% Faster App Launches, 70% Faster Photos, 80% Faster AirDrop. The Mac Observer. 2026-09-10. https://www.macobserver.com/tips/round-ups/ios-27-performance-claims-explained/
[56] iOS 27 Speed Claims: What Apple's 30%, 70%, and 80% Numbers Actually Measure（含 Tom's Guide 独立实测）. XenoSpectrum. 2026-09-20. https://xenospectrum.com/en/ios27-performance-speed-tests/
[57] Major updates for Apple's software platforms are now available（Siri AI 英文 Beta、10 月新增语言）. Apple Newsroom. 2026-09-14. https://www.apple.com/newsroom/2026/09/major-updates-for-apples-software-platforms-are-now-available/
[58] iOS 27 Call Context Explained: Features, Privacy, and Eligibility. Gadget Hacks. 2026-06-09. https://apple.gadgethacks.com/news/ios-27-call-context-explained-features-privacy-and-eligibility/
[59] Siri AI Launches With iOS 27 On September 14（自 iPhone 15 Pro 起）. Dataconomy. 2026-09-10. https://dataconomy.com/2026/09/10/siri-ai-launches-with-ios-27-on-september-14/
[60] usbliter8-fun: iOS 27 Jailbreak on iPhone 11 Pro. ONE Jailbreak. 2026-07-25. https://onejailbreak.com/blog/usbliter8-fun-ios-27-jailbreak-on-iphone-11-pro/
[61] usbliter8-fun: iOS 27 jailbreak with usbliter8 exploit（34306/usbliter8-fun）. GitHub. 2026-07-08. https://github.com/34306/usbliter8-fun
[62] Magnet Forensics, LLC v. Del Gaudio, 1:26-cv-03781 (N.D. Ga.)（2026-07-21 命令原文）. CourtListener. 2026-07-21. https://www.courtlistener.com/docket/73584326/magnet-forensics-llc-v-del-gaudio/
[63] Android exploits commanding higher price than ever before（Threat Memo）. CERT-EU. https://cert.europa.eu/publications/threat-intelligence/threat-memo-190910-1/pdf
[64] Zero-Day Market Dynamics: Broker Pricing, Government Procurement, and Disclosure Norms. mapshock. 2026-08-22. https://mapshock.com/briefings/zero-day-market-dynamics-broker-pricing-government
[65] Will iOS 27 Slow Down Your Older iPhone? Apple Tested... The Mac Observer. 2026-09-11. https://www.macobserver.com/tips/round-ups/will-ios-27-slow-down-older-iphone-apple-published-claims/
[66] iPadOS 27 官方页面（兼容列表含 iPad mini 第 6 代；页脚注明 Apple Intelligence 设备门槛）. Apple. https://www.apple.com/os/ipados/
[67] iPadOS 27 Supported Devices List（iPad mini 6 不支持 Apple Intelligence）. EveryMac. 2026-09-16. https://everymac.com/systems/apple/ipad/ipad-faq/ipados-27-supported-devices-ipad-system-requirements.html
[68] iPhone exploit fight highlights who owns security research. AppleInsider. 2026-07-24. https://appleinsider.com/articles/26/07/24/iphone-exploit-legal-fight-is-really-about-who-owns-security-research
[69] usbliter8 lawsuit forces 'unpatchable' iPhone hack offline. Cult of Mac / Yahoo Tech. 2026-07-27. https://tech.yahoo.com/cybersecurity/articles/usbliter8-lawsuit-forces-unpatchable-iphone-140002992.html
[70] 苹果的安全系统一直难以破解 Mythos帮助研究人员找到了入侵途径. 163.com. 2026-05-15. https://www.163.com/dy/article/KSVN9AT30511BLFD.html
[71] Dopamine（含 3.0 发布说明）. The Apple Wiki. 2026-10-12. https://theapplewiki.com/wiki/Dopamine
[72] opa334/Dopamine 官方仓库. GitHub. https://github.com/opa334/dopamine
[73] iOS Jailbreak History: Every Major Tool From 2007 to 2026. TechEngage. 2026-09-14. https://techengage.com/ios-jailbreaking-complete-history/
[74] iPhone Jailbreak Guide 兼容矩阵. iClarified. 2026-08-07. https://www.iclarified.com/jailbreak
[75] opa334/Dopamine Releases（v3.0.5：支持 iOS 26.1 beta 1-3, A12/A13）. GitHub. 2026-08-13. https://github.com/opa334/Dopamine/releases
[76] Dopamine 3.0 Jailbreak With iOS 26 Support. ONE Jailbreak. 2026-08-07. https://onejailbreak.com/blog/dopamine-3-0-jailbreak-out-with-ios-26-support/
[77] Unpatchable BootROM Flaw Impacts Apple A12, A13 Chips. Infosecurity Magazine. 2026-06-22. https://www.infosecurity-magazine.com/news/apple-bootrom-exploit-a12-a13/
[78] Dopamine 官网. ellekit.space. https://ellekit.space/dopamine/
[79] Choose an IPSW for the iPhone 12（iPhone13,2：26.0.1 已停签）. ipsw.me. https://ipsw.me/iPhone13,2/
[80] Choose an IPSW（iPhone12,5：签署状态列表）. ipsw.me. https://ipsw.me/iPhone12,5
[81] Apple закрыла возможность отката с iOS 26.1 на iOS 26.0.1（苹果停止签署 iOS 26.0.1，2025 年 11 月）. OSTORE. https://ostore.ru/blog/obzory-tovarov-i-novosti/apple-zakryla-vozmozhnost-otkata-s-ios-26-1-na-ios-26-0-1/
[82] Apple SoC security. Apple Platform Security. 2026-01-28. https://support.apple.com/guide/security/apple-soc-security-sec87716a080/web
[83] Memory Integrity Enforcement: A complete vision for memory safety in Apple devices. Apple Security Research. 2025-09-09. https://security.apple.com/blog/memory-integrity-enforcement/
[84] Memory Integrity Enforcement Bolsters iPhone Security. TidBITS Talk. 2025-09-12. https://talk.tidbits.com/t/memory-integrity-enforcement-bolsters-iphone-security/31894
[85] Memory Integrity Enforcement (MIE) on iOS Deep Dive. 8ksec. 2026-01-27. https://www.8ksec.io/mie-deep-dive-kernel/
[86] XNU 内核安全研究（PPL/SPTM 章节）. arXiv:2510.09272. 2025-10-10. https://arxiv.org/html/2510.09272v1
[87] SPTM 推广时间线. Nicolás Alvarez (Mastodon). 2025-06-22. https://social.treehouse.systems/@nicolas17/114727754975559614
[88] Apple internals #0: The Apple security stack. sigreturn.com. 2026-06-21. https://sigreturn.com/blog/apple-security-stack/
[89] iPhone models compatible with iOS 27. Apple Support. https://support.apple.com/guide/iphone/iphone-models-compatible-with-ios-27-iphe3fa5df43/ios
[90] Can your iPhone run iOS 27? Here's the full compatibility list. Macworld. 2026-09-14. https://www.macworld.com/article/1811287/which-version-of-ios-can-my-iphone-run.html
[91] iPadOS 27 review: Compatible iPads. Tom's Guide. 2026-09-15. https://www.tomsguide.com/computing/ipads/ipados-27-review
[92] Operating system integrity. Apple Platform Security. 2026-01-28. https://support.apple.com/guide/security/operating-system-integrity-sec8b776536b/web
[93] xnu-vm-map-cur-max-bug（iOS 17.0→17.5.1 PPL 静默加固 diff）. GitHub Somisomair. 2026-03-28. https://github.com/Somisomair/xnu-vm-map-cur-max-bug
[94] Fugu14 README（neoncloud/Fugu14_onlinebuild）. GitHub. 2021-10-27. https://github.com/neoncloud/Fugu14_onlinebuild
[95] Fugu15 - The Journey to Jailbreaking iOS 15.4.1（Linus Henze slides）. Objective by the Sea v5. https://objectivebythesea.org/v5/talks/OBTS_v5_lHenze.pdf
[96] nekoJB Jailbreak Scam? Investigating the iOS 18.7 Fake Jailbreak Trap. Cellebrite/Corellium. 2026-06-08. https://cellebrite.com/en/blog/nekojb-jailbreak-scam/
[97] Fake iOS jailbreak scams are turning profiles into credential traps. NHI Mgmt Group. 2026-08-18. https://nhimg.org/articles/fake-ios-jailbreak-scams-are-turning-profiles-into-credential-traps/
[98] Jailbreak iOS 27: Latest Status, Tools & Safer Options. 4uKey (Tenorshare). 2026-08-28. https://www.4ukey.com/unlock-iphone/ios-27-jailbreak.html
[99] Why Jailbreak Is Over with iOS 26. Cellebrite. 2026-09-28. https://cellebrite.com/en/blog/ios-risk-for-mobile-app-developers-and-security-teams/
[100] Primeiro jailbreak para o iOS 26 é lançado（mas só em certos iPhones）. MacMagazine. 2026-08-10. https://macmagazine.com.br/post/2026/08/10/primeiro-jailbreak-para-o-ios-26-e-lancado-mas-so-em-certos-iphones/
[101] Unpatchable 'usbliter8' Exploit Breaks Apple A12 and A13 SecureROM Boot Chain. The Hacker News. 2026-06-20. https://thehackernews.com/2026/06/unpatchable-usbliter8-exploit-breaks.html
[102] usbliter8 Exploit: Apple A12/A13 SecureROM Flaw Explained. Hoplon Infosec. 2026-06-20. https://hoploninfosec.com/usbliter8-exploit-apple-a12-a13-securerom-vulnerability
[103] iOS 27 正式版推了…按芯片看（含 A14/M1 受影响之未证实传闻）. 什么值得买. 2026-09-17. https://post.smzdm.com/p/a70nrpn9/
[104] CRC Cloud Security Briefing（iOS 26.7.1 修复 CVE-2026-86950；CISA KEV 在列）. CRC Cloud. 2026-10-12. https://www.crccloud.com/resources/security-briefing/
[105] iOS 26.7.1 Fixes a CoreGraphics Flaw Possibly Used in Attacks. iThinkDiff. 2026-09-28. https://www.ithinkdiff.com/ios-26-7-1-coregraphics-exploited-vulnerability-fix/
[106] You can no longer downgrade to iOS 27 or iOS 26.7. APLWire. 2026-10-06. https://www.aplwire.com/2026/10/06/you-can-no-longer-downgrade-to-ios-27-or-ios-26-7/
[107] SEP and Baseband Compatibility List for FutureRestore downgrade（A12+ SHSH 无效）. ONE Jailbreak. 2025-09-05. https://onejailbreak.com/blog/futurerestore-sep-baseband-compatibility-list/
[108] iPhone Duo Will Have Day-One iOS 27.1 Update. MacRumors. 2026-10-13. https://www.macrumors.com/2026/10/13/iphone-duo-day-one-update/
[109] Michael Tsai Blog 汇编（Gruber：27.1 为 iPhone Duo 独占；27.2 预计 10 月下旬）. mjtsai.com. 2026-10-14. https://mjtsai.com/blog/
[110] How to Update…Using Delayed OTAs（关签后约 90 天缓冲）. BetaProfiles Forum. 2025-05-21. https://forum.betaprofiles.com/t/how-to-update-from-ios-beta-to-an-unsigned-official-release-using-delayed-otas/19574
[111] iOS 26.1 — Bug Fixes, Changes, and Improvements（2025-11-03 发布）. MacRumors Forums. 2025-11-03. https://forums.macrumors.com/threads/ios-26-1-bug-fixes-changes-and-improvements.2470382/
[112] Apple Patches 200 Vulnerabilities With New iOS 27, macOS Golden Gate 27 Releases. SecurityWeek. 2026-09-15. https://www.securityweek.com/apple-patches-200-vulnerabilities-with-new-ios-27-macos-golden-gate-27-releases/
[113] iOS 27 and iOS 26.7 Fix More Than 100 Security Bugs. MacRumors Forums. 2026-09-14. https://forums.macrumors.com/threads/2489301/
[114] CVE-2026-20700: Apple Patches Zero-Day Exploited in Sophisticated Cyber Attacks. SOC Prime. 2026-02-12. https://socprime.com/blog/cve-2026-20700-vulnerability/
[115] Apple patches CoreGraphics zero-day flaw exploited in attacks. BleepingComputer. 2026-09-29. https://www.bleepingcomputer.com/news/security/apple-patches-coregraphics-zero-day-flaw-exploited-in-attacks/
[116] iOS 27 Fixes 126 Security Flaws, Including 20 Kernel Issues. eSecurityPlanet. 2026-09-21. https://www.esecurityplanet.com/news/news-ios-27-126-security-fixes-kernel-2026/
[117] A jailbreak for iOS 26 is finally here, 326 days after launch. TechSpot. 2026-08-09. https://www.techspot.com/news/113408-jailbreak-ios-26-finally-here-326-days-after.html
[118] iOS 27.0.1 is the direct new upgrade from iOS 26（OTA 可选 27.0.1 或 26.7.1）. APLWire. 2026-10-09. https://www.aplwire.com/2026/10/09/ios-27-0-1-is-the-direct-new-upgrade-from-ios-26-not-ios-27/
[119] iOS 26 broke my iPhone. A fix is coming (I hope). Macworld. 2026-05-11. https://www.macworld.com/article/3129163/ios-26-iphone-performance-battery-ios-27-wwdc.html
[120] iOS 26.1: 'Killer' Update Reportedly Caused Battery Drain, Lagged UI and Vanished Apps. IBTimes UK. 2025-11-04. https://www.ibtimes.co.uk/ios-261-killer-update-reportedly-cause-battery-drain-lagged-iu-vanished-apps-1752355
[121] iOS 26.2 RC is Out – Full Features, Changes, Release Notes（续航反馈好转）. AutoAkhbar. 2025-12-06. https://autoakhbar.com/ios-26-2-rc-is-out-features-release-date/
[122] How to get the next generation of Apple Intelligence（iPhone 15 Pro 起；iPad M1+ 或 A17 Pro mini）. Apple Support. 2026-09-14. https://support.apple.com/en-us/121115
[123] The Great iOS Jailbreak of 2019—and why you should update to 12.4.1 right now! SecureMac. 2019-08-26. https://www.securemac.com/news/the-great-ios-jailbreak-of-2019-and-why-you-should-update-to-12-4-1-right-now
[124] Unc0ver（v3.5.0, 2019-08-18）. The Apple Wiki. https://theapplewiki.com/wiki/Unc0ver
[125] iOS 26 update sparks backlash: Overheating, battery drain and more. Livemint. 2025-09-19. https://www.livemint.com/technology/tech-news/ios-26-update-sparks-backlash-overheating-battery-drain-and-more-here-s-how-apple-vows-to-fix-them-11758263449295.html
[126] 苹果首次主动解释掉电快及发热原因，iOS 26更新后掉电快、发热？ 搜狐. 2025-09-16. https://www.sohu.com/a/935519410_121893989
[127] Apple Releases iOS 26.1 With Liquid Glass Toggle, Slide to Stop Alarm... MacRumors. 2025-11-03. https://www.macrumors.com/2025/11/03/apple-releases-ios-26-1/
