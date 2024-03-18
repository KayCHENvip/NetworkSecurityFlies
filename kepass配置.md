# 一劳永逸：KeePass 全网最详使用

## **前言：**

- 警告：请不要下载和使用任何非官方来源的 KeePass 应用程序及第三方插件。包括但不限于各种精简版，修改版，增强版，一键安装版等。**KeePass 完全免费，官方提供简体中文。**
- **本教程仅适用于 Windows，chrome 和 Android 平台**。
- 文中的下载链接均来自于 KeePass 官方网站（**如有例外会特别注明**）。
- 本文介绍了 keepass（Windows）及其 9 个插件（包括 chrome 平台）以及 Keepass2Android（安卓）的配置使用，特殊技巧和注意事项。

## **为什么写这篇教程？**

1. keepass 虽然功能强大但使用相对复杂，而且官方帮助文档是全英文，对普通用户不友好；
2. 网上所能找到的中文教程都不完整。如果说看了网上的教程去试用 keepass 你有 80% 的可能会放弃，那么看完本教程后 90% 的可能你今后不会再用除 keepass 以外的任何其他密码管理软件。

## **keepass 与同类软件相比究竟有何优势？**

- 最直观的优势当然是**完全免费**。
- 最实际的优势：今后基本可以告别手动输入账户密码。**（在此建议将所有密码更换为 16 位随机强密码）**
- 最强大的优势：这是一个开源软件，拥有众多优秀的第三方开源插件支持。即使将来有一天开发者不更新了也会有其他开发者接手。
- 最重要也是最容易被忽略的优势：keepass 的加密方式和加密算法均处于同类软件的领先水平 (至今未暴露出任何安全隐患) ，**你的数据完全掌握在自己手中**，无需将任何敏感信息托付给第三方服务商。
- 一图胜千言，来张动图感受一下 keepass 双通道自动输入混淆的逼格吧！

![img](https://pic4.zhimg.com/v2-3a7b25ab1cf10bd4a7c7f1400ebafacf_b.gif)

## **那么这篇教程能达到什么效果呢？**

1. 替你节省至少 90% 摸索学习的时间；
2. 让你在三小时之内熟练使用 keepass；
3. 学会一项受益终身的技能，**一劳永逸**。

正文开始前，先放上所有官方下载地址。注：Github 地址中下载页面中**后缀为. plgx** 的文件即可。另：无法正常访问地址的均额外提供本人的度盘分享链接。

- KeePass（Windows）：[下载地址](https://sourceforge.net/projects/keepass/)
- Keepass2Android 离线版（安卓）：[下载地址](https://play.google.com/store/apps/details?id=keepass2android.keepass2android_nonet) [度盘链接](https://pan.baidu.com/s/1gv4GSwJGgJph1tZS12OdWw)（提取密码：abd4）
- chromeIPass（chrome）：[下载地址](https://chrome.google.com/webstore/detail/ompiailgknfdndiefoaoiligalphfdae) [度盘链接](https://pan.baidu.com/s/1G4E4WwY1wQOamWrk4kf90w)（提取密码：892a）
- 简体中文语言包：[下载地址](https://sourceforge.net/projects/keepass/files/Translations 2.x/2.39/KeePass-2.39-Chinese_Simplified.zip/download?use_mirror=jaist)
- KeePassHttp：[下载地址](https://github.com/pfn/keepasshttp)
- KeeTrayTOTP：[下载地址](https://github.com/victor-rds/KeeTrayTOTP/releases)
- WebAutoType：[下载地址](https://sourceforge.net/projects/webautotype/)
- AutoTypeSearch：[下载地址](https://sourceforge.net/projects/autotypesearch/)
- KPEntryTemplates：[下载地址](https://github.com/mitchcapper/KPEntryTemplates/releases)
- KPEnhancedEntryView：[下载地址](https://sourceforge.net/projects/kpenhentryview/)
- SourceForgeUpdateChecker：[下载地址](https://sourceforge.net/projects/kpsfupdatechecker/)
- YetAnotherFaviconDownloader：[下载地址](https://github.com/navossoc/KeePass-Yet-Another-Favicon-Downloader/releases)

## **KeePass（Windows）篇**

## **（1）中文语言包安装**

1. 安装并运行 keepass（安装时默认英文，无中文可选）；
2. 点击主界面中的【View】→【Change Language】；
3. 点击【Open Folder】打开 keepass 的语言安装文件夹；
4. 解压下载的中文语言 zip 包，将解压后的文件复制并粘贴到步骤 3 打开的文件夹中；
5. 重复步骤 2，选择【Chinese_Simplified】，点击弹出框中的【是】重启 keepass。

![img](https://pic3.zhimg.com/v2-730e633d8cac203cdcb0065fcfa5d506_r.jpg)

## （**2）插件安装**

1. 在 keepaass 主界面中点击【工具】→【插件管理器】→【打开文件夹】；
2. 将下载的后缀为. plgx 的文件复制并粘贴到步骤 1 打开的的文件夹中（zip 包请先解压）；
3. 关闭然后重新打开 keepass。
4. （度盘下载的 chromeIPass 插件安装方法：点击 chrome 浏览器右上角的三个点→【更多工具】→【扩展程序】，在打开标签页的右上角启用开发者模式→将下载的 crx 文件拖动到此标签页）

![img](https://pic2.zhimg.com/v2-828c71f173f6ad3026c81f9dae678c69_r.jpg)

## **3）数据库同步和加密方式**

如果你在使用 Windows10，建议将 keepass 数据库直接存储在 OneDrive 文件夹中。因为 OneDrive 已经集成到了 Windows 文件资源管理器中，而且 OneDrive 网页版可查看**文件版本历史记录**和回收站，即使操作失误或误删文件也有补救的余地。

在创建数据库时 keepass 提供了 3 种可任意组合的加密方式：【管理密码】【密钥文件 / 提供器】和【Windows 用户账户】，也就是说一共可以组合出 8 种不同的加密方式。追求更高安全性可以了解一下 [OtpKeyProv](https://keepass.info/plugins.html#otpkeyprov)：每次打开数据库需要输入至少 3 个一次性的 6 位数安全码，但是必须要有一个 YubiKey 硬件或一台装有 Google 身份验证器的备用手机，而且这种加密方式的容错率和安全性有所冲突，本文篇幅所限不再赘述。所以综合安全性，易用性，跨平台等多方面因素，建议使用【管理密码】+【密钥文件 / 提供器】的加密方式。其中【密钥文件 / 提供器】可使用任意类型文件（图片，文档，音频，视频等），它的工作原理是使用 SHA-256 对密钥文件进行哈希处理并将生成的 32 个字节用作密钥，因此**请妥善保存密钥文件，不要随意修改**（重命名无影响）。你可以使用一张家人的照片，一首喜欢的歌或一段不可描述的视频作为密钥文件，谁又能猜得到呢？（使用 100mb 以上的文件作为密钥文件会明显影响解锁速度）

![img](https://pic3.zhimg.com/v2-af31abca9a2897b86047e472209a3b5e_r.jpg)

数据库配置的【安全】选项卡中有一个【迭代次数】，次数越高数据库越难被暴力破解，但每次打开数据库和保存记录的耗时也越长。默认值为 60000，我将它设为 500000，在 Keepass2Android（安卓）上打开时间为 5～6 秒，可供参考。

![img](https://pic4.zhimg.com/v2-99e6d687d3a5fbd5b83403596bbfe237_r.jpg)

为使你的主密码更安全，请在 keepass 的**【选项】→【安全】中勾选【在安全桌面输入管理密码】**，因为几乎没有键盘记录软件能在安全桌面工作。

![img](https://pic1.zhimg.com/v2-a1dc8f62ceb90eda641496f9dc134eb8_r.jpg)

## **（4）自动输入和双通道自动输入混淆**

自动输入顾名思义就是用软件模拟按键以代替手动输入，需要注意的是如果目标应用程序使用管理员权限运行，那么 keepass 也必须使用管理员权限运行才能在此应用程序中使用自动输入。通俗点讲，当你打开一个程序弹出了【用户账户控制】警告（UAC），那么要在这个程序中使用自动输入必须右键点击桌面上的 keepass 图标，选择【以管理员身份运行】。**自动输入全局热键默认为【Ctrl + Shift + A】**。使用方法非常简单：**点击输入框→按下自动输入全局热键；或点击输入框→切换到 keepass 主界面→右键单击记录→【执行自动输入】**。

双通道自动输入混淆不仅逼格高（具体效果如上面动图所示），而且非常强大。它的工作原理简洁明了：将要输入的字符随机分为两部分，然后使用模拟按键和复制粘贴两种方式混合输入，输入完成后再还原输入前的剪切板内容。这样一来，**单一的键盘记录软件或剪切板监听软件都无法窃取到输入的完整字段**。由于某些软件的输入框没有移动光标或不支持粘贴操作，因此这个功能**默认不启用**。必须在添加或编辑记录时点击【自动输入】，勾选【双通道自动输入混淆】。这无疑增加了普通用户的使用成本：为一条记录启用双通道自动输入混淆需要额外点击 3 次，对于有上百条密码的用户来说绝对是一场灾难。下文将给出**一劳永逸**的解决方案。

自动输入匹配规则：当按下自动输入全局热键时，keepass 会根据当前活动窗口的标题在数据库中搜索相匹配的记录；如果一条记录的标题或网址包含在活动窗口标题内那么这条记录将被匹配。举个例子：在 chrome 浏览器中打开淘宝网，当前活动窗口的标题就是 “淘宝网 - 淘！我喜欢 - Google Chrome”。那么问题来了：标题为“Google” 的记录也会被匹配到，而且在 chrome 中打开的任何标签页的标题都会包含 “Google Chrome” 字段。**因此为防止误输密码，请务必在 keepass 的【工具】→【选项】→【高级】中勾选【总是显示全局自动输入记录选取对话框】**。

![img](https://pic3.zhimg.com/v2-5150edeed8ee83c1f89d70191970805a_r.jpg)

最后一点是自动输入的键入规则，默认规则为 {USERNAME}{TAB}{PASSWORD}{ENTER}。解释：输入用户名，Tab 键（换行），输入密码，回车键。但这套规则明显不适合中文用户，因为**使用自动输入时输入法必须是英文**，否则会出现很尴尬的场面。**本文推荐使用以下规则**：+{DELAY 100}{CLEARFIELD}{USERNAME}{TAB}{PASSWORD}{ENTER}。解释：Shift 键（Windows10 输入法切换），等待 100 毫秒，清空输入框，输入用户名，Tab 键（换行），输入密码，回车键。由于新建记录默认从群组继承输入规则，所以只需修改一次即可，**一劳永逸**，具体操作见下图。**注：使用此规则自动输入时请确保输入法为中文**。

![img](https://pic3.zhimg.com/v2-a7c84dbc7b71bb40894c713a4d72be96_r.jpg)

## **（5）使用固定设置**

这是一个很特殊的技巧，可以让你在每次打开 keepass 时使用固定的应用设置，类似于网吧电脑系统的重启还原。由于 keepass 密码锁定的是数据库而不是主程序，所以在电脑未锁定的情况下任何人都可以进入 keepass 主程序并更改设置。这个技巧主要用于应对以下两个场景：1. 熊孩子乱改你的 keepass 设置；2. 别有用心的人趁你不备在 keepass 选项中更改密码导出策略，在你下次打开数据库后忘记锁定或中途离开电脑时快速导出全部数据。但是这个技巧**防小人不防高手**，而且如果 TA 恰好也看过本文，那就只是多花一分钟的事了。所以还是建议养成随手锁定电脑的好习惯（Win 键 + L），不过拿来对付家里的熊孩子绰绰有余了。

配置方法：打开 keepass 的【工具】→【选项】，设定好你需要设置→【确定】；打开 Windows 文件资源管理器，点击选项卡【查看】，勾选【隐藏的项目】。打开文件夹 C:\Users(用户)\User Name\AppData\Roaming\KeePass，将文件夹中的 KeePass.config.xml 重命名为 KeePass.config.enforced.xml，然后将它剪切并粘贴到文件夹 C:\Program Files (x86)\KeePass Password Safe 2 即可。如需取消，请删除 KeePass.config.enforced.xml 文件。

![img](https://pic4.zhimg.com/v2-036d04d83cffd086d46b6bf3ca3480cb_r.jpg)

![img](https://pic1.zhimg.com/v2-9b37bf34f01d0e7ba842de357e058938_r.jpg)

## **6）其他概念**

替代 URL：使用指定的应用程序（浏览器）打开 URL。

TAN (transaction authentication number)：一次性验证码，通常在启用网站两步验证的同时会提供多个备用验证码（TAN）。keepass 中每条 TAN 用过后会打上╳并注明失效日期，非常实用。

- 技巧：

1. 由于 TAN 记录的标题不能自定义，因此建议为每个账户的 TAN 记录单独创建一个群组，这样不容易混淆；
2. 在添加 TAN 时勾选【序号连续，开始于】，这样创建的每条 TAN 会有一个序号，以便识别和使用。

- 添加方法：创建一个新群组（推荐）→点击群组→【工具】→【TAN 向导】。

![img](https://pic3.zhimg.com/v2-fa1b4513fc424a6ce4ed431798f9ba66_r.jpg)

## **KeePass 插件篇**

## **①KPSourceForgeUpdateChecker**

此插件的作用是检查从 SourceForge 上下载的插件的更新信息。SourceForge 是一个类似于 GitHub 的网站，本文中所有插件均来自于这两个站点。遗憾的是 keepass 主程序目前只能检查来自 GitHub 的插件的更新，这个插件刚好弥补了此不足。**需要注意的是此插件没有在 keepass 官网插件列表中列出，但是本文介绍的多个插件的开发者都建议使用此插件检查更新，而且此插件在 SourceForge 网站上 keepass 开发者参与的多个帖子中被人提及，keepass 开发者并未指出此插件有风险**。总而言之，请酌情下载和使用。

![img](https://pic2.zhimg.com/v2-32a0d6a22072fd72ec6fbdd96c135e95_r.jpg)

![img](https://pic4.zhimg.com/v2-6c827f7c982642e80d45e2c12b582033_r.jpg)

## **②KPEnhancedEntryView**

增强记录视图：提供颜值更高的查看视图，支持一键查看 / 隐藏所有加密字段（F9），安装后可在 keepass 主界面直接添加备注和附件，显著提升用户体验，可以说是必备插件。

![img](https://pic2.zhimg.com/v2-0d95b2e23b4428becf903d70d4eb79cd_r.jpg)

![img](https://pic2.zhimg.com/v2-60311890712258212af1eef92346a089_r.jpg)

为达到最佳显示效果，请按以下说明配置：

1. 在 keepass 主界面中点击【显示】→【窗口布局】→【平铺】；
2. 在 keepass 主界面中点击【显示】→【列设置】，取消勾选除【标题】以外的所有选项→【确定】。

## **③KPEntryTemplates**

更美观，更简洁，更高效，可全面定制的模板编辑器，说它是神器也不为过。当初我的一百多条账户密码全部是手动转移到 keepass 中的，顺便也把所有账户的密码都改成了随机强密码，要是没有这个插件可能要多花两到三倍的时间。没有对比就没有伤害，直接上图。

![img](https://pic4.zhimg.com/v2-4431fee71501ac00c13178f68f34fb33_r.jpg)

![img](https://pic1.zhimg.com/v2-a49cc843ef702e307a9fea8bd487f5fc_r.jpg)

配置方法：

1. 点击 keepass 主界面的【文件】→【数据库设置】→【高级】，在【模板记录组】中选择一个群组→【确定】；
2. 返回主界面，点击步骤 1 中选择的群组，按 Ctrl+I 键（或点击上方工具栏的钥匙图标）添加记录；
3. 点击【自动输入】，勾选【双通道自动输入混淆】（**以后用模板添加记录时就不需要再勾选了，一劳永逸**）；
4. 点击最左边的【Template】→【Init As Template】；
5. 配置所需模板→【确定】。
6. （排序和删除方法见下图）

![img](https://pic2.zhimg.com/v2-ddd32976aef88296f08a683577864a79_r.jpg)

创建好的模板在 Keepass2Android（安卓）也可以使用，**下文会给出 6 套现成的模板设定图，足以胜任日常使用，要是没有耐心或不熟悉英文请直接按图配置吧**！

先将 Template（模板）中的所有英文字段翻译一遍：

Title: 标题 Field: 字段 Field Name: 字段名 Type: 类型 Opt: 选项 Custom: 自定义 Username: 用户名 Password: 密码 Password Conformation: 确认密码 URL: 网址 Notes: 备注 Override URL: 替代 URL Expiry Date: 到期日 Inline: 文本编辑框 Inline URL: 文本网址 Popout: 弹出式文本编辑窗口 Protected Inline: 加密文本编辑框 Protected Popout: 加密弹出式文本编辑窗口 Date: 日期 Time: 时间 Date Time: 日期时间 Checkbox: 复选框 Divider: 分割器 Listbox: 列表框 RichTexbox: 富文本框

**这么一大堆看上去容易头晕但需要记住的只有两个：Protected Inline 和 Listbox**。Protected Inline 用于存储支付密码等需要加密的字段；Listbox 则省去了每次添加记录都要重复输入邮箱或手机号的烦恼，配置方法如下图。**注意：分隔符是英文字符中的逗号**。

![img](https://pic1.zhimg.com/v2-86d38c460cebe9b8a03868ae795c87ac_r.jpg)

那么设置好的模板要怎么用呢？

1. 【添加记录】→【Template】→【Set Template Parent】；
2. 见下图。

![img](https://pic4.zhimg.com/v2-458e1ef4b6577d08ff2c47e2cd9770b3_r.jpg)

**6 套常用模板配置图：**

![img](https://pic4.zhimg.com/v2-c2bfde21e782db7e991aad4db9a695c3_r.jpg)

![img](https://pic1.zhimg.com/v2-c12bcdf5d06ec614869b489faea27484_r.jpg)

![img](https://pic1.zhimg.com/v2-43d9e17606d8c4773068b24853e6d7d0_r.jpg)

![img](https://pic2.zhimg.com/v2-3fbb5be50dc81c707f2ad4a237246e41_r.jpg)

![img](https://pic3.zhimg.com/v2-9590baa25e7333da98035b70b048f7c6_r.jpg)

![img](https://pic2.zhimg.com/v2-32a93af286c29f54978490a6f712b3c5_r.jpg)

## **④KeePassHttp**

此插件需要配合下面的 chromeIPass 使用，形象点说它就是个传信的邮差。因为 chromeIPass 本身并不存储任何记录，所以当你在 chrome 浏览器中打开一个网页时，它就会询问 KeePassHttp 你的数据库中有没有关于这个网址的记录；如果有 KeePassHttp 会弹出一个对话框，点击【Allow】它就会将记录以 AES 256 算法加密然后通过 http 协议发送给 chromeIPass 从而实现网页自动填充。觉得每次都问很烦的话请勾选【Remember this decision】；这样还不够？那么请在 keepass 主界面中点击【工具】→【KeePassHttp Options】→【advanced】→勾选【Always allow access to entries】，这样它就再也不会问你了（不推荐）。

**进阶技巧：添加记录时去掉网址的前缀**。以百度为例：添加一条百度账户的记录时应将网址设为 https://baidu.com/ 而不是 https://www.baidu.com/ ，否则将无法在百度的一些子域名上实现自动填充，比如百度贴吧 https://tieba.baidu.com/ ，百度地图 https://map.baidu.com/ 等。不过复制粘贴我都嫌麻烦更不可能去手动输网址，还是那句话：下文将给出**一劳永逸**的解决方案。

## **⑤chromeIPass**

配置方法：点击 chromeIPass 图标→【Connect】，在弹出的对话框中输入一个便于识别的名称（比如家里的电脑，公司的电脑等）→【Save】。默认快捷键：**填充用户名 + 密码：【Ctrl + Shift + U】 仅填充密码：【Ctrl + Shift + P】**。这个插件除了能自动填充用户名密码外，还可以在数据库中新建记录和更新已有记录：当你在 keepass 数据库中未记录的网站上注册或登录时，或是在一个已有记录的网站上使用新的用户名或密码登录时，chromeIPass 图标会变成一把红色的小锁并持续闪烁，点击图标即可新建记录或更新已有记录（新建记录存储在 KeePassHttp Passwords 群组中）。

![img](https://pic2.zhimg.com/v2-eedd624d140ab85d16bd80892679a485_b.jpg)

chromeIPass 的工作方式类似于 chrome 内置密码管理器，但需要注意的是它无法识别是否成功登录，也就是说登录网站时不小心输错用户名或密码 chromeIPass 图标也会闪烁。注：出现用户名或密码输入框识别不准确的情况可点击插件图标→【Choose own credential fields for this page】，然后自定义页面中的用户名输入框和密码输入框。**警告：请尽量不要使用此插件的密码生成功能，因为它在将密码复制到剪切板后无法自动清除。**关闭方法：点击 chromeIPass 图标→【Settings】→取消勾选页面中的【Activate password generator】。

使用此插件修改网站密码的技巧：点击 chromeIPass 图标→【Settings】→**取消勾选页面中的【Automatically fill-in single credentials entry】**→打开网站的密码修改页面→按 Ctrl + Shift + P 填入原密码→在 keepass 主程序中修改此网站记录的密码→切换到 chrome 其他标签页→切换回密码修改页面→按 Ctrl + Shift + P 填入修改后的新密码。原理：这个技巧利用的是 chromeIPass 不存储记录的特性，每切换一次标签页 chromeIPass 就会让 KeePassHttp 检索一次 keepass 数据库。

## **⑥KeeTrayTOTP**

TOTP 是一种基于时间的一次性密码算法。绝大多数网站的两步验证都使用了这种算法，例如国外的谷歌，微软，亚马逊，国内的小米，163 邮箱等。

TOTP 两步验证的优势和劣势同样明显：

- 优点：大大提高了账户安全性，不需要再忍受短信验证码的延迟；
- 缺点：过于依赖移动设备，如果不小心卸载两步验证 app，手机出故障或丢失就无法访问账户。需要输验证代码时手机没电了也很让人抓狂。

所以**这个插件绝对是一个名副其实的神器，装上后手机上的 Google 身份验证器，Microsoft Authenticator，小米安全令牌，Steam 手机客户端等通通可以卸载掉**，直接可在 Windows 上的 keepass 中生成两部验证代码，而且只需要在 Windows 的 keepass 上配置一次，安卓端的 Keepass2Android 无需任何设置可直接使用。以后换电脑换手机也不需要再重新配置，真真正正的**一劳永逸**。

配置方法：设置网站两步验证时会给出一个二维码，通常在二维码下方会有一个【无法扫描条形码】（实际情况可能略有出入），点击后会显示一串密钥。复制密钥，打开 keepass，点击要添加两步验证的记录，按 **Ctrl+Shift+I** 打开 KeeTrayTOTP 设置页面，将复制的密钥粘贴到【TOTP Seed】输入框中，【TOTP Format】选择 6（8 位的很少见，通常都是采用 6 位两步验证代码），点击【Finish】。

![img](https://pic3.zhimg.com/v2-fd3b74daa4b0fb283850b0ff11d5afe6_b.jpg)

使用方法：点击记录，按 Ctrl+T（或右键单击→Copy TOTP）即可将两步验证代码复制到剪切板；安卓端 Keepass2Android 中记录里的 TOTP 加密字段就是两步验证代码。

接下来说两个奇葩网站两步验证的设置方法：一个是小米：只能扫码，不给密钥；另一个是 Steam：码都不给你扫，必须下载手机客户端。

- 小米的很好解决，随便用一个二维码扫描软件就能获取密钥（TOTP Seed），下图中红色方框部分就是密钥。

![img](https://pic3.zhimg.com/v2-f6a5b8f2f762e7b6d3c640ad1e8e8e32_r.jpg)

- Steam 的稍微复杂一点，如果你的手机没有 root 请跳过这部分。

1. 下载并安装 [Steam 手机客户端](https://support.steampowered.com/kb_article.php?ref=4440-RTUI-9218)，设置好 Steam 令牌；
2. 使用已获取 root 权限的文件管理器（推荐使用 [FX File Explorer](https://play.google.com/store/apps/details?id=nextapp.fx)）打开目录：/data/data/com.valvesoftware.android.steam.community/files/，将目录中的 Steamguard 文件以文本方式打开，红色方框部分即为密钥（见下图）；
3. 复制密钥到【TOTP Seed】中，**【TOTP Format】选择【Steam】，点击【Finish】，然后点击记录，按 Ctrl+T 将两步验证代码复制到任意文本框，确认与手机端 steam 令牌显示的代码一致后即可卸载 Steam 手机客户端。**

![img](https://pic4.zhimg.com/v2-7829028ef8c7850d2bdcffc2ab09f1db_r.jpg)

## **⑦YetAnotherFaviconDownloader**

这个插件的作用是根据网址为数据库中的记录下载网页图标，可一次为多条记录下载。用法：在 keepass 主界面选中一条或多条记录→单击右键→【Download Favicons】。至于为什么叫 YetAnother 呢？因为它有个前辈叫 [FaviconDownloader](https://keepass.info/plugins.html#faviconimp)，可以一次为全部记录下载图标，但是用起来比较糟心，下载多了容易卡死。

## **⑧AutoTypeSearch**

上文中解释了 keepass 的自动输入匹配规则，但这个规则并不尽如人意，因为网站的标题不是一成不变的，尤其是电商网站的标题经常因为各种活动而发生变化。此时这个插件的作用就体现出来了：当按下全局自动输入热键没有匹配到记录时，它会弹出一个搜索框，搜索到所需记录后点击即可使用自动输入。同时还可以为 AutoTypeSearch 设置一个全局搜索热键：在 keepass 主界面中点击【工具】→【选项】→【AutoTypeSearch】→勾选【Show when system-wide hot key is pressed:】→点击下方输入框→按下你想使用的全局热键。

![img](https://pic4.zhimg.com/v2-a79c9023802412af8f9a217c57df26d7_r.jpg)

## **⑨WebAutoType**

最后一个插件！！！这个插件有两个功能：

1. 让 keepass 的全局自动输入根据网址而不是浏览器窗口标题匹配记录。说实话真的不好用，必须为每条记录添加一次自定义规则，一百多条记录就是一百多次。有了 chromeIPass 的自动填充在 chrome 中很少能用到 keepass 的全局自动输入了，再说就算全局自动输入匹配不到记录不是还有 AutoTypeSearch 吗？
2. 真正好用是它的快速添加记录功能，也就是上文提到的**一劳永逸**的解决方案：在 keepass 主界面中点击【工具】→【WebAutoType Options】→点击【Global hot key】输入框→按下你想使用的全局热键→【OK】。随便打开一个网页，按下全局热键，弹出的添加记录窗口中标题和网址都替你输好了，这个功能真是深得我心啊！

![img](https://pic2.zhimg.com/v2-e7fd9ad58604ad7db577a67c19430a99_r.jpg)

## **Keepass2Android（安卓）篇**

这是最短的一篇，因为这个 app 简单易懂，不需要安装任何插件，该有的功能还一个不少：支持 TOTP，支持指纹解锁 / 快速解锁，自动清除剪切板，禁止应用内截图，同步数据库（联网版）。在安卓 8.0 上 Keepass2Android 已经实现了应用内自动填充，8.0 以下的系统可以使用内置键盘填充（见下图），永久告别手动输入（不得不吐槽某些反人类的银行 app，自带输入法那么烂还好意思禁用第三方输入法）。离线版同步数据库建议使用 [FolderSync](https://play.google.com/store/apps/details?id=dk.tacit.android.foldersync.lite)（同步类型设为双向），root 用户可用 [Tasker](https://play.google.com/store/apps/details?id=net.dinglisch.android.taskerm&hl=zh) 实现自动同步：检测到 keepass 数据库文件被修改 + Wifi 已连接→启用 FolderSync 同步。最后提醒：**Keepass2Android 完全免费，内置中文，千万不要在除 Google Play 以外的任何网站或应用商店下载和使用【**[联网版](https://play.google.com/store/apps/details?id=keepass2android.keepass2android)**】**。
本教程到此正式完结，感谢坚持看到这里的各位。此时你可能还有一个问题：我那么多账户密码该怎么转移到 keepass 中呢？我的想法是：**如果你打算使用 keepass，那么也是时候把所有密码都更新一遍了**。

![img](https://pic2.zhimg.com/v2-2bc89ad831b57c2e128812f2db851c79_b.gif)

## **结语：**

在此感谢 KeePass 开发者 Dominik Reichl，Keepass2Android 开发者 Philipp Crocoll 以及众多 KeePass 插件开发者的无私奉献。**特别感谢 WorkFlowy 开发团队，没有 WorkFlowy 就不会有这篇教程**。都看到这里了，不点个赞吗？

这是我的 WorkFlowy 邀请链接，通过此链接注册可获得双倍永久条目：https://workflowy.com/invite/596a4009.lnx

本教程纯文字版（不注册可查看，注册后可保存，**一劳永逸**）：https://workflowy.com/s/ISBT.wj9WmTV4g1

没有讲到的小众技巧（不定期更新，注册后可收藏，**一劳永逸**）：https://workflowy.com/s/ISBT.t7vhV8DIvC