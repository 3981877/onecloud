# onecloud
入门玩客云和各种新鲜玩法
## 「玩客云硬件参数：」

CPU是晶晨S805 ，4核心 1.5GHZ

双USB2.0

千兆单网口

1GB内存+8GB存储

这个配置平平无奇，主要的硬伤是cpu32位的，usb2.0传输速度只有20M/秒左右，下载速度跑不满千兆带宽，好的地方是千兆网口。

## 「刷机准备工具：」

WIN10电脑一台

双公头USB线一根

矿渣玩客云

螺丝刀、吹风机

## 玩客云刷机方法：
刷入方法：解压固件包，电脑连接玩客云靠近hdmi的USB口，短接刷机触点或者按住复位键的同时给玩客云通电，即可使用Aml Burn Tool软件直接烧录固件至玩客云。 

- 初始账号密码  root   1234


(特别注意：USB Burning Tool 请使用2.1.6.8版本，其他版本可能出现超时等报错，软件下载详见下述文本）


刷机包下载地址：https://github.com/hzyitc/armbian-onecloud/releases 注意后缀带Burn的才是直刷包，其他都是USB启动包。

## 两个包随便一个都可以刷 

- 一个是ubantu22.04(jammy) 

- 一个是dibian11(bullseye)

## 首次刷入系统短接方法：

- 老主板短接位置

![1](https://github.com/3981877/onecloud/assets/60610978/2c06a27f-eb7b-486e-a8ed-57b0a3d9ac35)

- 新主板短接位置

![2](https://github.com/3981877/onecloud/assets/60610978/d96e1273-f83a-4051-939a-0ec30d3c1a14)

## 拆机后、usb插入玩客云「靠近HDMI的接口」、另一头接入电脑usb、短接焊点、通电。

- 安装 Armbian 后 更新软件（非必要）

```
apt-get update && apt-get upgrade
```

## 「安装docker」

```
apt install docker.io
```

等待代码跑完后，docker -v 查看下版本，证明安装成功！

## 安装OpenWrt

镜像一：https://www.right.com.cn/forum/thread-8024126-1-1.html

镜像二：https://hub.docker.com/r/xuanaimai/onecloud

本次安装的是镜像一，比较精简，功能够用；镜像二功能很全，根据自己需要选择。
## 「打开网卡混杂模式」
```
ip link set eth0 promisc on
```

## 「创建网络」

```
docker network create -d macvlan --subnet=192.168.100.0/24 --gateway=192.168.100.1 -o parent=eth0 macnet
```

##### 自己根据 玩客云 所在网段修改，如：玩客云IP:192.168.1.175，则192.168.0.0/24 改成 192.168.1.0/24，192.168.0.1改成主路由地址

## 拉取 OpenWRT 镜像

```
docker pull jyhking/onecloud:1.1
```

## 创建容器
```
docker run -itd --name=OneCloud --restart=always --network=macnet --privileged=true jyhking/onecloud:1.1 /sbin/init
```
--name=OneCloud 其中OneCloud是容器名称，可以更改成你想要的，容器名称注意不要和其他容器一样，会冲突

openwrt镜像运行成功，然后打开路由器后台，找到openwrt地址。

## 「默认用户名：root  密码： password」

-「玩客云旁路由设置：」

- LAN口固定IP地址，网关指向主路由IP地址

- 关闭DHCP服务器

- 关闭IPV6

## 防火墙自定义规则添加下面代码:

```
iptables -t nat -I POSTROUTING eth0 -j MASQUERADE
```
