# 技术解剖：Calif 于 iPhone Duo 发售日（2026-10-23）攻破 iPhone Duo

> 检索时间：2026-10-23。标注约定：**【已证实】**=有一手/多个独立来源直接支持；**【合理推断】**=由已证实事实强约束推出；**【未证实】**=传闻、单一二手来源或尚无公开证据。

---

## 0. 事件本体

**【已证实】** 2026-10-23 10:27 UTC，Calif 官方账号 @calif_io 发推："Just in case it wasn't clear: **we popped the iPhone Duo on release day**, adding a much needed feature, inspired by the php2shell hacker @lcfr_hax!"，引用了一条 09:55 UTC 的演示视频推文（视频 6 秒，CC @pwnlambda——疑为负责 Duo 移植的团队成员）[^fetch_urls:527#0^]。截至抓取时视频推 3,761 赞 / 79.8 万浏览。"adding a much needed feature, inspired by the php2shell hacker" 暗示演示内容是在被攻破的 Duo 上跑了一个 php→shell 式的"新功能"（即拿 shell 的玩笑式展示）。

iPhone Duo 当日发售，出厂 iOS 27.1（24A94425，设备标识 iPhone19,4），A20 Pro 芯片[^web_search:529#0^][^web_search:521#0^]。

---

## 1. 是否使用了与 Hexacon 演示同一条链？

**【已证实】** 没有任何 Calif 官方声明逐字确认"Duo 用的是同一条链"。Calif 的推文只说 "we popped"，未提链的构成[^fetch_urls:527#0^]；Calif 博客（blog.calif.io）截至检索时最新 iPhone 相关文章仍是 10-16 的 Hexacon 演示汇总，无 Duo 专门 write-up[^fetch_urls:525#0^][^fetch_urls:525#1^]。

**【合理推断，接近证实】** 第三方最接近确认的证据是 ONE Jailbreak 10-23 的分析文《iPhone Duo iOS 27.1 Jailbreak Window》[^web_search:520#6^]：

- 该文明确把 Duo 被攻破与 10-16 Hexacon 演示" ties back（关联回）"到一起：Calif 在 iPhone 18 Pro（同为 A20 Pro）上演示的 untethered 越狱——重启、正常输密码解锁后仍给出 root SSH 会话——其未修补 0-day 使得"Duo 发售日 untethered"成为可能；
- 越狱开发者 **Johnny（@0xjohnny）在 Duo 发售当天拿到 27.1 IPSW 做了二进制级比对**，确认：SPTM bypass 漏洞与 airlift 沙箱逃逸在 27.1 中**仍未修补**——这两条正是 Hexacon 链被描述包含的环节（内核读写 + SPTM bypass + 沙箱逃逸）[^web_search:520#6^]；
- 但 Johnny 随后**澄清**："SPTM 传闻可能被夸大了，Calif 在（Hexacon）演示本身中可能并未使用 SPTM bypass"[^web_search:520#6^]。这意味着"链含独立 SPTM bypass"这一早期描述存在不确定性——可能被夸大的只是传闻，也可能 Calif 演示用的是别的路径。

**结论**：Duo 被攻破在技术上依赖与 Hexacon 同一批未修补漏洞，是强证据支持的推断；但"完全同一条链、逐环节一致"无官方确认，且"SPTM bypass 是否真在链中"已被 Johnny 本人打上问号。**【未证实】** 的部分：Calif/Esser/其他成员对 Duo 攻击链构成的任何逐字技术表态——截至检索时不存在。

---

## 2. 版本跨度：27.0.1 → 27.1，漏洞在 27.1 中仍未修

**【已证实】** Hexacon 演示基于 iOS 27.0.1（24A446，9-28 发布）[^web_search:524#6^][^web_search:528#22^]；Duo 出厂 27.1（24A94425），是普通 iPhone 拿不到的独占版本（普通机型从 27.0.1 直接跳 27.2，苹果刻意跳过 27.1 以免折叠屏代码提前曝光）[^web_search:528#24^][^web_search:529#2^]。

**【已证实】** Johnny 对 Duo 27.1 IPSW 的二进制比对：SPTM bypass 漏洞与 airlift 沙箱逃逸在 27.1 中**依然开放**；airlift 在其他设备的 27.2 beta 3 中已被修补，但在 Duo 的 27.1 上"仍然可用"[^web_search:520#6^]。这直接证实：相关漏洞横跨 27.0.1 → 27.1 两个版本均未修复（合理，因为 27.1 与 27.0.x 属同一分支，而修复集中在 27.2 分支）。

**【合理推断】窗口上限**：Johnny 判断 Duo 一旦升到 27.2，"SPTM 漏洞以及 Calif 链的至少一部分、甚至全部预计将被关闭"[^web_search:520#6^]。因此窗口上限即 **iOS 27.2**：
- 27.2 RC（24B115）已于 10-19 向开发者推送[^web_search:528#0^][^web_search:528#1^]；
- theapplewiki 主站已出现 Duo 专属的 27.2 构建号 **24B91016** 条目[^web_search:529#10^]（但 ONE Jailbreak 10-23 称"Duo 尚无 27.2 RC IPSW"可供下载分析[^web_search:520#6^]——两者不矛盾：构建存在 ≠ 公开 IPSW 放出）；
- 27.2 正式版外界普遍预期 **10-27 前后**推送（RC 后约一周；HomePod mini 2 于 10-30 上市、需 HomePod 软件 27.2，构成时间锚点）[^web_search:528#2^][^web_search:528#10^][^web_search:528#13^]。

即：**Duo 的越狱窗口 = 出厂 27.1（24A94425）起，至 27.2 正式版落地止，预计仅约 4–7 天**。ONE Jailbreak 的建议直白："想保留这些路径，27.1 就是要守住的固件"[^web_search:520#6^]。

---

## 3. iOS 27.2 的修复状态

**【已证实】** opa334（Dopamine 作者 Lars Fröder）10-17 在 Mastodon 发帖：在意越狱的用户**不要**在 27.2 正式版放出后立即升级——传言 27.2 beta 3 已修补一处可独立用于越狱的 SPTM bypass；他明确该 SPTM bypass **与 Calif Hexacon 链无关**；且 iOS > 26.0.1 要做完整越狱仍需新的内核漏洞[^web_search:529#3^][^web_search:529#4^]。国内越狱媒体（爱思助手转载口径）同样记录"opa334 提醒勿升 27.2""苹果已修复关键 SPTM 旁路漏洞"，并指出 27.0.1 签名验证当时仍开放、已升 27.2 beta 者可刷机降级[^web_search:529#8^]。

**【已证实】** Esser 10-19 对 heise 所说"大部分漏洞已在 27.2 beta 修复"（任务背景，未重复核实）有独立旁证：
- Johnny 的 Duo IPSW 比对结论："SPTM 漏洞 + Calif 链的至少一部分/全部将在 27.2 关闭"[^web_search:520#6^]——与 Esser"大部分已修"方向一致，且把"修复落地版本"明确锁定为 27.2；
- airlift（Johnny 自己的沙箱逃逸）在 27.2 beta 3 已被修补[^web_search:520#6^]。

**【已证实】** 27.2 正式版**尚未发布**（截至 10-23）：最新公开节点是 10-19 的 RC（24B115），苹果未公布正式版日期，外界预期 10-27 前后[^web_search:528#0^][^web_search:528#2^]。

**【已证实/负面结果】** Apple 安全说明：截至 10-18 的检索，Apple 安全发布页上最新 iPhone 更新仍是 9-28 的 27.0.1，且**该版本不含任何安全修复**（主要为修 Face ID 重启）；即 Hexacon 演示时"没有可安装的修复"[^web_search:524#6^]。27.2 正式版的安全说明（是否给 Calif 漏洞分配 CVE、是否致谢）须待其发布后核查——目前**无 CVE、无 Apple 官方确认**[^web_search:524#3^]。

另注：与 Calif 链完全无关的另一事件——Johnny 的 TSS/Cryptex 服务端签名漏洞（27.2 beta 3 上的"半越狱"，无内核读写、无 SPTM bypass）已被苹果在服务端光速封堵，Johnny 宣布"已开源（已打补丁）"[^web_search:529#9^][^web_search:529#3^]。opa334 对该 bug 的评价是"入行十年见过最疯狂的洞"，并借此批评苹果对 SRD（安全研究设备）的保守政策[^web_search:529#9^]。注意勿与 Calif 链混淆[^web_search:520#7^]。

---

## 4. Calif 关于发布时间表的新信号

**【已证实】** 最新官方信号仍是 10-16 博客那句 "**Everything will be released in due time**"——无日期、无漏洞名、无工具承诺[^web_search:524#4^][^fetch_urls:525#1^]。博客归档页截至 10-23 检索时无 Duo 后续文章[^fetch_urls:525#0^]。

**【已证实】** 10-23 的 Duo 推文本身是"信号"性质：以戏谑口吻（"What else there to hack? Wrong answers only."）继续造势，但未给任何 write-up/PoC 时间承诺[^fetch_urls:527#0^]。

**【合理推断】** 结合背景中 Esser 10-19 的表态（补丁后披露漏洞 + 可能放 PoC、不做终端工具），披露节奏的强约束是 **27.2 正式版发布**：只要苹果还没把修复推给普通用户（27.2 尚未落地），Calif 大概率继续只放演示视频不放细节。换言之，**write-up/PoC 的最早现实窗口是 27.2 正式版发布（约 10-27）之后**。在此之前出现完整披露的概率低。**【未证实】**：任何确切的披露日期——不存在。

---

## 5. 技术圈对"发售日即攻破"的解读

### 5.1 对 MIE 防护的评价

**【已证实】** Apple 官方安全博客对 MIE（Memory Integrity Enforcement，A19 起引入的硬件内存标签+安全分配器）的定位是：对真实 exploit 链做红队重放后，"即便投入大量努力也无法通过换漏洞重建任何一条链"，MIE"在根本层面切断了大量利用步骤"[^web_search:530#0^]。业界随之出现"MIE 使依赖越狱的 iOS 测试过时"的论断[^web_search:530#2^]。

**【已证实】** 但 Calif 恰恰是 MIE 的第一个公开破解者：2026 年 5 月即在 macOS 26.4.1（M5/A19 平台）完成首次公开 MIE bypass（ Bruce Dang 7-27 博文《Apple MIE exploitation challenge》回顾"两个月前我们演示了首个公开 MIE bypass"[^fetch_urls:525#0^]；第三方报道指其借助 Anthropic Claude Mythos Preview，5 天攻破苹果逾五年打造的防护[^web_search:524#11^][^web_search:530#3^]）。

**【合理推断】** Duo（A20 Pro，MIE 二代硬件）发售日即破，意味着：**MIE 第二次被同一团队跨越，且这次是在全新形态设备、独占固件上当天完成**。技术圈的解读方向因此收敛为：MIE 显著抬高了链的构建成本、淘汰了"换洞续链"的旧模式，但对拥有 AI 辅助挖洞+全链自研能力的顶级团队**不构成不可逾越的壁垒**；8 月那种"越狱式测试已死"的论断被现实部分证伪[^web_search:530#2^]。需要注意的保留项：Johnny 关于"SPTM 传闻被夸大、演示或未用 SPTM bypass"的澄清[^web_search:520#6^]提醒——链对硬件缓解的穿透深度可能被社区高估，最终定性要等 Calif 的 write-up。

### 5.2 对 A14/A15（PPL/SPTM 老芯片）的移植前景

**【已证实】** 体系背景：PPL bypass 自 iOS 15.2 起为 A12+ 应用型越狱所必需；SPTM bypass 自 iOS 17.0 起为 A15+ 所必需，**而 iOS 27.0 beta 起该要求扩展到 A13–A14 及 M1**[^web_search:524#9^]。即：在 iOS 27 上，A14 也第一次需要 SPTM bypass，A15 本就需要；两者均无 MIE（MIE 仅 A19+）。

**【已证实】** 社区已有可用的老芯片 bypass 储备：Alfie 的 **Titan**（PPL+SPTM bypass，覆盖 A14–A17，iOS 16.1–17.4b3，为 Dopamine 3 集成设计）与 Staturnz 的 **Momentarius**（A12/A13 PPL bypass）已于 8 月开源[^web_search:524#10^]。opa334 同时指出：>26.0.1 的完整越狱仍缺新的内核漏洞[^web_search:529#3^]。

**【合理推断】** 若 Calif 链的漏洞确为 27.0.1/27.1 通用的软件漏洞（Esser 称"基本适用任何运行 27.0.1 的 iPhone"，见背景），则移植到 A14/A15 的边际成本反而**低于** A19/A20——因为无需处理 MIE，只需把内核读写落到既有的 PPL/SPTM bypass 技术上（Titan/Momentarius 类）。真正的门槛是 Calif 是否放出足够细节/PoC，以及 27.2 封堵后签名窗口是否关闭（10-20 前后 27.0.1 仍在签名，可降级[^web_search:529#8^]）。中文越狱社区已出现实际行动：有用户把 iPhone 12（A14）刷到 27.0.1 "等完美越狱"，社区评价"内核读写、SPTM 绕过、沙箱逃逸，哪一个都是重量级词汇"[^web_search:529#7^]。**【未证实】**：Calif 链在 A14/A15 上的实测——无任何公开验证。

### 5.3 其他值得记录的技术圈反应

- opa334 在 Hexacon 演示推文下现身，自嘲 "Hexacon FOMO"[^web_search:520#9^]；其后围绕 TSS bug 与 27.2 升级建议持续发声[^web_search:529#3^][^web_search:529#9^]。
- 社区最关心的问题：Lockdown Mode 能否挡住（Calif 未回答——其 Hexacon 演示含 Signal/iMessage 一键入口，理论上 Lockdown Mode 关闭的正是这类攻击面，但漏洞细节未公开前无人敢打包票）、是否已报苹果、是否 90 天后发布[^web_search:520#9^]。
- 冷静派媒体（iphonea2）提醒：会议舞台演示 ≠ 第三方可复现，"在 iPhone 18 Pro/27.0.1 上成立不能推出所有设备都脆弱"，真实覆盖面待证[^web_search:520#7^]。

---

## 6. 结论速览

| 命题 | 状态 |
|---|---|
| Calif 于 10-23 发售日攻破 iPhone Duo（27.1 / 24A94425） | **已证实**（官方推文+视频）[^fetch_urls:527#0^] |
| Duo 攻击与 Hexacon 27.0.1 链同源 | **合理推断（强）**：Johnny 的 27.1 IPSW 比对证实链上关键环节未修[^web_search:520#6^]；无 Calif 明示 |
| 链含独立 SPTM bypass | **未证实/被质疑**：Johnny 称传闻或被夸大[^web_search:520#6^] |
| 漏洞在 27.1 仍未修（窗口下限 27.0.1，上限 < 27.2） | **已证实**（IPSW 二进制比对）[^web_search:520#6^] |
| 27.2 将关闭 SPTM 洞 + Calif 链至少一部分 | **已证实（分析级）**，正式版预计 10-27 前后[^web_search:520#6^][^web_search:528#2^] |
| Esser"大部分漏洞已在 27.2 beta 修复"获旁证 | **已证实**（opa334 的 27.2b3 SPTM 修补传闻 + Johnny 比对）[^web_search:529#3^][^web_search:520#6^] |
| 27.2 正式版安全说明/CVE | **未发布**，不存在 |
| Calif 披露时间表新信号 | **无**，仍只有 "released in due time"[^web_search:524#4^] |
| MIE 被第二次跨越 → "抬高成本但非终结" | **合理推断**[^web_search:530#0^][^fetch_urls:525#0^] |
| A14/A15 移植前景乐观（无 MIE，缺内核洞与细节公开） | **合理推断**；实测**未证实**[^web_search:524#9^][^web_search:524#10^] |
