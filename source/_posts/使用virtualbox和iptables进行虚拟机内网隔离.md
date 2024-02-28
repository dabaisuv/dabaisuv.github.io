---
title: 使用virtualbox和iptables进行虚拟机内网隔离
date: 2024-02-28 14:13:40
tags:
---
**作者：dabaisuv，原文链接：https://dabaisuv.github.io/2024/02/28/使用virtualbox和iptables进行虚拟机内网隔离**
##### 背景 - 在虚拟机中使用某些工具时，有后门风险。
##### 方案 - 组建一个虚拟机内网，利用网关虚拟机来进行完全的进出网流量控制。

* 在virtualbox虚拟机设置中，给每个虚拟机分配一个内部网络网卡（内网），名称随意但要相同。
* 在virtualbox虚拟机设置中，给网关虚拟机分配一个NAT网卡（外网）。

配置如下：

不可信虚拟机1（linux）
网卡1ip：192.168.8.2（内网）

不可信虚拟机2（linux）
网卡1ip：192.168.8.3（内网）

不可信虚拟机3（linux）
网卡1ip：192.168.8.4（内网）

网关虚拟机（linux）
网卡1_ip：192.168.8.1（内网）
网卡2_ip：192.168.1.1（外网）


#### 在网关虚拟机执行：
* 在这里假设允许放行的目标ip是：1.1.1.1 ，且放行的dns服务器是：114.114.114.114.
* 先使用nmtui将网卡1的ip设置为静态ip，ip地址为192.168.8.1，其他不用动，保存后重连。

```bash
sudo echo 1 > /proc/sys/net/ipv4/ip_forward #开启ip转发
sudo iptables -v -n -L --line-number #可通过这个命令查看现有规则
sudo iptables -P FORWARD DROP #默认不允许转发
sudo iptables -t nat -A POSTROUTING -s 192.168.8.0/24 -j MASQUERADE #转发时更改源地址
sudo iptables -A FORWARD -d 114.114.114.114 -j ACCEPT #允许目标地址114.114.114.114的转发（这是国内的dns服务器，你可以改成自己想要的）
sudo iptables -A FORWARD -d 192.168.8.0/24 -j ACCEPT #允许目标地址为192.168.8.0/24网段的转发
sudo iptables -A FORWARD -d 1.1.1.1 -j ACCEPT #允许目标地址1.1.1.1的转发（这里可以改为自己想让虚拟机能够访问的外网ip）
```
* 基本上网关虚拟机就配置好了，后续根据自己的需要添加，删除或者更改目标。
* 上面的命令是实时生效的，但是重启就会丢失，可以使用以下命令保存和恢复iptables配置
```bash
sudo iptables-save -f /etc/iptables.conf #这个保存地址可以更改为自己喜欢的
sudo iptables-restore < /etc/iptables.conf #从之前保存的配置文件中恢复
```

#### 在不可信虚拟机执行(这里拿不可信虚拟机1举例，大同小异)：
* 先使用nmtui将网卡1的ip设置为静态ip，ip地址为192.168.8.2，其他不用动，保存后重连。

```bash
sudo route add default gw 192.168.8.1 #设置默认路由为网关虚拟机
```



#### 参考
* https://wooyun.js.org/drops/Iptables%E5%85%A5%E9%97%A8%E6%95%99%E7%A8%8B.html
* https://wx.comake.online/doc/l71stn6d8shjct-SSD268/customer/development/software/m6-ipc/data.html
* https://tonydeng.github.io/sdn-handbook/linux/iptables.html
* https://www.cnblogs.com/paul8339/p/14688156.html#_label0