# 玩客云OneCloud刷ubantu或者刷dibian直刷EMCC起动
入门玩客云和各种新鲜玩法
## 「玩客云硬件参数：」

CPU是晶晨S805 ，4核心 1.5GHZ

双USB2.0

千兆单网口

1GB内存+8GB存储

这个配置平平无奇，主要的硬伤是cpu32位的，usb2.0传输速度只有20M/秒左右，下载速度跑不满千兆带宽，好的地方是千兆网口。

测试下来稳定在5万多分，功耗1瓦多，CPU占用率在 40% 左右。 最高也就2.2W CPU占用59%。

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


## docker环境搭建

国内网络连接不稳定，需要更换软件源，然后更新系统，之后才能安装docker。

- 「换Debian源」

```
nano /etc/apt/sources.list
```
进入编辑器后，粘贴下面代码

## 国内常见镜像站点

#### 请记住，除非要从源代码构建应用程序，否则可以安全地注释掉以deb-src开头的存储库，这将使用更少的数据。

- 使用说明:

以下我们用dibian11(bullseye)演示：

一般情况下，将/etc/apt/sources.list文件中Debian默认的软件仓库地址和安全更新仓库地址修改为国内的镜像地址即可，比如将deb.debian.org和security.debian.org改为mirrors.xxx.com，并使用https访问，可使用如下命令：
```
sed -i "s@http://\(deb\|security\).debian.org@https://mirrors.xxx.com@g" /etc/apt/sources.list
```
- 阿里云镜像站
```
deb https://mirrors.aliyun.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye main non-free contrib
deb https://mirrors.aliyun.com/debian-security/ bullseye-security main
deb-src https://mirrors.aliyun.com/debian-security/ bullseye-security main
deb https://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
```
- 腾讯云镜像站
```
deb https://mirrors.tencent.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.tencent.com/debian/ bullseye main non-free contrib
deb https://mirrors.tencent.com/debian-security/ bullseye-security main
deb-src https://mirrors.tencent.com/debian-security/ bullseye-security main
deb https://mirrors.tencent.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.tencent.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.tencent.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.tencent.com/debian/ bullseye-backports main non-free contrib
```
- 网易镜像站
```
deb https://mirrors.163.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.163.com/debian/ bullseye main non-free contrib
deb https://mirrors.163.com/debian-security/ bullseye-security main
deb-src https://mirrors.163.com/debian-security/ bullseye-security main
deb https://mirrors.163.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.163.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.163.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.163.com/debian/ bullseye-backports main non-free contrib
```
- 华为镜像站
```
deb https://mirrors.huaweicloud.com/debian/ bullseye main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye main non-free contrib
deb https://mirrors.huaweicloud.com/debian-security/ bullseye-security main
deb-src https://mirrors.huaweicloud.com/debian-security/ bullseye-security main
deb https://mirrors.huaweicloud.com/debian/ bullseye-updates main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye-updates main non-free contrib
deb https://mirrors.huaweicloud.com/debian/ bullseye-backports main non-free contrib
deb-src https://mirrors.huaweicloud.com/debian/ bullseye-backports main non-free contrib
```
- 清华大学镜像站
```
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-freedeb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-freedeb https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
```
- 中科大镜像站
```
deb https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
deb https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
deb https://mirrors.ustc.edu.cn/debian-security/ bullseye-security main contrib non-free
```
- 修改之后再运行apt update更新索引

```
apt-get update && apt-get upgrade
```
## 「安装docker」

```
apt install docker.io
```

等待代码跑完后，docker -v 查看下版本，证明安装成功！

## 安装OpenWrt

镜像一：
```
docker pull jyhking/onecloud:1.1
```
来源：https://www.right.com.cn/forum/thread-8024126-1-1.html

镜像二：
```
docker pull xuanaimai/onecloud:21-09-15
```
来源：https://hub.docker.com/r/xuanaimai/onecloud

镜像三：
```
docker pull 2224758988/onecloud:openwrt-1.0
```
本次安装的是镜像一，比较精简，功能够用；镜像二功能很全，根据自己需要选择。
## 「打开网卡混杂模式」
```
ip link set eth0 promisc on
```

## 「创建网络」

```
docker network create -d macvlan --subnet=192.168.3.0/24 --gateway=192.168.3.1 -o parent=eth0 macnet
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
## openwrt插件安装方法 如：frpc

下载方法：
```
wget https://github.com/kuoruan/openwrt-frp/releases/download/v0.45.0-1/frpc_0.45.0-1_arm_cortex-a5_vfpv4.ipk
wget https://github.com/kuoruan/luci-app-frpc/releases/download/v1.2.1-1/luci-app-frpc_1.2.1-1_all.ipk
wget https://github.com/kuoruan/luci-app-frpc/releases/download/v1.2.1-1/luci-i18n-frpc-zh-cn_1.2.1-1_all.ipk
```
安装方法：
```
opkg install frpc_0.45.0-1_arm_cortex-a5_vfpv4.ipk --force-depends
opkg install luci-app-frpc_1.2.1-1_all.ipk --force-depends
opkg install luci-i18n-frpc-zh-cn_1.2.1-1_all.ipk --force-depends
```
- 其他的也可以如法炮制
## 玩客云openwrt专用源推荐

- 清华大学开源软件镜像站
```
src/gz openwrt_core https://mirrors.tuna.tsinghua.edu.cn/openwrt/releases/packages-23.05/arm_cortex-a5_vfpv4/packages/
src/gz openwrt_base https://mirrors.tuna.tsinghua.edu.cn/openwrt/releases/packages-23.05/arm_cortex-a5_vfpv4/base/
src/gz openwrt_luci https://mirrors.tuna.tsinghua.edu.cn/openwrt/releases/packages-23.05/arm_cortex-a5_vfpv4/luci/
src/gz openwrt_packages https://mirrors.tuna.tsinghua.edu.cn/openwrt/releases/packages-23.05/arm_cortex-a5_vfpv4/packages/
src/gz openwrt_routing https://mirrors.tuna.tsinghua.edu.cn/openwrt/releases/packages-23.05/arm_cortex-a5_vfpv4/routing/
src/gz openwrt_telephony https://mirrors.tuna.tsinghua.edu.cn/openwrt/releases/packages-23.05/arm_cortex-a5_vfpv4/telephony/
```
- openwrt.ai比较专业和丰富的源推荐使用

```
src/gz openwrt_core https://dl.openwrt.ai/packages-23.05/arm_cortex-a5_vfpv4/packages
src/gz openwrt_base https://dl.openwrt.ai/packages-23.05/arm_cortex-a5_vfpv4/base
src/gz openwrt_packages https://dl.openwrt.ai/packages-23.05/arm_cortex-a5_vfpv4/packages
src/gz openwrt_luci https://dl.openwrt.ai/packages-23.05/arm_cortex-a5_vfpv4/luci
src/gz openwrt_routing https://dl.openwrt.ai/packages-23.05/arm_cortex-a5_vfpv4/routing
src/gz openwrt_kiddin9 https://dl.openwrt.ai/packages-23.05/arm_cortex-a5_vfpv4/kiddin9
```
## 可修改LED灯颜色 0为关闭, 1为开启
- 配置即时生效：例图【蓝色】
```
echo 0 > /sys/class/leds/onecloud:red:alive/brightness
echo 1 > /sys/class/leds/onecloud:blue:alive/brightness
echo 0 > /sys/class/leds/onecloud:green:alive/brightness
```
- 配置永久生效：例图【蓝色】
```
vi /sys/class/leds/onecloud:blue:alive/brightness
1
vi /sys/class/leds/onecloud:red:alive/brightness
0
vi /sys/class/leds/onecloud:green:alive/brightness
0 
```
