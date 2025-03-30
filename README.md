# cudy-tr3000-immortalwrt
cudy-tr3000-immortalwrt大分区 教程

## 介绍：
Cudy多酷TR3000是一款功能强大的便携旅行AX3000路由器，支持高速Wi-Fi 6技术，适合家庭和中小型办公室使用。然而，默认的固件可能无法满足某些高级用户的需求，因此许多人选择刷入OpenWrt固件，以便获得更多的自定义功能和控制权限。如果你也想为你的 Cudy TR3000 路由器刷入OpenWrt，这篇教程将为你提供详细的步骤。此设备硬件配置如下：

## 型号：Cudy TR3000
Wi-Fi协议：Wi-Fi 6
天线：外置2根
网口：WAN口 1× 2.5GbE，LAN口 1× 1GbE
无线速率：AX3000
覆盖面积：60-90平米左右
芯片CPU:MT7981
内存：512MB
闪存：128MB
保守带机量（仅供参考）：2.4GHz: 25台, 5GHz: 50台
定时重启:支持
IPV6:支持
VPN:支持
是否内置防火墙:支持
MESH组网:支持
2.4g频宽: 20/40MHz
5g频宽: 20/40/80/160MHz
电源规格: 5V-3A
是否内置UBOOT: 是


## 操作步骤：
1. 高级设置-固件升级 上传openwrt中间固件cudy_tr3000-v1-sysupgrade.bin
3. 刷机完成后，访问192.168.1.1，系统-备份与升级-刷写固件 上传immortalwrt固件immortalwrt-24.10.0-mediatek-filogic-cudy_tr3000-v1-squashfs-sysupgrade.bin
4. SSH设备，解锁 MTD 分区
~~~
opkg update
opkg install kmod-mtd-rw
insmod mtd-rw i_want_a_brick=1
~~~
4.写入新的 BL2 和 FIP, 将immortalwrt-24.10.0-mediatek-filogic-cudy_tr3000-v1-ubootmod-preloader.bin和write immortalwrt-24.10.0-mediatek-filogic-cudy_tr3000-v1-ubootmod-bl31-uboot.fip上传到tmp文件夹
~~~
cd /tmp
mtd write immortalwrt-24.10.0-mediatek-filogic-cudy_tr3000-v1-ubootmod-preloader.bin BL2
mtd write immortalwrt-24.10.0-mediatek-filogic-cudy_tr3000-v1-ubootmod-bl31-uboot.fip FIP
~~~
5.电脑设置静态IP
IP 地址: 192.168.1.254
子网掩码: 255.255.255.0
网关: 192.168.1.1（设备默认网关）
6.关闭电脑防火墙，将immortalwrt-mediatek-filogic-cudy_tr3000-v1-ubootmod-initramfs-recovery.itb 放到tftpd64根目录下，并打开tftpd64，拔掉电源，设备自动进入恢复模式
7.使用ssh登录路由器，vi /lib/upgrade/platform.sh，在下面这个位置添加这么一行：
~~~
cmcc,a10|\
cmcc,rax3000m|\
cudy,tr3000-v1-ubootmod|\
gatonetworks,gdsp|\
h3c,magic-nx30-pro|\
imou,lc-hx3001|\
~~~
切记不要重启路由器。
8.上传固件immortalwrt-24.10.0-mediatek-filogic-cudy_tr3000-v1-ubootmod-squashfs-sysupgrade.itb到tmp文件夹
~~~
sysupgrade -n /tmp/immortalwrt-24.10.0-mediatek-filogic-cudy_tr3000-v1-ubootmod-squashfs-sysupgrade.itb
~~~

## 安装tailscale
1.下载tailscale最新版压缩包，上传到tmp文件夹,解压缩tailscale
~~~
tar x -zvC / -f openwrt-tailscale-enabler-v1.60.0-e428948-autoupdate.tgz
~~~
2.安装依赖包
~~~
opkg update
opkg install libustream-openssl ca-bundle kmod-tun
~~~
3.运行tailscale
~~~
/etc/init.d/tailscale start
tailscale up --accept-dns=false --advertise-routes=10.0.0.0/24
~~~
4.设置开机启动，验证开机启动
~~~
/etc/init.d/tailscale enable
ls /etc/rc.d/S*tailscale*
~~~
5.获取登陆链接
~~~
tailscale up
~~~
5.开启子路由，禁用自动过期
6.添加接口
在OpenWrt上新建一个接口，协议选静态地址，设备选tailscale0，地址为Taliscale管理页面上分配的地址，掩码255.0.0.0。防火墙区域选lan区域。
7.添加防火墙规则
将以下内容，加到防火墙的自定义规则当中，并重启防火墙。
iptables -I FORWARD -i tailscale0 -j ACCEPT
iptables -I FORWARD -o tailscale0 -j ACCEPT
iptables -t nat -I POSTROUTING -o tailscale0 -j MASQUERADE
现在各个Tailscale节点之间已经可以正常互访了。

