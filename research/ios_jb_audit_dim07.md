# iOS/iPadOS 27 支持列表与安全更新事实核查报告（dim07）

- 核查日期：2026-10-17
- 核查方式：16 次独立网络搜索/抓取，优先采用苹果官方来源（apple.com 产品页、Apple 支持文档、Apple 安全发布页与安全内容页），辅以 MacRumors、Macworld、The Verge、Tom's Guide、EveryMac 等。
- 总体结论：**断言 1 前半属实、后半被证伪（iOS 27 并未砍掉 iPhone 11）；断言 2、3、4 基本属实（含需补充的重要细节）；断言 5 的推断前提为假，推断不成立。**

---

## 断言 1：iOS 26 支持 iPhone 11 及更新；iOS 27 砍掉 iPhone 11，仅支持 iPhone 12 及更新

**裁决：部分属实 —— 前半属实，后半【证伪】。置信度：极高。**

- 前半「iOS 26 支持 iPhone 11 及更新」**属实**。苹果官方支持文档《How to download iOS 26》逐字列出："iOS 26 is compatible with iPhone models: … iPhone 11, iPhone 11 Pro, iPhone 11 Pro Max, iPhone SE (2nd generation and later)"[^1^]。
- 后半「iOS 27 砍掉 iPhone 11」**证伪**。苹果官网 iOS 27 页面（2026-10-17 抓取）逐字写明："iOS 27 is compatible with these devices. … iPhone 11 Pro Max, iPhone 11 Pro, iPhone 11, iPhone SE (2nd generation and later)"[^2^]。苹果安全发布页对 iOS 27 及 iOS 27.0.1 的适用设备均标注 "iPhone 11 and later"[^3^]。MacRumors 2026-09-09 报道逐字写道："Apple's latest software carries over the same device compatibility list as iOS 26, so no iPhones have been dropped this year."[^4^]The Verge 发布日报道同样写明 "The iOS 27 update will be available for iPhone 11 and iPhone SE (2nd gen) and later devices"[^5^]。
- 备注：检索中发现个别低可信度网页（如 techpulse.press）声称 iOS 27 仅支持 iPhone 13 及更新，与苹果官方页面直接矛盾，应视为错误信息，不予采信。

## 断言 2：iPadOS 27 支持列表（用户持有 iPad mini 6，需确认是否支持）

**裁决：属实 —— iPadOS 27 支持 iPad mini（第 6 代，A15）。置信度：极高。**

- 苹果官网 iPadOS 27 页面（2026-10-17 抓取）逐字写道："iPadOS 27 is compatible with these devices. … iPad Pro (4th generation and later, M1 and later), iPad Air (4th generation and later, M2 and later), iPad (9th generation and later, A16), **iPad mini (6th generation, A17 Pro)**"[^6^]。
- 苹果安全内容页对 iPadOS 27 的适用范围同样写明 "… iPad 9th generation and later, and **iPad mini 6th generation and later**"[^7^]。MacRumors iPadOS 27 roundup 兼容列表含 "iPad mini (6th generation)"[^8^]。
- iPadOS 27 砍掉的机型为：iPad 第 8 代、iPad mini 第 5 代、iPad Air 第 3 代、11 英寸 iPad Pro 第 1 代、12.9 英寸 iPad Pro 第 3 代（即 A12/A12X 机型），这些设备停留在 iPadOS 26 并继续获得安全更新[^8^][^9^]。
- **重要补充（iPad 侧 AI 门槛）：iPad mini 6（A15）虽能升级 iPadOS 27，但不支持 Apple Intelligence / Siri AI。** 苹果官网页脚逐字注明："Apple Intelligence is available on all iPhone 16 models and later, iPhone 15 Pro, iPhone 15 Pro Max, **iPad mini (A17 Pro)**, iPad models with M1 and later …"[^6^]——即 iPad 侧 AI 门槛为 M1 及更新芯片、或 iPad mini（A17 Pro，第 7 代）。EveryMac 亦明确指出："The iPad mini 6, iPad Air 4, and iPad Pro 11\" (2nd Gen) and iPad Pro 12.9\" (4th Gen) do not support it [Apple Intelligence], either."[^10^]

## 断言 3：完整 Apple Intelligence 功能仅 iPhone 15 Pro 及更新机型可用（iOS 27 下）；iPhone 12/13/14 无法使用完整 AI 功能

**裁决：属实（含一处可补充的分层细节）。置信度：高。**

- 苹果官方支持文档《How to get the next generation of Apple Intelligence》（2026-09-14）逐字写道："Apple Intelligence in iOS 27 … are available on iPhone 16 models or later, **iPhone 15 Pro, iPhone 15 Pro Max**, iPhone Air, iPad mini (A17 Pro), … iPad models with M1 or later, Mac with M1 or later …"[^11^]。
- 因此 iPhone 12/13/14 全系、以及**标准版 iPhone 15/15 Plus** 均被排除在 Apple Intelligence 之外（A17 Pro/8GB RAM 为 iPhone 侧门槛）[^11^][^12^]。断言中「iPhone 12/13/14 等无法使用完整 AI 功能」成立。
- 分层细节（报告可酌情补充）：MacRumors 指出 iOS 27 中最先进的端侧模型（更具表现力的 Siri 语音、增强听写）需要 12GB 内存，"the iPhone 17 Pro, iPhone 17 Pro Max, and iPhone Air are the only current models that support every new Siri AI feature"，iPhone 17/17e 仅 8GB[^4^]。即「完整」之上还有一个更高的 12GB 档位，但不影响 15 Pro 为 AI 入门门槛这一核心论断。

## 断言 4：iOS 27 于 2026-09-14 发布，修复 120+ 漏洞（含 20 个内核漏洞）；iOS 27.0.1 为当前最新公开版；iOS 27.2 已进入 beta

**裁决：属实（内核漏洞计数口径见备注）。置信度：高。**

- 发布日期：苹果安全内容页逐字注明 "**Released September 14, 2026**"，适用 "iPhone 11 and later …"[^7^]；MacRumors、Macworld、The Verge 均确认为 2026-09-14（周一）发布[^5^][^8^][^13^]。
- 漏洞数量：本次核查直接抓取了苹果官方安全内容页（support.apple.com/en-us/149034）并程序化统计，页面含 **126 个互不重复的 CVE 编号**，与 eSecurityPlanet 的独立统计 "Apple's security bulletin lists 122 fixes covering 126 unique CVEs, including 20 entries tied to the iPhone kernel" 一致[^14^]。「120+ 漏洞」**属实**。
- 内核漏洞计数备注：官方页面以「Kernel」为组件标题的条目为 **18 条**（本次抓取的程序化计数），另有若干内核级影响（如沙盒应用提权至内核 CVE-2026-84607、内核内存损坏 CVE-2026-28968 等）列在 AVEVideoEncoder 等其他组件下；eSecurityPlanet 按影响口径计为 20 条[^14^][^15^]。故「含 20 个内核漏洞」与第三方统计一致，可接受，但非苹果官方分组口径。
- iOS 27.0.1 为当前最新公开版：苹果安全发布页（2026-09-28 更新）将 "iOS 27.0.1 and iPadOS 27.0.1 — This update has no published CVE entries" 列为最新，发布日期 2026-09-28[^3^]；Macworld 同日发布其更新说明（修复 iPhone 18 Pro Face ID 重启等问题）[^16^]。截至 2026-10-17，无 27.0.2/27.1 正式版记录（iOS 27.1 预计随 iPhone Duo 于 10 月 23 日前后发布）[^17^]。**属实**。
- iOS 27.2 已进入 beta：**属实**。IPSW 构建记录显示 iOS 27.2 beta 1（24B5084k，9 月 16 日）、beta 2（9 月 21 日）、beta 3（24B5099f，2026-10-05）已发布[^18^]；苹果开发者发布 feed 中亦有 Xcode 27.2 beta 2[^19^]。

## 断言 5：「iPhone 11 被砍支持说明 iPhone 12 已在淘汰边缘，留在 26 等于提前进维护期」——推断合理性评估

**裁决：推断不成立（前提证伪 + 历史规律不支持）。置信度：高。**

- 前提为假：如断言 1 所证，iOS 27 **没有**砍掉 iPhone 11，兼容列表与 iOS 26 完全相同[^2^][^4^]。整个推断的事实基础不存在。
- 历史支持周期：Macworld 总结苹果惯例为 "Major iOS upgrades typically continue for around five to seven years, while security-related updates can continue for considerably longer"[^20^]；RefurbMe 统计 2015 年后机型平均获 6–7 年大版本更新（均值约 6.4 年），且苹果在设备止步大版本后仍提供 1–2 年以上安全补丁[^12^]。实例：iPhone 6s（2015）2026 年 5 月仍获 iOS 15.8.8 安全更新（超 10 年）；iPhone XR/XS（2018）止步 iOS 18 后 2026 年 8 月仍获 iOS 18.7.10[^20^]。
- 按此规律，iPhone 11（2019 年发布）如今运行第 8 个大版本（iOS 13→27），已属苹果史上最长支持之列；iPhone 12（2020 年发布）距支持底线还有两代机型（11、SE 2），按历史节奏预计仍可获约 2028–2030 年之前的大版本更新[^20^][^12^]。
- 「留在旧大版本 = 提前进维护期」亦不成立：苹果同日为旧分支发布含大部分同源修复的安全更新（如 iOS/iPadOS 26.7 含 iOS 27 约 80 项修复，26.7.1 持续更新）[^3^][^14^]。真正处于「淘汰边缘」的是 iPhone XR/XS（止步 iOS 18，仅靠安全补丁延续）[^20^]。

---

## 给用户的直接答案（iPad mini 6）

iPad mini 6（A15）**可以升级 iPadOS 27**，但**无法使用任何 Apple Intelligence / Siri AI 功能**（iPad 侧门槛为 M1 及更新芯片或 iPad mini A17 Pro 第 7 代）；升级获得的是系统功能与安全更新，不含 AI 层[^6^][^10^][^11^]。

## 引用来源

[^1^]: Apple Support, "How to download iOS 26", https://support.apple.com/en-us/123705 （页面更新 2026-09-16，检索于 2026-10-17）
[^2^]: Apple, "iOS 27" 官方页面, https://www.apple.com/os/ios/ （2026-10-17 直接抓取，兼容列表含 iPhone 11/11 Pro/11 Pro Max 与 iPhone SE 2 及后续）
[^3^]: Apple Support, "Apple security releases", https://support.apple.com/en-us/100100 （2026-09-28 更新：iOS 27.0.1/iPadOS 27.0.1 无 CVE 条目；iOS 27/iPadOS 27 发布于 2026-09-14）
[^4^]: MacRumors, "Which iPhones Support Every iOS 27 Feature?", https://www.macrumors.com/2026/09/09/which-iphones-support-every-ios-27-feature/ （2026-09-09）
[^5^]: The Verge, "Apple releases iOS 27 with Siri AI overhaul", https://www.theverge.com/news/994756/apple-ios-27-launch-iphone-siri-ai-update （2026-09-14）
[^6^]: Apple, "iPadOS 27" 官方页面, https://www.apple.com/os/ipados/ （2026-10-17 直接抓取；兼容列表含 iPad mini 第 6 代；页脚注明 Apple Intelligence 设备门槛）
[^7^]: Apple Support, "About the security content of iOS 27 and iPadOS 27", https://support.apple.com/en-us/149034 （2026-09-14；本次核查程序化统计页面含 126 个唯一 CVE）
[^8^]: MacRumors, "iPadOS 27 roundup", https://www.macrumors.com/roundup/ipados/ （2026-09-14：兼容列表与被砍机型）
[^9^]: Stuff, "iPadOS 27 system requirements", https://www.stuff.tv/features/ipados-27-system-requirements-will-it-run-on-your-apple-ipad/ （2026-09-16）
[^10^]: EveryMac, "iPadOS 27 Supported Devices List", https://everymac.com/systems/apple/ipad/ipad-faq/ipados-27-supported-devices-ipad-system-requirements.html （2026-09-16：明确指出 iPad mini 6 不支持 Apple Intelligence）
[^11^]: Apple Support, "How to get the next generation of Apple Intelligence", https://support.apple.com/en-us/121115 （2026-09-14：AI 门槛为 iPhone 15 Pro/Pro Max、iPhone 16 及更新；iPad 为 M1+ 或 iPad mini A17 Pro）
[^12^]: RefurbMe, "How Long Does Apple Support iPhones? (2026 Guide)", https://www.refurb.me/blog/how-long-does-apple-support-iphones （2026-09-15：6–7 年大版本更新均值 6.4 年；iOS 27 未砍任何机型）
[^13^]: Macworld, "iOS 27: What's new, what's in the latest beta", https://www.macworld.com/article/2986799/ios-27-features-compatiblity-siri-ai-updates.html （2026-10-13）
[^14^]: eSecurityPlanet, "iOS 27 Fixes 126 Security Flaws, Including 20 Kernel Issues", https://www.esecurityplanet.com/news/news-ios-27-126-security-fixes-kernel-2026/ （2026-09-21）
[^15^]: CyberPress, "Apple iOS 27 and macOS 27 Security Updates Fix RCE, Kernel and WebKit Flaws", https://cyberpress.org/apple-ios-27-and-macos-27-security-updates-fix/ （2026-09-16）
[^16^]: Macworld, "iOS 27.0.1 is out now to fix iPhone 18 Pro restart bug", https://www.macworld.com/article/3245812/ios-27-0-1-is-out-now-to-fix-iphone-18-pro-restart-bug.html （2026-09-28）
[^17^]: ZEERA, "iOS 27.2 Release Date", https://zeerawireless.com/blogs/news/ios-27-2-release-date-should-you-wait （2026-10-05：苹果列 iOS 27.0.1 为最新公开版，27.2 处于 beta 2）
[^18^]: IPSWBeta.dev, "iOS 27.x Release History", https://ipswbeta.dev/ios/27.x/ （2026-10-05：iOS 27.2 beta 3 build 24B5099f）
[^19^]: Blake Crosley, "iOS 27.0 Release Notes"（引用 Apple Releases RSS feed）, https://blakecrosley.com/blog/ios-27-release-notes （2026-10-01）
[^20^]: Macworld, "How long does Apple support iPhones with updates?", https://www.macworld.com/article/675021/how-long-does-apple-support-iphones.html （2026-09-15：大版本更新约 5–7 年；XR/XS 止步 iOS 18 仍获安全补丁）
