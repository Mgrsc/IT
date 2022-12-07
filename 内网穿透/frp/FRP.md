# FRP

https://github.com/fatedier/frp/tree/v0.45.0

## FRPS服务端配置

配置内容在frps.ini中

```bash
frps.inih
ttps://github.com/fatedier/frp/blob/master/conf/frps_full.ini
# [common] 部分不可缺少
[common]

# bind_addr 必须包含IP地址 或者 域名 主机名
# 用方括号括起来，例如 "[::1]:80", "[ipv6-host]:http" 或 "[ipv6-host%zone]:80"

bind_addr = 0.0.0.0
bind_port = 7000

# udp端口有助于 udp 内网穿透
bind_udp_port = 7001

# 用于 kcp 协议的 udp 端口，可以与'bind_port'相同
# 如果未设置，则以 frps 禁用 kcp
kcp_bind_port = 7000

# 指定要侦听的地址代理，默认值与 bind_addr 相同，127.0.0.1仅限本地、0.0.0.0为不限地址
# proxy_bind_addr = 127.0.0.1


# 如果要支持虚拟主机，则必须设置用于侦听的http端口（可选）
# vhost_http_port 参数来设置 HTTP 访问端口，此处示例：8090

# 注意：http 端口和 https 端口可以与 bind_port 相同
vhost_http_port = 80
vhost_https_port = 443

# vhost http 服务器的响应标头超时（秒），默认为60s
# vhost_http_timeout = 60

# 指定 仪表盘控制界面 Dashboard 的监听的 IP 地址 //0.0.0.0 为所以IP均可访问
# 指定 仪表盘控制界面 dashboard_port 的监听的端口，默认为 7500
dashboard_addr = 0.0.0.0
dashboard_port = 7500

# 指定访问 仪表盘控制界面 Dashboard 的登录用户名、用户密码，如果未设置，则两个默认值均为admin
dashboard_user = admin
dashboard_pwd = admin123


# dashboard assets directory(only for debug mode)
# assets_dir = ./static

# 日志文件路径
log_file = ./frps.log

# 设置日志显示级别{debug, info, warn, error)
log_level = info

# 设置日志最大天数
log_max_days = 3

# disable log colors when log_file is console, default is false
disable_log_color = false

# FRP 身份验证
token = 12345678


# heartbeat configure 不建议修改默认值
# heartbeat_timeout 的默认值为90
# heartbeat_timeout = 90



# 端口白名单
# 为了防止端口被滥用，可以手动指定允许哪些端口被使用，在 frps.ini 中通过 allow_ports 来指定：
# 只允许 frpc 绑定您列出的端口，如果不进行任何设置，则没有任何限制
# allow_ports 可以配置允许使用的某个指定端口或者是一个范围内的所有端口，以 , 分隔，指定的范围以 - 分隔。
allow_ports = 2000-3000,3001,3003,4000-50000


# 每个代理中的 pool_count 超过最大值，最大链接池,每个代理预先与后端服务器建立起指定数量的最大链接数
#  frps.ini 中设置每个代理可以创建的连接池上限，避免大量资源占用
max_pool_count = 5


# 每个客户端可以使用的最大端口数，默认值为0表示没有限制
max_ports_per_client = 0


# 如果 subdomain_host 不为空，则可以在frpc的配置文件中将类型设置为http或https时设置子域
# subdomain_host 参数来设置  Web 服务增加自定义二级域名
# 当子域时，路由使用的主机是 test.frps.com

subdomain_host = frps.com


# 如果使用 tcp 流多路复用，则默认为true
tcp_mux = true

# 定制 HTTP 请求的 404 页面
# custom_404_page = /path/to/404.html
```

![image-20221118143001093](image-20221118143001093.png)



开启服务

```bash
./frps -c frps.ini #指定配置文件
```





## FRPC客户端配置



```bash
frpc.ini
https://github.com/fatedier/frp/blob/master/conf/frpc_full.ini
# [common] is integral section
[common]
# A literal address or host name for IPv6 must be enclosed
# in square brackets, as in "[::1]:80", "[ipv6-host]:http" or "[ipv6-host%zone]:80"
# For single "server_addr" field, no need square brackets, like "server_addr = ::".
server_addr = 0.0.0.0
server_port = 7000

# The maximum amount of time a dial to server will wait for a connect to complete. Default value is 10 seconds.
# dial_server_timeout = 10

# dial_server_keepalive specifies the interval between keep-alive probes for an active network connection between frpc and frps.
# If negative, keep-alive probes are disabled.
# dial_server_keepalive = 7200

# if you want to connect frps by http proxy or socks5 proxy or ntlm proxy, you can set http_proxy here or in global environment variables
# it only works when protocol is tcp
# http_proxy = http://user:passwd@192.168.1.128:8080
# http_proxy = socks5://user:passwd@192.168.1.128:1080
# http_proxy = ntlm://user:passwd@192.168.1.128:2080

# console or real logFile path like ./frpc.log
log_file = ./frpc.log

# trace, debug, info, warn, error
log_level = info

log_max_days = 3

# disable log colors when log_file is console, default is false
disable_log_color = false

# for authentication, should be same as your frps.ini
# authenticate_heartbeats specifies whether to include authentication token in heartbeats sent to frps. By default, this value is false.
authenticate_heartbeats = false

# authenticate_new_work_conns specifies whether to include authentication token in new work connections sent to frps. By default, this value is false.
authenticate_new_work_conns = false

# auth token
token = 12345678

# oidc_client_id specifies the client ID to use to get a token in OIDC authentication if AuthenticationMethod == "oidc".
# By default, this value is "".
oidc_client_id =

# oidc_client_secret specifies the client secret to use to get a token in OIDC authentication if AuthenticationMethod == "oidc".
# By default, this value is "".
oidc_client_secret =

# oidc_audience specifies the audience of the token in OIDC authentication if AuthenticationMethod == "oidc". By default, this value is "".
oidc_audience =

# oidc_token_endpoint_url specifies the URL which implements OIDC Token Endpoint.
# It will be used to get an OIDC token if AuthenticationMethod == "oidc". By default, this value is "".
oidc_token_endpoint_url =

# oidc_additional_xxx specifies additional parameters to be sent to the OIDC Token Endpoint.
# For example, if you want to specify the "audience" parameter, you can set as follow.
# frp will add "audience=<value>" "var1=<value>" to the additional parameters.
# oidc_additional_audience = https://dev.auth.com/api/v2/
# oidc_additional_var1 = foobar

# set admin address for control frpc's action by http api such as reload
admin_addr = 127.0.0.1
admin_port = 7400
admin_user = admin
admin_pwd = admin
# Admin assets directory. By default, these assets are bundled with frpc.
# assets_dir = ./static

# connections will be established in advance, default value is zero
pool_count = 5

# if tcp stream multiplexing is used, default is true, it must be same with frps
# tcp_mux = true

# specify keep alive interval for tcp mux.
# only valid if tcp_mux is true.
# tcp_mux_keepalive_interval = 60

# your proxy name will be changed to {user}.{proxy}
user = your_name

# decide if exit program when first login failed, otherwise continuous relogin to frps
# default is true
login_fail_exit = true

# communication protocol used to connect to server
# now it supports tcp, kcp and websocket, default is tcp
protocol = tcp

# set client binding ip when connect server, default is empty.
# only when protocol = tcp or websocket, the value will be used.
connect_server_local_ip = 0.0.0.0

# if tls_enable is true, frpc will connect frps by tls
tls_enable = true

# tls_cert_file = client.crt
# tls_key_file = client.key
# tls_trusted_ca_file = ca.crt
# tls_server_name = example.com

# specify a dns server, so frpc will use this instead of default one
# dns_server = 8.8.8.8

# proxy names you want to start separated by ','
# default is empty, means all proxies
# start = ssh,dns

# heartbeat configure, it's not recommended to modify the default value
# The default value of heartbeat_interval is 10 and heartbeat_timeout is 90. Set negative value
# to disable it.
# heartbeat_interval = 30
# heartbeat_timeout = 90

# additional meta info for client
meta_var1 = 123
meta_var2 = 234

# specify udp packet size, unit is byte. If not set, the default value is 1500.
# This parameter should be same between client and server.
# It affects the udp and sudp proxy.
udp_packet_size = 1500

# include other config files for proxies.
# includes = ./confd/*.ini

# By default, frpc will connect frps with first custom byte if tls is enabled.
# If DisableCustomTLSFirstByte is true, frpc will not send that custom byte.
disable_custom_tls_first_byte = false

# Enable golang pprof handlers in admin listener.
# Admin port must be set first.
pprof_enable = false

# 'ssh' is the unique proxy name
# if user in [common] section is not empty, it will be changed to {user}.{proxy} such as 'your_name.ssh'
[ssh]
# tcp | udp | http | https | stcp | xtcp, default is tcp
type = tcp
local_ip = 127.0.0.1
local_port = 22
# limit bandwidth for this proxy, unit is KB and MB
bandwidth_limit = 1MB
# true or false, if true, messages between frps and frpc will be encrypted, default is false
use_encryption = false
# if true, message will be compressed
use_compression = false
# remote port listen by frps
remote_port = 6001
# frps will load balancing connections for proxies in same group
group = test_group
# group should have same group key
group_key = 123456
# enable health check for the backend service, it support 'tcp' and 'http' now
# frpc will connect local service's port to detect it's healthy status
health_check_type = tcp
# health check connection timeout
health_check_timeout_s = 3
# if continuous failed in 3 times, the proxy will be removed from frps
health_check_max_failed = 3
# every 10 seconds will do a health check
health_check_interval_s = 10
# additional meta info for each proxy
meta_var1 = 123
meta_var2 = 234

[ssh_random]
type = tcp
local_ip = 127.0.0.1
local_port = 22
# if remote_port is 0, frps will assign a random port for you
remote_port = 0

# if you want to expose multiple ports, add 'range:' prefix to the section name
# frpc will generate multiple proxies such as 'tcp_port_6010', 'tcp_port_6011' and so on.
[range:tcp_port]
type = tcp
local_ip = 127.0.0.1
local_port = 6010-6020,6022,6024-6028
remote_port = 6010-6020,6022,6024-6028
use_encryption = false
use_compression = false

[dns]
type = udp
local_ip = 114.114.114.114
local_port = 53
remote_port = 6002
use_encryption = false
use_compression = false

[range:udp_port]
type = udp
local_ip = 127.0.0.1
local_port = 6010-6020
remote_port = 6010-6020
use_encryption = false
use_compression = false

# Resolve your domain names to [server_addr] so you can use http://web01.yourdomain.com to browse web01 and http://web02.yourdomain.com to browse web02
[web01]
type = http
local_ip = 127.0.0.1
local_port = 80
use_encryption = false
use_compression = true
# http username and password are safety certification for http protocol
# if not set, you can access this custom_domains without certification
http_user = admin
http_pwd = admin
# if domain for frps is frps.com, then you can access [web01] proxy by URL http://web01.frps.com
subdomain = web01
custom_domains = web01.yourdomain.com
# locations is only available for http type
locations = /,/pic
# route requests to this service if http basic auto user is abc
# route_by_http_user = abc
host_header_rewrite = example.com
# params with prefix "header_" will be used to update http request headers
header_X-From-Where = frp
health_check_type = http
# frpc will send a GET http request '/status' to local http service
# http service is alive when it return 2xx http response code
health_check_url = /status
health_check_interval_s = 10
health_check_max_failed = 3
health_check_timeout_s = 3

[web02]
type = https
local_ip = 127.0.0.1
local_port = 8000
use_encryption = false
use_compression = false
subdomain = web01
custom_domains = web02.yourdomain.com
# if not empty, frpc will use proxy protocol to transfer connection info to your local service
# v1 or v2 or empty
proxy_protocol_version = v2

[plugin_unix_domain_socket]
type = tcp
remote_port = 6003
# if plugin is defined, local_ip and local_port is useless
# plugin will handle connections got from frps
plugin = unix_domain_socket
# params with prefix "plugin_" that plugin needed
plugin_unix_path = /var/run/docker.sock

[plugin_http_proxy]
type = tcp
remote_port = 6004
plugin = http_proxy
plugin_http_user = abc
plugin_http_passwd = abc

[plugin_socks5]
type = tcp
remote_port = 6005
plugin = socks5
plugin_user = abc
plugin_passwd = abc

[plugin_static_file]
type = tcp
remote_port = 6006
plugin = static_file
plugin_local_path = /var/www/blog
plugin_strip_prefix = static
plugin_http_user = abc
plugin_http_passwd = abc

[plugin_https2http]
type = https
custom_domains = test.yourdomain.com
plugin = https2http
plugin_local_addr = 127.0.0.1:80
plugin_crt_path = ./server.crt
plugin_key_path = ./server.key
plugin_host_header_rewrite = 127.0.0.1
plugin_header_X-From-Where = frp

[plugin_https2https]
type = https
custom_domains = test.yourdomain.com
plugin = https2https
plugin_local_addr = 127.0.0.1:443
plugin_crt_path = ./server.crt
plugin_key_path = ./server.key
plugin_host_header_rewrite = 127.0.0.1
plugin_header_X-From-Where = frp

[plugin_http2https]
type = http
custom_domains = test.yourdomain.com
plugin = http2https
plugin_local_addr = 127.0.0.1:443
plugin_host_header_rewrite = 127.0.0.1
plugin_header_X-From-Where = frp

[secret_tcp]
# If the type is secret tcp, remote_port is useless
# Who want to connect local port should deploy another frpc with stcp proxy and role is visitor
type = stcp
# sk used for authentication for visitors
sk = abcdefg
local_ip = 127.0.0.1
local_port = 22
use_encryption = false
use_compression = false

# user of frpc should be same in both stcp server and stcp visitor
[secret_tcp_visitor]
# frpc role visitor -> frps -> frpc role server
role = visitor
type = stcp
# the server name you want to visitor
server_name = secret_tcp
sk = abcdefg
# connect this address to visitor stcp server
bind_addr = 127.0.0.1
bind_port = 9000
use_encryption = false
use_compression = false

[p2p_tcp]
type = xtcp
sk = abcdefg
local_ip = 127.0.0.1
local_port = 22
use_encryption = false
use_compression = false

[p2p_tcp_visitor]
role = visitor
type = xtcp
server_name = p2p_tcp
sk = abcdefg
bind_addr = 127.0.0.1
bind_port = 9001
use_encryption = false
use_compression = false

[tcpmuxhttpconnect]
type = tcpmux
multiplexer = httpconnect
local_ip = 127.0.0.1
local_port = 10701
custom_domains = tunnel1
# route_by_http_user = user1
```

![image-20221130105528340](image-20221130105528340.png)

```bash
windows开启服务需要打开终端
./frpc -c frpc.ini #指定配置文件
```







## 配置systemd



```
mv frp /etc/frp
```



```bash
在/etc//etc/systemd/system/frps.service中配置

[Unit]
Description=Frp Server Service # 服务描述
After=network.target # frps 将会在 network.service 启动完毕之后再启动

[Service]
Type=simple # 不论进程是否启动成功，systemctl start 都执行成功
User=nobody # 设置进程在执行时使用的用户
Restart=on-failure # on-failure 表示仅在服务进程异常退出时重启
RestartSec=5s # 设置在重启服务前暂停多长时间
ExecStart=/etc/frp/frps -c /etc/frp/frps.ini # 在启动该服务时需要执行的命令行

[Install]
WantedBy=multi-user.target # 用于 systemctl enable 时创建软连接




systemctl start frps
```

windows中加入后台

```bat
#创建一个start.bat文件

@echo off
if "%1" == "h" goto begin
mshta vbscript:createobject("wscript.shell").run("""%~nx0"" h",0)(window.close)&&exit

:begin
frpc.exe -c frpc.ini
goto begin

shell:startup打开开机运行目录，创建快捷方式后放进去
```

![image-20221118230312931](image-20221118230312931.png)
