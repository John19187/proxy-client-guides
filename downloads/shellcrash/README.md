# 2026年 ShellCrash安装与配置教程：SSH菜单导入、启动和更新

<a id="shellcrash安装与配置教程ssh菜单导入启动和更新"></a>

<a id="2026年-shellcrash-安装与使用教程linux路由器导入配置及更新"></a>

ShellCrash通过Linux或兼容路由器的SSH交互菜单管理代理内核和配置。它是管理脚本，不是固件或LuCI插件。下面区分固定1.9.4归档和随master变化的在线安装器，按设备支持的路线安装后再运行crash。

<a id="packages"></a>

<a id="固定归档和在线安装方式"></a>

## ShellCrash 1.9.4固定归档与在线安装方式

下表是 1.9.4 管理脚本归档；它不是固件，也不是可以上传到 LuCI 的 IPK／APK。下面的在线安装路线使用开发者 master 安装器，内容随master更新。

| 支持环境 | 管理脚本归档 |
|---|---|
| 兼容Linux／OpenWrt Shell环境 | [ShellCrash.tar.gz](https://github.com/juewuy/ShellCrash/releases/download/1.9.4/ShellCrash.tar.gz) |

**归档不分 CPU，所有设备都能用吗？** 归档是管理脚本，运行内核另选 CPU。系统命令、权限、启动机制和防火墙也要兼容。

## 确认系统与恢复入口

ShellCrash 用脚本管理代理内核、配置及服务运行。设备要具备项目支持的 Linux／路由器环境、SSH 管理权限以及所需启动与防火墙能力；记录系统版本、CPU 架构和现有包管理器后，再按该设备的安装方式继续。

下载需要 curl 或 wget，透明代理还受 iptables／nftables 等条件影响。OpenWrt 的 opkg／apk 与其他 Linux 包管理器不能混为一种系统。只有电脑浏览器需要代理时，也可从[桌面客户端目录](../../README.md#desktop)选择图形 App，不必因此改路由器。

路由器先把固件配置备份下载到电脑，保存原 DNS、网关与代理分流值，保留有线管理电脑和可用 SSH 会话。普通 Linux 主机则保存当前配置，记下将改变的服务与网络设置。后续恢复靠这些具体入口，关闭终端窗口本身不会停止已经运行的服务。

## 下载并运行安装脚本

在设备的 SSH 会话先下载脚本。已有 curl 时使用：

~~~sh
curl -fL https://raw.githubusercontent.com/juewuy/ShellCrash/master/install.sh -o /tmp/shellcrash-install.sh
~~~

只有 wget 时，改用这一条下载命令，不必两条都执行：

~~~sh
wget -O /tmp/shellcrash-install.sh https://raw.githubusercontent.com/juewuy/ShellCrash/master/install.sh
~~~

下载完成并确认是预期脚本后，路由器按项目 Shell 路线运行：

~~~sh
sh /tmp/shellcrash-install.sh
~~~

标准 Linux 按项目路线使用 bash：

~~~sh
bash /tmp/shellcrash-install.sh
~~~

安装器询问目录及运行方式时，依据该设备的空间和支持条件选择。下载失败、文件是错误网页、依赖或启动机制报错时，先解决该错误，不继续运行不完整文件。

[![2026年 ShellCrash 安装与使用教程：Linux、路由器导入配置及更新封面](../../assets/images/shellcrash-2026-guide-cover.webp)](../../assets/images/shellcrash-2026-guide-cover.webp)

## 打开 crash 菜单

安装完成，在当前会话加载环境后打开交互菜单：

~~~sh
. /etc/profile
crash
~~~

**找不到 crash 怎么办？** 先加载 profile 或重连 SSH，让安装器注册的命令进入当前会话，再看安装是否真正完成。命令可用后，`crash -h` 可以查看帮助，不用重复运行来历不明的安装器。

菜单编号会随版本变化，按当前显示的功能名称操作。ShellCrash 管理的是所选内核和配置，菜单出现只是管理入口准备完成。

<a id="import"></a>

## 按内核选择配置

先在 crash 菜单选择内核，并按 CPU 下载对应文件。Mihomo 读取 Clash／Mihomo YAML，sing-box 读取与其版本匹配的 JSON；这些文件同时安排服务器与流量规则，单节点分享链接或服务 App 的账户不能直接替代。

若还在选择接入方式，可先比较[VPN 服务自带 App 与通用客户端](https://github.com/John19187/The-40-Best-VPNs#翻墙客户端与使用方式怎么选)。决定在路由器或 Linux 使用完整配置后，再从[机场推荐帖的兼容条件](https://github.com/John19187/ji-chang-tui-jian#airport-clients)核对 YAML／JSON 格式及路由器使用限制；已有来源直接继续。

内核准备好后，在 `crash` 交互菜单完成配置与启动：

1. 选择配置导入功能。手里有远程地址时，填写本人完整配置 URL；只有本地文件时，选择本地导入方式并给出实际文件位置。
2. 等待下载与解析结果。解析失败先记下字段错误，核对所选内核和配置版本；输入差异见[订阅与完整配置](../../subscription-formats.md#format-table)。
3. 配置加载正常后，返回主菜单选择启动服务，再按下面的设备用途测试。

<a id="connect"></a>

## 测试实际设备

路由器部署先让一台终端进入测试范围，用它访问目标网页和应用，同时保留有线管理及局域网可达。Linux 主机则按当前运行模式，用实际需要代理的本机应用访问目标。菜单的运行状态说明服务已启动，访问结果才回答本次用途是否完成。

[![路由器网络诊断、终端网页访问与有线管理的关系](../../assets/images/router-and-device-check.webp)](../../assets/images/router-and-device-check.webp)

这是检查关系说明图。LuCI 网络诊断只对应具备该后台的路由器；普通 Linux 仍通过 SSH 管理，并在本机应用验证用途。

服务运行而终端不通，先看接管范围、规则与防火墙，再看目标连接错误。多个节点只在同一网络失败时，检查该网络的认证和网关，核对本家公告；根据日志定位后，每次只调整对应的一项设置。

<a id="update"></a>

## 脚本、内核和配置分别在何时更新

服务调整线路时刷新原配置；协议或配置字段兼容变化时处理内核；管理与安装功能变化时才更新 ShellCrash 脚本。更新前保留现有配置和设置，让旧版本与新版本之间有可恢复的资料。

内核和配置匹配、实际应用完成访问后，日常可以继续使用这组文件。只更新发生变化的对象，更容易理解更新改变了什么，也便于需要时退回。

<a id="troubleshooting"></a>

## 从 crash 停止服务并恢复网络

1. 从保留的 SSH 会话运行 `crash`，在交互菜单选择停止服务。
2. 新配置出错时，通过配置导入功能换回已保存的原配置；改过 DNS、路由或分流时，只撤回本次新增的对应设置。
3. 确认普通联网和原管理入口恢复，再决定是否继续处理具体错误。

停止服务不需要先删除安装目录。关闭终端窗口也不会代替停止服务。

安装下载失败看设备网络、证书和下载工具，配置解析失败看所选内核与字段，启动后管理受影响则从保留的 SSH／有线入口停止并恢复。保留具体错误，可以继续处理对应环节，不必为了退出一次部署重置整台设备。

<details>
<summary>来源注释与开发者资料</summary>

- [ShellCrash 原仓](https://github.com/juewuy/ShellCrash)
- [官方中文安装说明、系统与依赖](https://github.com/juewuy/ShellCrash/blob/master/README_CN.md)
- [1.9.4 固定脚本归档](https://github.com/juewuy/ShellCrash/releases/tag/1.9.4)

版本、商店要求和引用材料的整理日期：2026-11-14。

</details>

## 文档更新记录


<details>
<summary>展开文档更新记录</summary>

- 2026-11-16　折叠更新记录，收起许可说明，撤下不必要的维护数据与贡献提示入口。 下载区标题标明本文固定版本或独立商店渠道，保原下载文件与旧锚。

- 2026-11-16　补充文末客户端与免费节点目录、文档维护入口；整理图源与版本说明，保留原安装包和操作内容。

</details>

## 客户端与相关资源目录

按设备和配置类型进入对应教程；旧版条目用于识别与迁移。软件之间的订阅、配置和备份条件仍以各篇说明为准。

<details>
<summary>展开客户端目录（含旧版与迁移说明）</summary>

| 客户端 | 设备或范围 | 教程用途 |
|---|---|---|
| **ShellCrash（本篇）** | Linux／路由器 | 安装脚本与运行条件 |
| [Clash Verge Rev](../clash-verge-rev/README.md) | Windows／macOS／Linux | Clash／Mihomo配置与系统代理 |
| [FlClash](../flclash/README.md) | 电脑／Android | 跨平台下载、导入与连接 |
| [Clash Party](../clash-party/README.md) | Windows／macOS／Linux | 安装版、便携版与订阅管理 |
| [Clash Mi](../clash-mi/README.md) | 跨平台 | 按本款入口导入Clash配置 |
| [ClashX Meta](../clashx-meta/README.md) | macOS | 菜单栏客户端与Mihomo配置 |
| [v2rayN](../v2rayn/README.md) | 电脑 | 节点订阅与系统代理 |
| [Hiddify](../hiddify/README.md) | 电脑／手机 | 跨平台导入与连接 |
| [Karing](../karing/README.md) | 电脑／手机 | 配置、规则与备份 |
| [Clash Meta for Android](../cmfa/README.md) | Android | Clash配置与VPN授权 |
| [v2rayNG](../v2rayng/README.md) | Android | 节点订阅与VPN连接 |
| [NekoBox for Android](../nekobox/README.md) | Android历史版本 | 1.4.2旧版订阅与迁移 |
| [Surfboard](../surfboard/README.md) | Android | 兼容配置与启动连接 |
| [sing-box](../sing-box/README.md) | 多平台 | 版本匹配的JSON配置 |
| [Shadowrocket](../shadowrocket/README.md) | iPhone／iPad | 节点、订阅与VPN连接 |
| [Stash](../stash/README.md) | Apple设备 | 按本款条件导入Clash类配置 |
| [Quantumult X](../quantumult-x/README.md) | iPhone／iPad | 节点资源与完整配置的区别 |
| [Loon](../loon/README.md) | iPhone／iPad | 订阅、策略与分流 |
| [Surge](../surge/README.md) | iOS／macOS | 按平台配置与接管流量 |
| [OpenClash](../openclash/README.md) | OpenWrt | 固件条件、配置与插件 |
| [PassWall](../passwall/README.md) | OpenWrt | 订阅节点与主开关 |
| [Nextin专用客户端](../provider-app/README.md) | 按服务公开支持范围 | 仅适用于已证实的服务条件 |
| [Clash for Windows](../cfw/README.md) | 旧版／迁移 | 识别旧配置并迁移 |
| [Clash for Android](../cfa/README.md) | Android旧版／迁移 | 原版CFA与CMFA分开 |
| [ClashX](../clashx/README.md) | macOS旧版 | 普通ClashX身份与迁移 |
| [ClashX Pro](../clashx-pro/README.md) | macOS历史版本 | Pro托管配置与增强模式 |
| [旧Clash Verge](../old-clash-verge/README.md) | 旧版／迁移 | 与Rev区分及迁移 |
| [NekoRay](../nekoray/README.md) | 电脑旧版／迁移 | 旧资料与替代客户端 |
| [旧客户端迁移目录](../legacy/README.md) | 旧版资料 | 按原客户端寻找迁移说明 |

</details>

| 相关文档 | 内容 |
|---|---|
| [VPN推荐与使用方式](https://github.com/John19187/The-40-Best-VPNs) | 比较服务的平台、接入方式与客户端 |
| [机场推荐与套餐比较](https://github.com/John19187/ji-chang-tui-jian) | 比较月付、按量与套餐条件 |
| **2026年机场通用客户端汇总与下载教程（本文档仓）** | 按设备选软件并进入具体教程 |
| [机场与代理工具科普指南](https://github.com/John19187/ssr-v2ray-trojan-clash-guide) | 理解机场、协议、客户端、订阅与线路 |
| [免费节点与订阅资源](https://github.com/John19187/v2ray-SSR-Clash-Verge-Shadowrocke) | 查看公开资源与导入说明，不承诺持续可用 |

<a id="文档维护与许可"></a>

<details>
<summary>开源维护与许可</summary>

本文由John19187统一整理维护，实际修改记录见本页更新日志。原创文档采用[MIT许可](../../LICENSE)；软件、商标及第三方材料的权利与使用范围见[素材说明](../../NOTICE)。

本仓整理客户端下载与使用文档，不是客户端官方项目；软件是否开源、收费及其许可由各开发者分别决定。

</details>
