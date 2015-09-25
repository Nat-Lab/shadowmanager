ShadowManager - 一个 Shadowsocks 管理工具
---

版权所有 MagicNAT 2015

### 用法

ShadowManager 是一个用于同时维护多个不同加密的 shadowsocks 服务器的轻量级，可扩展脚本。


	usage: shadowmanager add <port> <password> <method>
	                     start
	                     stop
	                     restart
	                     status
	                     show
	                     remove <id>
	                     enovr [overrides ...]
	                     disovr [overrides ...]

命令注解：

	add:     添加一个服务器到 shadowmanager 的管理，需要 3 个参数。端口，密码，和加密方法。
	start:   启动 shadowmanager，无需参数。
	stop:    停止 shadowmanager，无需参数
	restart: 重启 shadowmanager，无需参数。
	status:  检视 shadowmanager 状态，无需参数。
	show:    显示所有 shadowsocks 服务器，无需参数。
	remove:  移除指定 ID 的 shadowsocks 服务器，使用 "show" 来查看所有服务器，需要 1 个参数，服务器 ID。
	enovr:   启用一个或多个覆写，可将覆写名作为参数（可选）。
	disovr:  禁用一个或多个覆写，可将覆写名作为参数（可选）。

### 覆写（Overrides），包含（Includes）与钩子（Hooks）

倘若要修改那些未在 Shadowmanager 中给出选项的行为，则可能会需要覆写与包含。前一个在 shdowmanager 载入后读取，后一个在 shadowsocks 载入之前。

在每个覆写之前都有两位字符，它们所代表的是载入的优先级。优先级从 00 排列至 zz，字符越往后，优先级越高。

官方提供的覆写列表：

	00-no-root：该覆写通过替换 root 检测函数来跳过 root 检查，在需要临时关闭 root 检测时很有用。
	10-base64-encrypted-passwd：使用 base64 来储存服务器的密码。这在你需要在密码中使用特殊字符时有用。
	20-json-to-shadowmanager：该覆写提供了一个 'json2manager' 命令，可以用于将 shadowsocks json 配置文件转换为 shadowmanager 的配置文件。
	30-generate-qr-code：为 Shadowmanager 的服务器生成二维码。
	40-randpass：添加一个命令 'add-randpass' 至 shadowmanager，这个命令允许用户添加随机密码的 shadowsocks 服务器。
	50-supervisord-warp：supervisord 的映射脚本。调用此命令会启动 shadowmanager ，在接收到 SIGINT 或 SIGTERM 时，会停止 shadowmanager。
	70-time-limit：以小时限制每个 shadowsocks 服务器可以使用的时间。这个覆写会使用 pre-add 事件钩子，并添加一个 Cronjob 来检查账户并移除过期服务器。
	90-pre-server-daemon：为每个服务器使用单独的进程。在需要分开统计每个服务器的流量时有用。
	90-screen-start：这个覆写将替换 'start' 命令原本的实现，使用该覆写会让 shadowsocks 服务器在 screen 内启动，而不是作为服务启动。这在需要查看服务器日志时有用。
	99-chinese-usage：这个覆写提供了中文的帮助文本。
	aa-wizard：为 shadowmanager 的服务器添加、服务器移除等操作提供一个向导。
	zz-interactive-mode：这个覆写会使得 shadowmanager 以交互式模式启动。该方法可能会引起一些问题，故不推荐。

钩子是在特定行为执行前后运行的函数。这些钩子可以在 hooks/ 中定义。某些覆写可能会按需修改钩子来达成某些目的。

在非原版的实现中也可以定义钩子。 （例如 覆写, 包含, 甚至钩子本身！） 

	usage: hook <hooked_function>
	
Hook 命令会检测函数是否存在，若存在则会将其执行。

若想要添加您自己的命令用法与解释至 shadowmanager，您可以使用 'add-help' 与 'add-usage'。这两个命令都会从标准输入读取输入。帮助文本的语言可以在这两个命令的参数内定义。若留空，则会被作为默认语言显示（当偏好语言不能被提供时使用）。

	usage: echo '	<your-command>: <your-explaination>' | add-help [help-language]
	usage: echo '	<your-command>: command-name <parameters>' | add-usage [help-language]

### 协议

MIT License
