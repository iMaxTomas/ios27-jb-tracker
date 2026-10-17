# usbliter8 断言事实核查报告（dim04）

- 核查日期：2026-10-17
- 核查方式：英文为主的网络检索（16 次独立搜索 + 原文页面抓取），交叉比对 ≥15 个独立来源
- 原始披露：Paradigm Shift《Introducing usbliter8: An A12/A13 SecureROM Exploit》，2026-06-18 发布于 `https://ps.tc/pages/blog-usbliter8.html`；原始 PoC 仓库 `github.com/prdgmshift/usbliter8`。**两者已于 2026-07-23 依美国联邦法院命令下架**（详见下文"重大背景事件"），原文可通过 Wayback Machine 存档访问（2026-06-18 快照）[^1^][^2^][^3^]。

---

## 一、逐条裁决

### 断言 1：usbliter8 是 A12/A13 芯片的 BootROM（SecureROM）漏洞，苹果无法通过系统更新修补

**裁决：属实（置信度：高）**

- 原始披露标题即为 *"Introducing usbliter8: An A12/A13 SecureROM Exploit"*，Paradigm Shift 于 2026-06-18 发布完整 write-up 与可用 PoC[^1^][^2^]。
- 漏洞根源是 Synopsys DesignWare DWC2 USB 控制器中的 DMA 指针下溢（第 4 个 Setup 包时 DMA 地址固定回退 24 字节，而短包只按实际长度递增，形成 12 字节 underflow），叠加 A12/A13 在 SecureROM 中将 USB DART 配置为 bypass。漏洞代码烧录在硅片只读存储器中，"no iOS update will ever close it"[^4^][^5^]。
- Paradigm Shift 原文（经多家媒体逐字转引）：*"As these vulnerabilities remain in unmodified code, affected users should know that upgrading to more modern hardware remains the most effective way to mitigate the risks."*[^6^]
- 影响范围精确表述：公开 PoC 支持 **A12、A13、S4、S5**；**A12X/A12Z 理论上易受攻击但 PoC 未实现**（截至 2026-10 仍如此）。The Apple Wiki 记录其影响 SecureROM 版本 3865.0.0.4.7 – 4479.0.0.100.4，在 5281.0.0.100.45 中修复[^2^][^7^][^8^]。
- 补充：披露前已与 Apple Product Security 协调（coordinated disclosure）[^4^]。

### 断言 2：物理访问型 PoC（DFU + USB + RP2350 类微控制器），无公开越狱工具

**裁决：部分属实——前半句属实，"无公开越狱工具"已过时/部分证伪（置信度：高）**

- 物理访问前提属实且被所有来源一致确认：需物理持有设备、手动进入 DFU、经 USB 连接可编程微控制器，全程 <2 秒[^4^][^7^][^9^]。细节修正：主要用 **RP2350**（如 Raspberry Pi Pico 2 / Waveshare RP2350 USB-A），**RP2040 也可用但成功率显著更低**[^2^][^10^]。
- 但"无公开越狱工具"在 2026-10-17 这个时间点**不成立**：
  - 2026-07-25 前后，社区项目 **usbliter8-fun**（github.com/34306/usbliter8-fun，基于 wh1te4ever 早期工作、Huy Nguyen 补丁）公开实现了 iPhone 11 Pro 上 **iOS 27.0 beta 2 的 tethered root 越狱**，含内核/设备树/用户态补丁、SSH、Sileo 安装流程[^11^][^12^]。
  - Elcomsoft 于 2026-07-15 发布了基于 usbliter8 的 **ipwndfu fork**（读/写/执行原语 + BootROM sigpatch），并正将其整合进 iOS Forensic Toolkit[^10^]。
  - 已有 SSH ramdisk 中文/英文全流程教程、约 10 美元的商用 "SUNSHINE RP2354A Pico DFU 工程板" 在售[^13^][^14^]。
- 准确表述应为：**尚不存在 checkra1n 式的用户友好、半完美（semi-tethered）越狱工具**；且因 iOS 17 起 BPR 强化（DFU 引导时 SEP 拒绝解密挂载 /var，直接 kernel panic），社区普遍认为 checkra1n/palera1n 式半完美越狱在 A12/A13 上**不可行**，只能做全 tethered 越狱[^15^][^16^]。usbliter8-fun 也明确是"会破坏设备的研究性项目"而非日用越狱。

### 断言 3：自 2019 年 checkm8 以来首个影响 A11 之后芯片的 BootROM 漏洞

**裁决：属实，但应限定为"首个公开披露的"（置信度：高）**

- Elcomsoft（2026-07-15）：*"It has been almost 7 years since the last major BootROM exploit called checkm8 was released, which exploits Apple chips up to A11… More recently, just last month, a new BootROM exploit called usbliter8 was published."*[^10^]
- 多家独立媒体（iRemove、Feedsta、pasqualepillitteri、xakep 等）一致称它是 checkm8 之后首个落在 post-A11 硅片上的公开 BootROM 漏洞[^4^][^7^][^17^]。
- 重要限定：Magnet Forensics 诉状称其内部早在 2024 年 4-5 月已拥有同类 A12/A13 SecureROM 零日能力（代号 "MSG"）并集成进商业取证产品——即**非公开**的同类漏洞实现可能更早存在。因此严谨表述为"首个**公开披露**的"[^18^][^19^]。

### 断言 4：iPhone 12 Pro Max（A14）与 iPad mini 6（A15）不在覆盖范围

**裁决：属实（置信度：高）**

- 原始研究及多方技术转述一致：**A11 不受影响**（其 USB 驱动每次传输前手动清除 DMA 地址）；**A14 及更新芯片在 SecureROM 中正确配置 DART**，不暴露该原语[^4^][^7^][^17^]。iPhone 12 Pro Max 为 A14、iPad mini 6 为 A15，均在覆盖范围之外。
- 谨慎提示（不影响裁决）：2026-09-01 有中文社区（玄烨）爆料称"早期 A14 与 M1 设备同样受影响，细节未公开"。该说法**无法证实**，与原始研究和所有主流技术来源矛盾，列为未证实传闻[^20^]。

---

## 二、专项核查：是否已有"checkra1n 之于 checkm8"式的越狱工具？披露后社区进展

**结论：没有 checkra1n 式的用户友好工具；但存在公开的研究级越狱链与取证工具整合，生态在 2026 年 7-10 月快速演进。**

时间线（均为公开来源）：
- **2026-06-18**：Paradigm Shift 发布 write-up + PoC（GitHub 数小时内 280+ star）[^1^]。
- **2026-07-03 前后**：SSH ramdisk 教程（iPhone XR，"PWND: usbliter8"）[^13^]。
- **2026-07-07**：Magnet Forensics 在佐治亚州北区联邦法院起诉前工程师 Mario Del Gaudio 与 Paradigm Shift（案号 1:26-cv-03781），指控 usbliter8 系其商业秘密 "MSG" 能力[^18^][^19^]。
- **2026-07-15**：Elcomsoft 发布 A12 BootROM sigpatch 研究与 ipwndfu fork，宣布将整合进自家取证工具[^10^]。
- **2026-07-21/23**：法官 Victoria Calvert 签发禁令，Paradigm Shift 删除博文与 PoC；社区镜像（ahmadkamal09999-tech、neonmodder123、JoshAtticus、AldazActivator/usbliter8-c 等）继续流传[^1^][^3^][^19^]。
- **2026-07-25/27**：**usbliter8-fun** 公开——iPhone 11 Pro 上 iOS 27.0 beta 2 定制固件恢复 + tethered root 越狱 + Sileo[^11^][^12^]。
- **2026-08-20**：中国厂商量产约 10 美元的 RP2354A DFU 工程板（带 Lightning/USB-C）[^14^]。
- **2026-09-17（中文社区综述）**：usbliter8 被用作"降级前置"（无 SHSH 强降、Spironolactone SSHRD 维护盘），降级后再上 Dopamine/surrealra1n；**iOS 27 正式版（9-14 发布）上没有可"上车"的越狱工具**[^20^]。

与 checkra1n 的关键差距：usbliter8 每次重启需重新外接硬件触发（全 tethered）；iOS 17+ 的 SEP/BPR 限制使半完美越狱被认为不可行；现有公开链仅限单一机型（iPhone 11 Pro）且是破坏性的[^11^][^15^]。

## 三、专项核查：usbliter8 与 iOS 27 越狱的关系

- **能否用于越狱 iOS 27？** 能，但形态有限：usbliter8-fun 于 2026-07 月在 iPhone 11 Pro 上引导了越狱态 **iOS 27.0 beta 2（build 24A5370h）**——iPhone 11 Pro 是 iOS 27 兼容列表中最老的机型，故成为演示目标[^12^][^21^]。该链为全 NAND 写入的定制固件恢复（make_cfw.py / tss_proxy_server.sh / restore_cfw.sh），打补丁绕过 USB Restricted Mode、/var/jb 沙盒、AMFI trust-cache，并 NOP 掉 coreauthd/ctkd 以容忍 SEP 异常[^11^]。
- **为何只演示 27.0 beta 2？** 原因是补丁与内核/DeviceTree **偏移量按具体 build 硬编码**（build-specific），换版本/换机型需重新找偏移；仓库中另存在 work-27.0b3 目录（下一 beta 的在制分支），但截至 2026-10-17 未见正式版 iOS 27.0/27.0.1 的可用链[^11^][^12^]。
- **代价**：恢复定制固件会清空设备并破坏 **SEP 相关功能、密码、Wi-Fi、基带、蓝牙（部分）及 Apple 服务**——与报告引用的 GBHackers 说法一致。GBHackers（2026-07-27）原文：*"restoring the custom firmware may erase the device and render major functions nonfunctional, including passcode support, Wi-Fi, baseband services, parts of Bluetooth functionality, Apple services, and features dependent on the Secure Enclave Processor."*[^21^]
- **iOS 27 越狱全景（2026-10-17）**：同日 GBHackers 报道黑客组织 Calif 演示了 iPhone 18 Pro 上 **iOS 27.0.1 untethered 越狱**，与 usbliter8 **无关**（未披露所用漏洞、无公开工具）；该报道明确将 usbliter8 演示描述为"需要物理访问、DFU 和专用 USB 硬件，并破坏了 SEP 依赖功能、Wi-Fi 和基带"[^22^]。

## 四、重大背景事件：原始披露已被法院命令下架（报告未提及，建议补充）

Magnet Forensics（GrayKey 母公司）诉称 usbliter8 源自其前工程师 Del Gaudio 在职期间接触的商业秘密 "MSG"。2026-07-21，美国联邦法官 Victoria Calvert 部分批准临时禁令，命令被告于 7-23 前删除博文 *"Introducing usbliter8: An A12/A13 Secure ROM Exploit"* 及全部 PoC 代码与衍生材料（案件仍在审理，商业秘密归属未有终局裁决）[^3^][^18^][^19^]。这意味着"找到 Paradigm Shift 原始披露页面"目前只能依赖 web.archive.org 存档与社区镜像。

## 五、汇总表

| # | 断言 | 裁决 | 置信度 |
|---|------|------|--------|
| 1 | A12/A13 BootROM 漏洞、不可软件修补 | 属实（影响范围还应含 S4/S5；A12X/Z 理论受影响未实现） | 高 |
| 2 | 物理访问型 PoC；无公开越狱工具 | 部分属实（物理 PoC 属实；"无公开越狱工具"已过时——存在 usbliter8-fun 等研究级公开越狱链，但无 checkra1n 式成熟工具） | 高 |
| 3 | checkm8 以来首个 post-A11 BootROM 漏洞 | 属实（限定"公开披露"；诉讼材料显示取证厂商或有更早非公开实现） | 高 |
| 4 | A14（iPhone 12 Pro Max）/A15（iPad mini 6）不在覆盖范围 | 属实（另有"A14/M1 也受影响"的未证实传闻，不采信） | 高 |

---

## 引用来源

[^1^]: ONE Jailbreak, "usbliter8 Exploit Removed After Court Order in Magnet Suit", 2026-07-23. https://onejailbreak.com/blog/usbliter8-exploit-removed-after-court-order-in-magnet-suit/ （确认原始博文地址 ps.tc/pages/blog-usbliter8.html 与仓库 prdgmshift/usbliter8 已下架）
[^2^]: The Apple Wiki, "usbliter8 Exploit", 更新至 2026-09-30. https://theapplewiki.com/wiki/Usbliter8_Exploit （SecureROM 版本范围、RP2040 成功率低、诉讼与存档链接）
[^3^]: CourtListener, Magnet Forensics, LLC v. Del Gaudio, 1:26-cv-03781 (N.D. Ga.), 2026-07-21 命令原文. https://www.courtlistener.com/docket/73584326/magnet-forensics-llc-v-del-gaudio/
[^4^]: Pasquale Pillitteri (EN), "usbliter8: Unpatchable BootROM Flaw Hits iPhone XS, XR and 11", 2026-09-06. https://pasqualepillitteri.it/en/news/6725/usbliter8-unpatchable-bootrom-iphone-xs-xr-11
[^5^]: hackyboiz（韩文技术分析）, "usbliter8: Apple A12/A13, S4/S5 SoC BootROM 취약점", 2026-06-27. https://hackyboiz.github.io/2026/06/27/ji9umi/usbliter8/
[^6^]: Mezha, "Paradigm Shift disclosed usbliter8 exploit that could jailbreak iPhones", 2026-06-22. https://mezha.net/eng/news/1d2041c4_paradigm_shift_disclosed/
[^7^]: iRemove Tools, "USBLiter8 BootROM Exploit Explained", 2026-07-04. https://iremove.tools/blog/usbliter8-bootrom-exploit-guide
[^8^]: 至顶网（TechCrunch 中译）, "Apple芯片现不可修复漏洞", 2026-07-20. https://m.zhiding.cn/article/3193526.htm
[^9^]: Feedsta, "Researchers Uncover Unpatchable BootROM Exploit Targeting Apple's A12 and A13 Chips", 2026-07-19. https://feedsta.ai/blog/apple-a12-a13-bootrom-exploit-usbliter8/
[^10^]: Elcomsoft Blog, "A12 usbliter8 BootROM sigpatches", 2026-07-15. https://blog.elcomsoft.com/2026/07/a12-usbliter8-bootrom-sigpatches/
[^11^]: ONE Jailbreak, "usbliter8-fun: iOS 27 Jailbreak on iPhone 11 Pro", 2026-07-25. https://onejailbreak.com/blog/usbliter8-fun-ios-27-jailbreak-on-iphone-11-pro/
[^12^]: GitHub - 34306/usbliter8-fun, "iOS 27 jailbreak with usbliter8 exploit", 2026-07-08 起更新. https://github.com/34306/usbliter8-fun
[^13^]: usbliter8.info, "How to Boot iPhone XR SSH Ramdisk Using USBLiter8 BootROM Exploit", 2026-07-03. https://www.usbliter8.info/how-to-boot-iphone-xr-ssh-ramdisk-by-using-usbliter8-bootrom-exploit/
[^14^]: CNX Software, "Why are people suddenly adding Apple Lightning cables to Raspberry Pi Pico 2 boards?", 2026-08-20. https://www.cnx-software.com/2026/08/20/why-are-people-suddenly-adding-apple-lightning-cables-to-raspberry-pi-pico-2-boards/
[^15^]: usbliter8.info, "Can usbliter8 be used for Untethered A12 and A13 jailbreak?", 2026-07-03. https://www.usbliter8.info/can-usbliter8-be-used-for-untethered-a12-and-a13-jailbreak/
[^16^]: r/jailbreak（Telegram 镜像）, "usbliter8: what you need to know — The iOS 17 problem", 2026-06-19. https://t.me/s/r_jailbreak?before=56488
[^17^]: xakep.ru MEGANews, "«Неисправимая» уязвимость в Apple A12 и A13", 2026-06-30. https://xakep.ru/2026/06/30/meganews-327/
[^18^]: AppleInsider, "iPhone exploit fight highlights who owns security research", 2026-07-24. https://appleinsider.com/articles/26/07/24/iphone-exploit-legal-fight-is-really-about-who-owns-security-research
[^19^]: Cult of Mac / Yahoo Tech, "usbliter8 lawsuit forces 'unpatchable' iPhone hack offline", 2026-07-27. https://tech.yahoo.com/cybersecurity/articles/usbliter8-lawsuit-forces-unpatchable-iphone-140002992.html
[^20^]: 什么值得买, "iOS 27 正式版推了……按芯片看", 2026-09-17. https://post.smzdm.com/p/a70nrpn9/
[^21^]: GBHackers, "New usbliter8 SecureROM Exploit Enables iOS 27 Jailbreak on iPhone 11 Pro", 2026-07-27. https://gbhackers.com/usbliter8-securerom-exploit-ios-27-jailbreak-on-iphone-11-pro/
[^22^]: GBHackers, "Researchers Demonstrate iOS 27.0.1 Untethered Jailbreak on iPhone 18 Pro", 2026-10-17. https://gbhackers.com/ios-27-0-1-untethered-jailbreak/
[^23^]: Beta Profiles, iOS 27.0 beta 2 = build 24A5370h（2026-06-22 发布）. https://betaprofiles.dev/ipsw/iphone-se-3rd-generation/
[^24^]: 9to5Mac, "New lawsuit alleges unpatchable Apple chip exploit was developed using stolen trade secrets", 2026-07-25. https://9to5mac.com/2026/07/24/new-lawsuit-alleges-unpatchable-apple-chip-exploit-was-developed-using-stolen-trade-secrets/
