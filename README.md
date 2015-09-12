ShadowManager - A shadowsocks manager
---

Copyright MagicNAT 2015

### Usage

ShadowManager is a lightweight, extendible script to maintaince multiple shadowsocks server with different encryption method at a time.

	usage: shadowmanager add <port> <password> <method>
	                     start
	                     stop
	                     restart
	                     status
	                     show
	                     remove <id>
	                     enovr [overrides ...]
	                     disovr [overrides ...]

commands explained:

	add:     add a server to shadowmanager handler, accepting 3 parameters, port, password and method.
	start:   start shadowmanager, no parameters required.
	stop:    stop shadowmanager, no parameters required.
	restart: restart shadowmanager, no parameters required.
	status:  show shadowmanager running status, no parameters required.
	show:    show all the servers handling, no parameters required.
	remove:  remove server with given ID. Use "show" command to show all servers, accepting 1 parameter, server ID.
	enovr:   enable one or more overrides, accepting override names as parameters (optional).
	disovr:  disable one or more overrides, accepting override names as parameters (optional).

### Overrides, Includes and Hooks

To change the way that shadowmanager act which not able to be adjust in configure file, the overrides and includes might be needed. The former one read after the shdowmanager script loaded, the latter one read before it.

There are two chars before every override. They stand for override priority. The priority are ranked from 00 to zz, later the char, higher the priority.

Official overrides list:

	00-no-root: An override to disable root check by override root check function. Useful when needs to disable root check temporary.
	10-base64-encrypted-passwd: Use base64 to store shadowsocks servers' passwords. This is useful when you need to some chars like ',' '(' or ')' in password.
	20-json-to-shadowmanager: This override provides a command 'json2manager'. It enable user to convert shadowsocks json configure files to shadowmanager configure file.
	30-generate-qr-code: Generate QR codes for servers in the shadowmanager.
	40-randpass: Add a command 'add-randpass' to shadowmanager which enable users to add a shadowsocks server with random password.
	70-time-limit: Limit the time of each shadowsocks server can use in hours. This override will hooks pre-add events so make sure you are not using some override that remove pre-add events. Cronjob will be added to check the status and remove expired accounts.
	90-pre-server-daemon: Use separated process to serve different servers. This is useful when you needs to stat the traffic of each server separately.
	90-screen-start: This override change the original implement of the 'start' command. By using this override, the shadowsocks servers will be start using screen instead of running background as daemon. You might want to use this if you wants shadowsocks logs.
	99-chinese-usage: This is an override that change the 'usage' command when the language is Chinese.
	aa-wizard: Add a wizard to shadowmanager for adding servers, removing servers etc.
	zz-interactive-mode: This makes shadowmanager run as an interactive shell. This is not recommend because it is not likely to work properly. However, this might be useful sometime.

And hooks are the function to be execute before or after certain actions. Those hooks can be define in hooks/. Some overrides might change the hooks as needed to archive some targets.

Hooks can also be add by the non-original implements. (i.e. overrides, includes, and even hooks itself!) 

	usage: hook <hooked_function>

Hook command will check if the function is exist, and execute it if exist.

To make you usages and explanations into shadowmanager, use 'add-help' and 'add-usage'. Both two of them accept inputs from stdin. The language of help can be specify as the parameter of this two commands. If the language is not specified, it will be use as the default language (shows if no preferred language provided).

	usage: echo '	<your-command>: <your-explaination>' | add-help [help-language]
	usage: echo '	<your-command>: command-name <parameters>' | add-usage [help-language]

### License

MIT License
