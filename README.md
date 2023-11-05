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

- 安装OpenWrt

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
- 「打开网卡混杂模式」
```
ip link set eth0 promisc on
```

- 「创建网络」

```
docker network create -d macvlan --subnet=192.168.3.0/24 --gateway=192.168.3.1 -o parent=eth0 macnet
```

- 自己根据 玩客云 所在网段修改，如：玩客云IP:192.168.1.175，则192.168.0.0/24 改成 192.168.1.0/24，192.168.0.1改成主路由地址

- 拉取 OpenWRT 镜像

```
docker pull jyhking/onecloud:1.1
```

- 创建容器
```
docker run -itd --name=OneCloud --restart=always --network=macnet --privileged=true jyhking/onecloud:1.1 /sbin/init
```
--name=OneCloud 其中OneCloud是容器名称，可以更改成你想要的，容器名称注意不要和其他容器一样，会冲突

openwrt镜像运行成功，然后打开路由器后台，找到openwrt地址。

- 「默认用户名：root  密码： password」

-「玩客云旁路由设置：」

- LAN口固定IP地址，网关指向主路由IP地址

- 关闭DHCP服务器

- 关闭IPV6

- 防火墙自定义规则添加下面代码:

```
iptables -t nat -I POSTROUTING eth0 -j MASQUERADE
```
## Docker#删除容器#删除镜像流程
```
#查看所有容器
docker ps -a
#设置容器不自启动
docker container update --restart=no [容器ID]
#停止容器
docker stop [容器ID]
#删除容器
docker rm [容器ID]
```
```
#查看所有镜像
docker images
#删除镜像
docker rmi [镜像ID]
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
src/gz openwrt_core https://dl.openwrt.ai/releases/targets/meson/meson8b/6.1.56
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
## 如何从u-boot启动？
- 从USB启动
```
setenv bootdev "usb 0"
usb start
fatload ${bootdev} 0x20800000 boot.scr && autoscr 0x20800000
```
- 从eMMC启动
```
setenv bootdev "mmc 1"
fatload ${bootdev} 0x20800000 boot.scr && autoscr 0x20800000
```
## 小米电视访问共享：
- Armbian 服务器修改配置：
```
vi /etc/samba/smb.conf
```
- 修改smb.conf文件， 注释掉in protocol=SMB2,并且添加server min protocol=NT1;
```
[global]
## fruit settings
server min protocol = NT1
#min protocol = SMB2
```

- 重启服务
```
service smbd restart
```
## 安装Casa OS 【NAS系统】
```
curl -fsSL https://get.casaos.io | sudo bash 
```
- 卸载casaos
```
casaos-uninstall
```
## 玩客云Armbian M1005打印服务器

- 安装cups
```
#查看可以使用的cups安装包
apt-cache madison cups
 cups | 2.3.3op2-3+deb11u2 | http://deb.debian.org/debian bullseye/main arm64 Packages
 cups | 2.3.3op2-3+deb11u2 | http://security.debian.org bullseye-security/main arm64 Packages
#若出现安装失败情况，请再次执行apt_install安装；
apt install cups
```
- 安装完成后修改以下服务脚本内容；
```
vi /etc/cups/cupsd.conf
```
- 修改的内容
```
 listen 0.0.0.0:631
# Restrict access to the server...
<Location />
  Order allow,deny
Allow all
</Location>
# Restrict access to the admin pages...
<Location /admin>
  Order allow,deny
Allow all
</Location>
# Restrict access to configuration files...
<Location /admin/conf>
  AuthType Default
  Require user @SYSTEM
  Order allow,deny
Allow all
</Location>
```
- 重启cups
```
systemctl restart cups	
```
- 安装完成后；打开【192.168.3.32:631】测试安装是否正常。

- cups 登录成功页面

- 输入系统账号密码；登录密码为系统账号密码； 

- 因本案例测试HP_LaserJet_M1005 为惠普厂家设备，以下代码安装HP打印机驱动
```
apt install hplip
```
## 玩客云Armbian 青龙面板

- 1、docker 安装后，直接脚本安装青龙面板
```
docker run -dit \
   -v $PWD/ql/config:/ql/config \
   -v $PWD/ql/log:/ql/log \
   -v $PWD/ql/db:/ql/db \
   -p 5700:5700 \
   --name qinglong \
   --hostname qinglong \
   --restart always \
   whyour/qinglong:latest
```

- 2、脚本安装青龙面板依赖
```
docker exec -it qinglong bash
curl -fsSL https://ghproxy.com/https://raw.githubusercontent.com/shufflewzc/QLDependency/main/Shell/QLOneKeyDependency.sh | sh
```

- 3、以下代码提供一个“测试库”更多好用的“库”各位同学自行搜寻
```
【jdpro集合库】
项目地址：https://github.com/6dylan6/jdpro.git
#国内机用下面指令（带代理）：
ql repo https://ghproxy.com/https://github.com/6dylan6/jdpro.git "jd_|jx_|jddj_" "backUp" "^jd[^_]|USER|JD|function|sendNotify"
#########################################################################
#国外机用下面指令：
ql repo https://github.com/6dylan6/jdpro.git "jd_|jx_|jddj_" "backUp" "^jd[^_]|USER|JD|function|sendNotify"
#########################################################################
```
- 4、最后一步，填写COOKIE，
```
名称：JD_COOKIE
代码：pt_key=AAJj4J5yADBEBscQUM;pt_pin=jd_737cdb; 
```
## 玩客云armbian灯光控制
- 介绍
- 玩客云刷armbian后灯光一直显示为蓝色，死机也不知道，这样灯光就是一个摆设了，所以就出现了这个脚本，控制灯光，让灯光一秒变换一次，起到一个肉眼监控的作用而不用打开ssh或是网站之类的；另外一些人认为玩客云稳定运行，不需要灯光，碍眼的可以使用关闭灯光脚本；还有可以修改定义颜色。三种模式，但是注意三者不可混用，否则会出问题。

- 灯光闪烁使用方法

- 登录ssh执行脚本：

```
wget -qO led.sh https://gitee.com/yinjiangbi_admin/wankeyunled/raw/master/led.sh && sh led.sh
```
- 改变灯光颜色使用方法

登录ssh执行脚本：
```
wget -qO changeled.sh https://gitee.com/yinjiangbi_admin/wankeyunled/raw/master/changeled.sh && sh changeled.sh
```
- 完全卸载

登录ssh执行脚本：
```
wget -qO rmled.sh https://gitee.com/yinjiangbi_admin/wankeyunled/raw/master/rmled.sh && sh rmled.sh
```
- 灯光闪烁需要vi /etc/crontab删除定时任务

## 修改固定IP和MAC地址
- 方法一：
```
armbian-config
```
- 方法二：
```
#以下两个都要改
nano /etc/network/interfaces
nano /etc/network/interfaces.default
#以上都必须需要加上这一条命令：
pre-up /sbin/ifconfig eth0 mtu 3838
```
## 如何挂载NTFS的硬盘？
```
安装ntfs-3g驱动
apt-get install ntfs-3g

 查看硬盘设备名称
lsblk

下面是挂载的命令。挂载之前请检查/mnt/casa_sda1/文件夹是否存在。
mount -t ntfs-3g -o locale=zh_CN.utf8,umask=0 /dev/sda1 /mnt/casa_sda1/

需要注意： ntfs-3g 后面有空格，然后才是 -o

- 如何开启SMB文件共享？


安装samba需要的驱动
apt-get install samba samba-common-bin avahi-daemon 

增加共享文件夹
nano /etc/samba/smb.conf

在smb.conf 末尾粘贴如下代码

security=user
[public]
comment = samba
path = /mnt/casa_sda1/
writable = yes
create mask = 0777
directory mask = 0777
browseable = yes

增加smb共享的用户（省事儿且不严谨的操作）
smbpasswd -a root

---------分割线-------

- 重启smb服务（部分机型可忽略）
systemctl restart smbd

---------------课外知识分割线---------

- 如何彻底卸载smb？

sudo apt-get remove --purge samba samba-*
sudo apt-get autoremove

- 如何卸载硬盘？

假设 挂载的路径为 /mnt/casa_sda1 ，则执行以下代码即可

umount -l /mnt/casa_sda1


手动挂载NTFS 硬盘

apt-get install ntfs-3g

mount -t ntfs-3g -o locale=zh_CN.utf8,umask=0 /dev/sda1 /mnt/casa_sda1/

```
