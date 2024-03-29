# mtproxy

MTProxyTLS一键安装绿色脚本



## 交流群组

Telegram群组：



## 安装方式

执行如下代码进行安装

```bash
mkdir /home/mtproxy && cd /home/mtproxy
curl -s -o mtproxy.sh https://raw.githubusercontent.com/xiaoyaohanyue/mtproxy/master/mtproxy.sh && chmod +x mtproxy.sh && bash mtproxy.sh
```

 ## 白名单 MTProxy Docker 镜像
The image integrates nginx and mtproxy+tls to disguise traffic, and uses a whitelist mode to deal with firewall detection.

该镜像集成了nginx、mtproxy+tls 实现对流量的伪装，并采用**白名单**模式来应对防火墙的检测。

 ```bash
apt update && apt install curl -y
curl -fsSL https://get.docker.com | bash
systemctl enable docker
systemctl start docker
secret=$(head -c 16 /dev/urandom | xxd -ps)
domain="azure.microsoft.com"
tag="d93f6793f50ee70dc6a70129dc31b781"
docker run --name nginx-mtproxy -d -e tag="$tag" -e secret="$secret" -e domain="$domain" -p 8080:80 -p 8443:443 xiaoyaohanyue/nginx-mtproxy:latest
 ```
镜像默认开启了 IP 段白名单，如果你不需要可以取消：

```bash
docker run --name nginx-mtproxy -d -e secret="$secret" -e domain="$domain" -e ip_white_list="IP" -p 8080:80 -p 8443:443 xiaoyaohanyue/nginx-mtproxy:latest
```

```ip_white_list``` 可选参数为:

```IP``` 允许单个 IP 访问

```IPSEG``` 允许 IP 段访问

```OFF ``` 允许所有 IP 访问



## 使用方式

whitelist

默认所有访客都不被允许连接，只有当访客尝试访问了下面的地址，才会将访客IP加入到白名单中。

IP 和端口取决于你 docker 的配置：

```
http://ip/add.php
```

运行服务

```bash
bash mtproxy.sh start
```

调试运行

```bash
bash mtproxy.sh debug
```

停止服务

```bash
bash mtproxy.sh stop
```

重启服务

```bash
bash mtproxy.sh restart
```



## 卸载安装

因为是绿色版卸载极其简单，直接删除所在目录即可。

```bash
rm -rf /home/mtproxy
```



## 开机启动

开机启动脚本，如果你的rc.local文件不存在请检查开机自启服务。

通过编辑文件`/etc/rc.local`将如下代码加入到开机自启脚本中：

```bash
cd /home/mtproxy && bash mtproxy.sh start > /dev/null 2>&1 &
```

