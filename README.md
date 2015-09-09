ShadowManager - A shadowsocks manager
---

Copyright MagicNAT 2015

### Usage

ShadowManager is a script to maintaince multiple shadowsocks server with different encryption method at a time.

	usage: shadowmanager add <port> <password> <method>
	                     start
	                     stop
	                     restart
	                     show
	                     remove <id>

commands explained:

	add:     add a server to shadowmanager handler, accepting 3 parameters, port, password and method.
	start:   start shadowmanager, no parameters required.
	stop:    stop shadowmanager, no parameters required.
	restart: restart shadowmanager, no parameters required.
	show:    show all the servers handling, no parameters required.
	remove:  remove server with given ID. Use "show" command to show all servers, accepting 1 parameter, server ID.

### Overrides & Includes

To change the way that shadowmanager act which not able to be adjust in configure file, the overrides and includes might be needed. The former one read after the shdowmanager script loaded, the latter one read before it. 

The two chars before every override. They stand for override priority. The priority are ranked from 00 to zz, later the char, higher the priority.

Official overrides list:

	00-no-root: An override to disable root check by override root check function. Useful when needs to disable root check temporary. 
	10-base64-encrypted-passwd: Use base64 to store shadowsocks servers' passwords. This is useful when you need to some chars like ',' '(' or ')' in password.
	90-pre-server-daemon: Use separated process to serve different servers. This is useful when you needs to stat the traffic of each server separately. 
	90-screen-start: This override change the original implement of the 'start' command. By using this override, the shadowsocks servers will be start using screen instead of running background as daemon. You might want to use this if you wants shadowsocks logs.
	99-chinese-usage: This is an override that change the 'usage' command when the language is Chinese.
	zz-interactive-mode: This makes shadowmanager run as an interactive shell. This is not recommend because it is not likely to work properly. However, this might be useful sometime. 

### License

MIT License
