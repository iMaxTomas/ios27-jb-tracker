# Calif（@calif_io）2026-10-23 iPhone Duo 越狱演示视频调查报告

调查时间：2026-10-23 晚（UTC）。调查方式：fxtwitter API 直接读取推文 JSON、经图片代理抓取视频缩略图并逐区域放大目检、全网二手报道搜集。视频本体（video.twimg.com）在本环境被网络层阻断，未能取得 6 秒动态画面；已取得其首帧（poster frame）原图并做了目视分析。

---

## 一、两条推文的已证实事实

**推文 1**（status/2113570095549337977，2026-10-23 09:55:53 UTC）：正文只有「CC @pwnlambda」，附一段视频：媒体 ID 2113570074489675776，时长 6 秒，720x1280 竖屏，h264，另有 320x568 / 480x852 码率变体和 m3u8 流。互动数据（抓取时）：约 80.7 万次浏览、3782 赞、112 条回复、130 条引用转发[^566#0^]。

**推文 2**（status/2113578130543882697，10:27:49 UTC，引用推文 1）：
> "Just in case it wasn't clear: we popped the iPhone Duo on release day, adding a much needed feature, inspired by the php2shell hacker @lcfr_hax! What else there to hack? Wrong answers only."

互动数据：3.66 万浏览、646 赞、53 回复[^586#0^]。

## 二、视频首帧（缩略图）直接目检结果 —— 一手证据

经 images.weserv.nl 代理成功取得 pbs.twimg.com 上的视频缩略图（720x1280，即视频首帧），放大各区域后确认：

- **设备与环境**：一台**展开状态的折叠屏 iPhone**（与 iPhone Duo 外形一致：横向展开的大内屏、右上角屏下/挖孔摄像头开孔、金色边框），放在绿色切割垫上，**底部插着数据线**。手机朝向略微倾斜拍摄。
- **状态栏**：时间 **11:40**，WiFi/充电图标。
- **小组件**：天气小组件显示 **Barcelona 19°**（Calif 虽注册在 Palo Alto/加州，演示机定位巴塞罗那）；日历小组件显示 **"Fri 23"（10 月 23 日星期五，即 Duo 发售日当天）**——这是"发售日即越狱"声明的画面佐证。
- **屏幕右半部分明显被改过**：电话（角标 12）、Safari、信息、音乐四个图标**竖直堆叠在屏幕右边缘**；一个显示 "FRIDAY 23" 的日历小组件**整个旋转了约 180°（上下颠倒）**；旁边有一个**粉发动漫女孩**（红上衣、深蓝色裙子）形象的贴纸/小组件，同样呈旋转倾倒状态。
- **Dock 栏**：健康、钱包、设置（角标 4）、一个**红色底白色火箭图案的 App 图标（角标 4）**、相机。
- 主屏幕左侧仍为正常排列（FaceTime、日历、邮件、备忘录、时钟、照片、TV、News、App Store、地图等）。

**解读（推断）**：图标和小组件"向右倾倒、旋转堆叠"的视觉效果，与 Calif 在 Hexacon 演示中安装的 **Gravity Lite 类重力插件**（图标随重力坠落、旋转，见下文 Hexacon 报道中的演示截图描述）高度一致[^586#1^]。结合推文 2"adding a much needed feature"和微博镜像标题"添加了有趣的插件"[^565#0^]，视频演示的很可能是：在越狱的 Duo 上安装了一个玩笑性质的桌面插件（重力/倾倒效果），外加动漫女孩小组件和红火箭图标等彩蛋元素。**6 秒内是否有打开 App、终端、shell 等进一步动作，无法从首帧确认（未证实）。**

## 三、"much needed feature" 与 "php2shell hacker @lcfr_hax" 的梗

### @lcfr_hax 是谁（已证实）

- X 资料：名字 **"LCFR"**，简介 "0day Connoisseur @calif_io"，个人认证账号，1.3 万粉[^555#0^]。
- 多份来源确认 **@lcfr_hax 就是 Calif 研究员 Ryan Austin**，即 Calif 官方 php2shell 技术报告《php2shell: A tale of two PHP remote vulnerabilities》的作者[^553#22^][^553#1^]。
- 2026-08-05 他发过一条只有一个 SHA-256 哈希的推文（status/2085031675382071483，"d935be02…"），后作为**最先发现 PHP2Shell 的"哈希承诺"证据**，被 Calif 时间线和 X 社区注释引用[^555#0^][^553#22^]。
- 10-21 披露当天 Calif 还专门发推"澄清纪录"：@lcfr_hax 最先发现 php2shell 并写出可用 exploit，他的工作促使 PHP 基金会投资修复整个漏洞类别，之后他参与审查、反复测试补丁[^553#17^]。

### php2shell 是什么（已证实）

PHP 解释器自身的两个严重漏洞 **CVE-2026-109618（OOM 内存破坏致 RCE）/ CVE-2026-109619（陈旧全局状态致 RCE）**，CVSS v4.0 均 9.2；未经认证的攻击者通过构造 multipart 请求即可在 Apache mod_php / PHP-FPM 上以 Web 服务器权限执行任意命令，影响 PHP 5.4 以来几乎所有版本；10-21 日 PHP 紧急发布 8.5.12/8.4.27/8.3.36/8.2.35 修复，Cloudflare 等提前部署 WAF 规则，多国政府网站一度下线避险[^553#8^][^553#14^][^553#4^]。名字即"用 PHP 拿 shell"。Calif 的报告页还内嵌了两段 YouTube 演示视频[^553#22^]。

### 梗的含义（合理推断）

"inspired by the php2shell hacker @lcfr_hax" 是双关：lcfr_hax 一周前刚因"把全球 PHP 服务器变成 shell"名噪一时；Calif 自称受此"启发"给 iPhone Duo"添加了一个急需的功能"。结合首帧画面与微博"有趣的插件"描述，**最合理的解读是：他们在越狱的 Duo 主屏幕上装了一个玩笑 tweak（重力倾倒效果 + 彩蛋元素），把"越狱后能自由改系统"本身作为卖点**。另一种可能（较弱）是演示中运行了 PHP/shell 相关彩蛋（"给 iPhone 加上 PHP"），但现有画面与二手报道均未支持这一点。**该 feature 的确切名称未证实。**

### "CC @pwnlambda"（已证实/推断）

@pwnlambda 是 Calif 的 iOS 安全研究员（曾以 "Apple C1 Tales" 在 0x41con 2025 演讲）；Hexacon 演示的 Chollima 越狱安装器界面落款为 "With <3 from Calif io(@pwnlambda(显示为@lambda), @dion, @black…)"[^586#1^]。因此"CC @pwnlambda"应理解为**把 Duo 移植工作归功于这位 Chollima 团队成员**。

## 四、媒体与社区二手报道（含可信度评估）

| 来源 | 内容 | 可信度 |
|---|---|---|
| ONE Jailbreak（越狱垂直站，10-23）| "Calif.io has already jailbroken iPhone Duo on the latest firmware iOS 27.1"；Duo 出厂 27.1 build 24A94232、首日更新 24A94425（仅 Duo，机型标识 **iPhone19,4**）；@0xjohnny 检查 IPSW 后称 SPTM 绕过与 airlift 沙箱逃逸在 27.1 上仍未修补，预言"day-one untethered jailbreak"成立；27.2 预计将修补 | 较高（越狱圈专业站，引用具名研究者）[^558#2^][^562#0^] |
| 微博 @玄v烨（9.3 万粉数码博主，10-23）| **镜像转发了该视频**（t.cn/AXjGNF4W → weibo fid 1034:5357307245297786，809 次观看），标题："iPhone Duo发售当天就被破解越狱，并添加了有趣的插件" | 中（镜像真实存在，但描述仅为概括，无细节）[^565#0^] |
| 土耳其 DonanımHaber 论坛 iPhone Fold 主帖 | 引用 Calif 推文，称 Duo "duo'ya özel yayınlanan iOS 27.1'de rootlanmış"（**在 Duo 专属的 iOS 27.1 上被 root**） | 中（论坛转述，与官方推文一致）[^537#9^] |
| zone.ci/黑鸟（10-17，Hexacon 详细报道）| 10-16 演示四段：Signal 恶意 PDF RCE、iMessage "Glyph Grift"（PDF→SpringBoard→IMDPersistence→HTTPS 外发聊天记录）、**Chollima untethered 越狱**（当众重启→密码解锁→SSH 连入→uid=0 root；uname 显示 Darwin 27.0.0/XNU-13432.2.10/T8160/**iPhone19,2**/iOS 27.0.1 build 24A446；neofetch 显示 67 个 dpkg 包、开机 2 分钟）；随后安装 Sileo 与 **Gravity Lite** 插件；锁屏出现 Calif 标志和 "From Calif with LOVE" | 较高（配有现场截图，细节丰富）[^586#1^][^556#2^] |
| glitchwire（10-16）| Hexacon 演示为 untethered、无公开工具、Apple 未置评 | 中[^563#3^] |
| iClarified（10-17）| "Untethered iOS 27.0.1 Jailbreak Demonstrated on iPhone 18 Pro [Video]"：重启后仍有 root，未宣布公开工具 | 较高[^558#6^] |

注：未发现 heise、ZDNET、9to5mac、reddit r/jailbreak 对这段 6 秒视频内容的逐帧描述；也未找到引用转发区的文字索引（X 回复/引用无法通过 fxtwitter 读取，搜索引擎未收录具体讨论）。

## 五、演示性质判定

- **设备是否 iPhone Duo**：已证实（画面为展开折叠屏 + 日历显示发售日 10/23；ONE Jailbreak 与论坛均称 Duo/iPhone19,4）[^562#0^]。
- **系统是否 iOS 27.1**：已证实（二手来源一致：Duo 专属 27.1；土耳其论坛明确"在 Duo 版 iOS 27.1 上 root"；ONE Jailbreak 标题"on the latest firmware iOS 27.1"）[^537#9^][^558#2^]。
- **是否显示 root/shell**：**未在首帧显示**（首帧为主屏幕）；"已 root"来自论坛转述与 Calif 自述（"we popped"）。6 秒内是否切入终端画面未证实。
- **是否 untethered**：**合理推断为是**（Chollima 链在 Hexacon 已证明重启存活，@0xjohnny 称相关 0day 在 Duo 27.1 上未修补，ONE Jailbreak 直接称之为 day-one untethered 路径）；但这段 6 秒视频本身未展示重启过程，严格意义上"本视频证明 untethered"未证实[^562#0^]。
- **"much needed feature" 具体是什么**：未完全证实。证据指向"一个玩笑性质的主屏幕插件（重力倾倒效果，类 Gravity Lite）+ 彩蛋元素（动漫女孩小组件、红火箭图标）"；"受 php2shell 黑客启发"主要是拿自家研究员 lcfr_hax/Ryan Austin 一周前刷屏的 PHP2Shell 事件玩梗[^586#1^][^565#0^][^553#17^]。
- **视频性质**：玩梗式"发售日秒杀"宣示，而非技术演示（对比 Hexacon 的 1.5 分钟完整 SSH 演示，这次只有 6 秒主屏画面）——"What else there to hack? Wrong answers only." 也表明其娱乐定位[^586#0^]。

## 六、遗留问题

1. 6 秒视频的动态内容（动作序列、是否有声音/旁白、结尾画面）——视频字节在本环境不可达（video.twimg.com TLS 被阻断，多个公共代理均失败），需要能直连 X 的环境或已下载镜像者补充。
2. 红火箭 App 图标与动漫女孩小组件的具体所指（可能是内部玩笑/彩蛋）。
3. X 回复区（112 条）与 130 条引用转发的讨论内容未被搜索引擎收录，未能取样。
