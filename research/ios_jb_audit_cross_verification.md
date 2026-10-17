# iOS 27.0.1 越狱报告审核 — 交叉验证结果

日期：2026-10-17 | 维度文件：ios_jb_audit_dim01–dim10.md

## 总体裁决：原报告 **核心事件属实，但含 3 处事实错误、2 处重要遗漏、若干推断需修正**

---

## Tier 1 — 高置信（≥2 个维度独立来源交叉确认）

| # | 断言 | 裁决 | 依据维度 |
|---|------|------|----------|
| 1 | 2026-10-16 Calif 于 Hexacon（巴黎，Synacktiv 主办）演示 iPhone 18 Pro（iPhone19,2/T8160/build 24A446）上 iOS 27.0.1 untethered 越狱：重启→密码解锁→SSH 2222 root（uid=0），dpkg 67 包 | **属实** | dim01（Hexacon 官方 X + 官网议程 + 6 家媒体）、dim02（内核 build 与真实 panic 日志逐字一致的强旁证） |
| 2 | 漏洞未披露、无 CVE、无公开工具；Calif 官方 X 账号称"Everything will be released in due time" | **属实**（"in due time"确为 Calif 原话，修正 dim 间小分歧） | dim01、dim02 |
| 3 | 演示在已解锁手机上进行，不涉及锁屏绕过 | **属实** | dim01、dim02 |
| 4 | Calif 是 Palo Alto 营利性安全公司，客户含 OpenAI/Anthropic/Google 等；有多次 AI 辅助挖洞记录；Apple iOS 27 安全说明（9-14）官方致谢"Calif.io in collaboration with Claude and Anthropic Research" | **属实** | dim01（Apple 官方安全页逐字确认） |
| 5 | heise 称 AI 帮助发现漏洞链但未说明方式；"对比 27.2 beta"仅为 heise 猜测 | **属实** | dim02、dim03 |
| 6 | usbliter8 = Paradigm Shift 2026-06-18 披露的 A12/A13（及 S4/S5）SecureROM 漏洞，不可通过 iOS 更新修补，物理访问型（DFU+USB+RP2350），不攻破 SEP | **属实** | dim04（原始披露 ps.tc） |
| 7 | 用户设备 12PM=A14、iPad mini 6=A15，不在 usbliter8 覆盖范围（A14+ 在 SecureROM 正确配置 DART） | **属实** | dim04、dim06（Paradigm Shift 原话） |
| 8 | iOS 27 无任何公开越狱工具；声称能越狱 iOS 27 的网站/App 多为骗局（nekoJB 恶意描述文件等已有实证剖析） | **属实** | dim02、dim05、dim09 |
| 9 | iOS 27 于 2026-09-14 发布，修复约 126 个 CVE；27.0.1（9-28）为当前最新公开版；27.2 已至 beta 3–4 | **属实** | dim07（Apple 官方安全页程序化计数）、dim09 |
| 10 | iPadOS 27 支持 iPad mini 6，但其不支持 Apple Intelligence（iPad 侧门槛 M1+ 或 mini A17 Pro） | **属实** | dim07（apple.com 官方列表） |
| 11 | iOS 26 曾遭大规模发热/耗电投诉，26.1 未根治（26.2 后好转） | **属实** | dim08 |

## Tier 2 — 证伪（原报告错误）

| # | 原报告断言 | 裁决 | 纠正 |
|---|-----------|------|------|
| E1 | "自 iOS 9 以来首个重启级越狱" | **证伪** | 2021 年 Linus Henze 的 **Fugu14**（iOS 14.3–14.5.1，arm64e 含 iPhone 12，开源发布）是公开 untethered 越狱，POC2022 官方简介称其为"the first publicly available untethered jailbreak since iOS 9"。heise 强断言错误，报告照录未加限定（dim03）。严谨表述：可能是"首个针对最新正式版系统的 untethered 演示"，且仅为演示非公开工具 |
| E2 | "iOS 27 砍掉 iPhone 11（仅支持 iPhone 12+）" | **证伪** | Apple 官方列表：iOS 27 兼容与 26 完全相同，iPhone 11 全系在列（被砍的是 XS/XR——发生在 iOS 26）。由此"12 系列已在淘汰边缘""留在 26 等于提前进维护期"的论证链断裂；且 iOS 26 分支仍在并行维护（26.7.1 与 27.0.1 同日发布）（dim07、dim10） |
| E3 | "预计苹果在下一个 27.0.x 或 27.1 修补" | **部分证伪** | 普通 iPhone 无 27.1（27.1 为 iPhone Duo 独占）；实际修补通道是 27.0.2 或 27.2（已 beta 4，预计 10 月下旬）→"锁 27.0.1"窗口仅约 1–2 周（dim09、dim10） |

## Tier 3 — 部分属实/需修正

| # | 断言 | 修正 |
|---|------|------|
| P1 | 性能数字"启动快 30%/照片快 70%/AirDrop 快 80%" | 官方口径属实但均为 **"up to"营销上限**，且分测三台不同设备（30% 恰测自 iPhone 11 Pro Max——比 A14 更老，说明老机型受益方向真实；Tom's Guide 实测 AirDrop 仅约 5%）。12PM 只能获底层流畅度收益（dim08） |
| P2 | "Siri AI + 跨 App + Call Context" | 功能真实但**英文 Beta 首发、需 iPhone 15 Pro+、国行首发无 AI、暂无中文**——对用户两台设备完全不适用（dim08） |
| P3 | usbliter8"无公开越狱工具" | 已过时：2026-07 起社区 usbliter8-fun 实现 iPhone 11 Pro 上 iOS 27.0b2 tethered 研究级越狱（破坏性）；无 checkra1n 式成熟工具。另：原始 write-up 已被美法院命令下架（Magnet 诉 Del Gaudio 案）（dim04） |
| P4 | "Calif 链大概率不适配/需适配 A14/A15" | 方向合理的**推断**，但 dim06 指出证据不足且 A14/A15 无 MIE 反而降低移植难度；真正障碍是 iOS 27 上 A14 缺公开 PPL bypass、A15 缺 SPTM bypass。注意 **A14（12PM）是 PPL 设备、A15（mini 6）是 SPTM 设备**，两者所需 bypass 不同 |
| P5 | "大会自称 Mythos era" | 精确措辞仅见单一二手来源；官网 keynote 确以 Mythos 模型为核心，方向正确（dim01） |
| P6 | "完整链在漏洞市场价值极高，Calif 无放出动机" | 前提属实（Zerodium $2M+、Crowdfense $7–9M、Google TAG：间谍厂商占 iOS 在野 0day 55%；Calif 历史模式为私下报告+补丁后发 crash-only PoC），但存在"in due time"这一反向信号，结论应为**不确定**而非"经常永远不公开"的强暗示（dim09） |

## Tier 4 — 重大遗漏（原报告未指出）

| # | 遗漏 | 影响 |
|---|------|------|
| O1 | **用户前提对其设备不成立**：Dopamine 3.0 的 iOS 26.0–26.0.1 越狱仅限 A12/A13（受益 iPhone 仅 11 系列/SE2）；A14/A15 在任何 26.x 上无越狱（上限 iOS 17.3.1）；26.1 正式版全芯片无工具；苹果已停签 26.0.1，降级不可行 | **颠覆用户决策框架**："留在 26 保越狱"的收益是确定的 0（dim05、dim10） |
| O2 | "停在 26.1"与"留在 iOS 26 分支"是两回事：26.1 已暴露约 11 个月补丁缺口（含 2 个确认在野 0day，CVE-2026-86950 在 CISA KEV），iOS 26 分支本身仍获维护（26.7.1） | 报告"升 27 才有补丁"的二分法不准确；停在 26.1 是最差选项（dim09、dim10） |
| O3 | 升级不可逆：A12+ 设备 SHSH blob 无效，升 27 是单向门；DelayOTA 有约 90 天补救窗口 | 决策需计入不可逆性（dim10） |

## 冲突区
- dim06 与报告对"A14/A15 移植难度"的定性存在程度差异（推断 vs 推断），无硬冲突，按 P4 处理。
- dim04 提及"A14/M1 也受 usbliter8 影响"的社区传闻——与 Paradigm Shift 官方矛盾，**无法证实**，按传闻标注。
- heise 原文在沙箱内无法直接抓取（dim01–dim03 均经独立转述+搜索快照三方交叉确认，内容一致），不影响裁决。

## 结论
无需 Phase 5 二次验证：所有冲突已在一手来源层面解决或明确标注为传闻/推断。
