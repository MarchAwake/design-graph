# RockyLinux-9.2 基于 dnsmasq 搭建 DHCP Sever

## 安装依赖
```bash
sudo dnf install dnsmasq
```
## dnsmasq 相关配置

### 创建dhcp-server.conf 配置文件
```bash
cd /etc/dnsmasq.d
vim dhcp-server.conf
```

### 配置内容如下
```conf
interface=enp4s0  # 指定网络接口
dhcp-range=192.168.138.101,192.168.138.200,12h  # 指定 IP 地址范围和租期时间
domain=rockyrobot.local  # 指定域名
dhcp-option=option:router,192.168.138.1  # 指定默认路由网关
dhcp-option=option:dns-server,1.1.1.1,8.8.8.8  # 指定 DNS 服务器
```
### 修改 dnsmasq.service 文件
```bash
sudo vim /usr/lib/systemd/system/dnsmasq.service
```
### 配置内容如下，增加重启配置
```conf
[Unit]
Description=DNS caching server.
After=network.target

[Service]
ExecStart=/usr/sbin/dnsmasq
Type=forking
PIDFile=/run/dnsmasq.pid
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
