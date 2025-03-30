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
6.将
