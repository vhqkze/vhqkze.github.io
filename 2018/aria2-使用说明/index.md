# Aria2 使用说明

## 外部链接
- [aria2 官网](https://aria2.github.io/)
- [aria2 github地址](https://github.com/aria2/aria2)
- [Aria2 & YAAW 使用说明](https://aria2c.com/usage.html)

## Aria2 安装
- 方式一，直接从源中安装
  ```bash
  sudo apt install aria2
  ```
- 方式二，编译安装

## Aria2 配置说明
首先为 aria2 创建配置存放的目录，新建必要文件
```bash
cd ~
mkdir ./.aria2
cd ./.aria2
touch aria2.conf
touch aria2.log
touch aria2.session
```
编辑 `aria2.conf` 文件，复制以下内容并保存
```nginx
## RPC相关设置 ##

# 允许rpc
enable-rpc=true
# 允许所有来源, web界面跨域权限需要
rpc-allow-origin-all=true
# 允许非外部访问
rpc-listen-all=true
#RPC端口, 仅当默认端口被占用时修改，默认6800
rpc-listen-port=6800
# 用户名
#rpc-user=username
# 密码
#rpc-passwd=passwd
# 设置的RPC授权令牌, v1.18.4新增功能, 取代 --rpc-user 和 --rpc-passwd 选项
rpc-secret=yourpassword
# 是否启用 RPC 服务的 SSL/TLS 加密,
# 启用加密后 RPC 服务需要使用 https 或者 wss 协议连接
#rpc-secure=true
# 在 RPC 服务中启用 SSL/TLS 加密时的证书文件,
# 使用 PEM 格式时，您必须通过 --rpc-private-key 指定私钥
#rpc-certificate=/path/to/certificate.pem
# 在 RPC 服务中启用 SSL/TLS 加密时的私钥文件
#rpc-private-key=/path/to/certificate.key


## 连接设置 ##

# 最大同时下载数(任务数), 路由建议值: 3
max-concurrent-downloads=8
# 断点续传
continue=true
# 同一服务器连接数, 添加时可指定, 默认:1
max-connection-per-server=12
# 最小文件分片大小, 添加时可指定, 取值范围1M -1024M, 默认:20M
# 假定size=10M, 文件为20MiB 则使用两个来源下载; 文件为15MiB 则使用一个来源下载
# 下载线程数上限取决于能分出多少片, 对于小文件重要
min-split-size=10M
# 单文件最大线程数, 添加时可指定，默认: 5
split=10
# 整体下载速度限制, 运行时可修改, 默认:0
max-overall-download-limit=0
# 单个任务下载速度限制, 默认:0
max-download-limit=0
# 整体上传速度限制, 运行时可修改, 默认:0
max-overall-upload-limit=102400
# 单个任务上传速度限制, 默认:0
max-upload-limit=0
# 禁用IPv6, 默认:false
#disable-ipv6=true
# 连接超时时间, 默认:60
#timeout=60
# 最大重试次数, 设置为0表示不限制重试次数, 默认:5
#max-tries=5
# 设置重试等待的秒数, 默认:0
#retry-wait=0
# 断开速度过慢的连接
#lowest-speed-limit=0
# 运行覆盖已存在文件
#allow-overwrite=true
# 验证用，需要1.16.1之后的release版本
#referer=*


## 文件保存设置 ##

# 文件保存路径, 默认为当前启动位置
dir=/home/pi/Downloads
log=/home/pi/.aria2/aria2.log
log-level=error
# 从会话文件中读取下载任务
input-file=/home/pi/.aria2/aria2.session
# 在Aria2退出时保存`错误/未完成`的下载任务到会话文件
save-session=/home/pi/.aria2/aria2.session
# 定时保存会话, 0为退出时才保存, 需1.16.1以上版本, 默认:0
save-session-interval=60

# 启用磁盘缓存, 0为禁用缓存, 需1.16以上版本, 默认:16M
#disk-cache=32M
# 文件预分配, 能有效降低文件碎片, 提高磁盘性能. 缺点是预分配时间较长
# 所需时间 none < falloc ? trunc << prealloc, falloc和trunc需要文件系统和内核支持
# NTFS建议使用falloc, EXT3/4建议trunc, MAC 下需要注释此项
file-allocation=trunc


## bt下载相关 ##

# 本地节点查找, PT需要禁用, 默认:false
bt-enable-lpd=true
# 添加额外的tracker
#bt-tracker=<URI>,…
# 单个种子最大连接数，默认55
bt-max-peers=180
# 每个种子限速, 对少种的PT很有用, 默认:50K
#bt-request-peer-speed-limit=50K
# 强制加密, 防迅雷必备
bt-require-crypto=true
# 当下载的文件是一个种子(以.torrent结尾)时, 自动下载BT
follow-torrent=true
# BT监听端口, 当端口屏蔽时使用，默认:6881-6999
#listen-port=6881-6999
# 打开DHT功能, PT需要禁用, 默认:true
enable-dht=true
# 打开IPv6 DHT功能, PT需要禁用
#enable-dht6=false
# DHT网络监听端口, 默认:6881-6999
#dht-listen-port=6881-6999

# 本地节点查找, PT需要禁用, 默认:false
bt-enable-lpd=true
# 种子交换, PT需要禁用, 默认:true
enable-peer-exchange=true

# 客户端伪装, PT需要
# user-agent=Transmission/2.77
# peer-id-prefix=-TR2770-
user-agent=uTorrent/2210(25130)
peer-id-prefix=-UT2210-

# 当种子的分享率达到这个数时, 自动停止做种, 0为一直做种, 默认:1.0
seed-ratio=0.1
# 强制保存会话, 即使任务已经完成, 默认:false
# 较新的版本开启后会在任务完成后依然保留.aria2文件
force-save=false
# BT校验相关, 默认:true
bt-hash-check-seed=true
# 继续之前的BT任务时, 无需再次校验, 默认:false
bt-seed-unverified=true
# 保存磁力链接元数据为种子文件(.torrent文件), 默认:false
bt-save-metadata=false
```

## 启动 Aria2
执行 `aria2c --conf-path=/home/pi/.aria2/aria2.conf -D` 命令，可以让aria2后台运行。
如果想要开机启动，ubuntu 下编辑 `/etc/rc.local` 文件，在 `exit 0` 前面加入下面一行
```bash
aria2c --conf-path=/home/pi/.aria2/aria2.conf -D
```
如果是在windows系统上，新建文件 `隐藏运行aria2.vbs`，复制以下内容并保存，如果需要开机启动，将`隐藏运行aria2.vbs`文件创建快捷方式，将快捷方式移动到启动文件夹里。
```vbs
CreateObject("WScript.Shell").Run "E:\Downloads\aria2\aria2c.exe --conf-path=E:\Downloads\aria2\aria2.conf",0
```

## Aria2 连接
JSON-RPC Path 默认为: `http://localhost:6800/jsonrpc`
如果提示 `Aria2 RPC 服务器错误` 按照以下方法修改
- host: 指运行 Aria2 所在机器的 `IP` 或者名字
- port: 使用 `--rpc-listen-port` 选项设置的端口, 未设置则是 `6800`
- 普通情况设置为: `http://host:port/jsonrpc`
- 使用 `--rpc-secret=xxxxxx` 选项设置为: `http://token:xxxxxx@host:port/jsonrpc`
- 使用 `--rpc-user=user --rpc-passwd=pwd` 选项设置为: `http://user:pwd@host:port/jsonrpc`
- 以上 JSON-RPC Path 中的 `http` 可以用 `ws` 替代, 代表使用 `WebSocket` 协议
- 当使用 `https://aria2c.com` 访问时, 需要使用 `https` 或 `wss` 协议

## 远程访问aria2
- AriaNg [github地址](https://github.com/mayswind/AriaNg)
- webui-aria2 [github地址](https://github.com/ziahamza/webui-aria2) [临时使用地址](https://ziahamza.github.io/webui-aria2/)
- YAAW [github地址](https://github.com/binux/yaaw) [临时使用地址](http://binux.github.io/yaaw/demo/)
- YAAW中文版 [github地址](https://github.com/aa65535/yaaw-zh-hans) [http版](http://aria2c.com/)    [https版](https://aria2c.com/)
- YAAW for Chrome [github地址](https://github.com/acgotaku/YAAW-for-Chrome) [Chrome store](https://chrome.google.com/webstore/detail/yaaw-for-chrome/dennnbdlpgjgbcjfgaohdahloollfgoc)
