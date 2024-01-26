# LiteLoaderQQNT - background-plugin

LiteLoaderQQNT插件，用于自动轮换QQNT的背景图片或者视频，并自带一些CSS透明度优化（参考自[LiteLoaderQQNT-Test-Theme](https://github.com/mo-jinran/test-theme)）。
使用前需要安装[LiteLoaderQQNT](https://github.com/mo-jinran/LiteLoaderQQNT)，并在QQNT新版上使用。

## 使用方法

*建议从`LiteLoaderQQNT`应用商店中直接下载安装，方便快捷。*（新版本1.0`LiteLoaderQQNT`没有插件商店了，请遵循[手动安装方法](https://liteloaderqqnt.github.io/guide/plugins.html)）

**版本不兼容提示**：从0.1.18起，插件已适配1.0版本以上`LiteLoaderQQNT`框架，同时不再兼容旧版框架，请遵循[安装方法](https://liteloaderqqnt.github.io/guide/install.html)更新框架。

**版本不兼容提示**：从0.1.24起，插件的背景视频功能需要1.0.3及以上`LiteLoaderQQNT`框架才支持，请遵循[安装方法](https://liteloaderqqnt.github.io/guide/install.html)更新框架。



也可以clone或下载zip文件解压，保留文件夹结构（文件夹名称为`插件名`，内容为github上的内容），将文件夹移动至`LiteLoaderQQNT数据目录/plugins/`下面，重启QQNT即可。

- 启动QQ后会自动写入默认配置文件到**插件目录下面的`config.json`**，然后你对配置文件做的任何修改都会被插件实时应用（详见后文）。若无必要，不建议手动修改`config.json`；
- 你还可以通过QQ设置里的背景插件设置界面对插件进行设置（推荐这种方法，更方便，也能实时应用）。

**如果出现错乱、设置修改后无法应用或无法加载背景的情况，请确认`config.json`是否被正确配置；若不能确定，可先前往设置界面恢复默认设置；如果连设置界面也进不去，那就手动删除`config.json`后重启NTQQ再试（恢复默认后无背景，请去设置背景后再看看有没有问题），若还有问题请发ISSUE。**

可搭配[MSpring主题](https://github.com/MUKAPP/LiteLoaderQQNT-MSpring-Theme)实现更佳效果哦~

默认加载背景的路径是插件目录下面的imgs文件夹，在QQ的设置里可以切换背景的目录，保存后下次更新背景时生效，目前只会读取目录同级的一些常见格式的图片或视频文件，如下：

图片：`const allowedImgExt= ["JPG", "BMP", "PNG", "APNG", "WEBP", "JPEG", "AVIF", "GIF"];  `

视频：`const allowedVideoExt = ["MP4", "WEBM", "OGG"];`

背景默认是居中适应，所以如果比例不对可能会不好看，尽量选择横着的图片吧~

**目前已支持从网络API获取图片和视频，详见下面的配置说明。**

现在深色浅色模式会根据`@media`媒体选择器自适应啦~

目前还很简陋，代码也比较粗糙，但能用！

## 配置说明

**注意：所有涉及到路径的字符串中的斜杠都是正斜杠（/）。**

*所有直接对配置文件的直接修改都会导致立即重置计时器并更新一次背景（因为无法确定你修改了哪一部分，所以只能全部重载），而在配置界面的修改只会立即应用修改的那一部分。*

imgDir（对应配置界面`本地背景文件夹路径`）：从哪个文件夹自动读取图片/视频文件，仅会读取一级的图片/视频，并不会递归读取子文件夹的哦。如果文件夹里同时有图片和视频，则都可能随机到。（特别注意：视频文件名**不能包含中文、空格和特殊字符，否则随机到这个视频时背景会空白**，图片没有这个限制）。默认：`插件目录下面的imgs文件夹`。**指定单张背景请使用imgFile参数（相较旧版本已修改）。**

imgApi（对应配置界面`网络背景链接`）：从哪个api获取网络图片或视频，可选，可以没有这一项，但背景来源为网络图片/网络视频时该项必填。从网络获取的图片/视频必须没有防盗链，可通过GET直接访问（也就是链接放到浏览器里直接就能打开看到），暂不支持JSON格式返回的API（后续将支持）。该项没有本地视频的文件名和路径限制，任意合法URL均可加载。默认：`""`。

apiType（配置界面会自动设置该项）：api返回的文件类型，是图片或者视频。可选值：img→图片；video→视频。如果选择错误将导致背景无法加载。默认：`img`。

imgFile（对应配置界面`本地背景路径`）：背景设置为哪个单独文件（可以是图片或者视频，但视频路径和文件名目前**不能包含中文、空格和特殊字符，否则无法加载**，图片没有这个限制），可选，可以没有这一项，但背景来源为文件时该项必填。默认：`""`。

imgSource（对应配置界面`修改背景来源`）：图片来源类型，是文件夹轮播，还是单个文件，还是从网络api获取图片或者获取视频。你还可以取消背景图，仅使用本插件提供的透明度。注意：如果选择从网络API获取，则必须正确选择是网络图片还是网络视频，否则背景会空白。可选值：none→无背景图；folder→文件夹；file→单个文件；network→网络图片；network_video→网络视频。默认：`folder`。

refreshTime（对应配置界面`背景更新间隔`）：多久随机更新一次背景，单位秒。默认：`600`（10分钟）。

isAutoRefresh（对应配置界面`是否自动轮播背景`）：是否自动轮播背景。若关闭，则每次NTQQ启动仅随机一次图片/视频，后续除非手动点击按钮更新，否则不再更新。默认：`true`。

enableFrostedGlassStyle（对应配置界面`是否对部分组件启用毛玻璃模糊效果`）：是否对部分组件启用毛玻璃模糊效果，若不喜欢可以关闭。你可以通过开关来对比效果，因为配置修改是实时生效的。默认：`true`。

overrideImgFile（暂无配置界面对应）：无论上面背景来源设置如何，强制使用本参数提供的背景文件路径作为背景图（可以是图片或者视频，但视频路径和文件名目前**不能包含中文、空格和特殊字符，否则无法加载**，图片没有这个限制）。这个参数是用来配合未来手动选择文件夹内某个文件作为背景使用的（目前该功能还没有实装）。默认：`""`。

apiOptions对象：用来配置来源是网络图片或者网络视频的请求。

​	useCache（对应配置界面`是否启用缓存`）：若来源是网络图片/视频，是否使用缓存。如果你设置的图片/视频地址是API（也就是每次请求图片/视频不一样），请务必设置为`false`，否则，缓存将导致图片/视频不会更新；若你设置的图片地址为单张图片/单个视频，每次请求均一样，可以设置为`true`来避免重复请求*（原理：每次请求会带上一个t=时间戳的参数，这样就能避免缓存）*。默认：`false`。

## 协议及免责

MIT | 禁止用于任何非法用途，插件开发属学习与研究目的，仅自用，未提供给任何第三方使用。任何不当使用导致的任何侵权问题责任自负。
