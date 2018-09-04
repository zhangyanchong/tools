
https://my.vultr.com  注册一个账户  然后去买一个国外的服务器  
安装  shadowsocks   即可


Shadowsocks 及其部署
如果你理解了上面那道隐形的墙的原理，那 Shadowsocks 的原理就可以用一句简单的描述来理解了：它发出的 TCP 包，没有明显包特征，GFW 分析不出来，当作普通流量放过了。

1. 基本原理



+------+     +------+     +=====+     +------+     +-------+
| 设备  | <-> |Client| <-> | GFW | <-> |Server| <-> | 服务器 |
+------+     +------+     +=====+     +------+     +-------+
Client 和 Server 之间可以通过多种方式加密，并要求提供密码确保链路的安全性。

2. 服务器端部署

Shadowsocks 封装后对用户而言就是一个程序指令，以 Ubuntu 为例，首先安装 pip，

apt-get install python-pip  
pip install shadowsocks

注意 pip 的安装现在要求 python 版本大于等于 2.6，然后通过 pip 安装 shadowsocks。启动 shadowsocks 

使用 config 文件启动，如先配置好文件（/etc/shadowsocks.json）：

	{
	  "server": "YOUR_SERVER_IP",  
	  "server_port": 8388,  
	  "local_address": "127.0.0.1",  
	  "local_port": 1080,  
	  "password": "PASSWORD",
	  "timeout": 300,  
	  "method":"aes-256-cfb"
	}

多端口配置  

	{
	    "server":"0.0.0.0"，
	    "local_address": "127.0.0.1",
	    "local_port":1080,
	    "port_password": {
	        "8381": "passwd.1",
	        "8382": "passwd.2",
	        "8383": "passwd.3",
	        "8384": "passwd.4"
	    },
	    "timeout":300,
	    "method":"aes-256-cfb",
	    "fast_open":false,
	    "workers":5
	} 

然后通过 ssserver 启动：

ssserver -c /etc/shadowsocks.json -d start

3. 客户端配置


Mac 客户端的下载地址：


Github
墙内地址，下方右侧

配置位置：

小结
刚开始在配置 ss-serser 的时候，我遇到了些问题，本地死活代理不成功，后来通过下面这种方式调试了下：

1.客户端通过 telnet ip port 确认 ss-server 是否正常开启

如果没有正常开启，有可能是设定的端口没有开放，

iptables -A INPUT -p tcp --dport 8388 -j ACCEPT
执行上述命令，将 8388 修改为你设定的端口即可。



centos  7  
那怎么开启一个端口呢  
添加  
firewall-cmd --zone=public --add-port=80/tcp --permanent    （--permanent永久生效，没有此参数重启后失效）  
重新载入  
firewall-cmd --reload  
查看  
firewall-cmd --zone= public --query-port=80/tcp  
删除  
firewall-cmd --zone= public --remove-port=80/tcp --permanent


https://www.vultrclub.com/174.html   （加速）