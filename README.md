### 项目地址

https://github.com/fatedier/frp

### 视频地址：

https://www.bilibili.com/video/BV1MXqAYREbN/

#### 给权限

```
chmod +x frps
chmod +x frpc

```

#### frps.toml

```
#【服务端口】
bindPort = 7001
#【授权码，请改成更复杂的 客户端会用到】
auth.token = "token123456"
#【服务端通过此端口接监听和接收公网用户的http请求】
vhostHTTPPort = 7002 #
#【dashboard配置】
webServer.addr = "0.0.0.0"
webServer.port = 7003
#【dashboard 用户名密码，可选，默认为空】
webServer.user = "admin"
webServer.password = "admin"

```

#### frpc.toml

```
serverAddr = "你的服务器地址"
serverPort = 7001
auth.token = "token123456"


[[proxies]]
name = "test-ssh"
type = "tcp"
local_ip = "127.0.0.1"
local_port = 22
remote_port = 6000

[[proxies]]
name = "test-docker"
type = "tcp"
local_ip = "127.0.0.1"
local_port = 9000
remote_port = 9000

```

#### 自启动命令❗️❗️❗️路径换成自己的别直接复制

```
sudo vim /lib/systemd/system/frps.service 【我这里是vim，你们用自己的编辑器自行修改】
```

`frps.service`内容如下

```
[Unit]
Description=frps
Wants=network-online.target
After=network.target network-online.target
Requires=network-online.target
[Service]
ExecStart=/home/itgoyo/frp_0.61.0_linux_amd64/frps -c /home/itgoyo/frp_0.61.0_linux_amd64/frps.toml
ExecStop=/bin/kill $MAINPID
Restart=always
RestartSec=5
StartLimitInterval=0
[Install]
WantedBy=multi-user.target
```

```
sudo vim /lib/systemd/system/frpc.service 【我这里是vim，你们用自己的编辑器自行修改】
```

`frpc.service`内容如下

```
[Unit]
Description=frpc
Wants=network-online.target
After=network.target network-online.target
Requires=network-online.target
[Service]
ExecStart=/home/itgoyo/frp_0.61.0_linux_amd64/frpc -c /home/itgoyo/frp_0.61.0_linux_amd64/frpc.toml
ExecStop=/bin/kill $MAINPID
Restart=always
RestartSec=5
StartLimitInterval=0
[Install]
WantedBy=multi-user.target
```

#### 最后4行命令

```
systemctl start frps
systemctl enable frps
systemctl start frpc
systemctl enable frpc
```


