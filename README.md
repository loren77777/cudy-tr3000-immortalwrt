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


## 准备工作：
1. Cudy TR3000 路由器：确保你手头有一台 Cudy TR3000。

2. OpenWrt 固件：下载适用于 Cudy TR3000 的 OpenWrt 固件。你可以从 OpenWrt 官方网站或immortalwrt官方网站获取固件文件。

3. 电脑：一台连接到 Cudy TR3000 的电脑。

4. 网线：赠送的网线或者六类以上网线。

5. 备份路由器数据：刷机过程会清除路由器上的所有设置，建议先备份原有的配置。

步骤一：获取OpenWrt固件
1. 下载Cudy提供的OpenWrt中间固件（https://drive.google.com/drive/folders/1BKVarlwlNxf7uJUtRhuMGUqeCa5KpMnj），该文件是“TR3000 V1.zip”。这个固件的主要目的是为了删除RSA签名检查。

2. 访问 OpenWrt 官方网站（https://openwrt.org）或ImmortalWrt官方网站（https://immortalwrt.org），在下载页面中找到 Cudy TR3000 的固件版本。请确保下载与你的路由器硬件版本匹配的固件文件，比如说Cudy TR3000 v1。注意这里只下载Sysupgrade固件就可以了。

下载Cudy TR3000 v1的immortalWrt固件
步骤二：连接路由器与电脑
1. 从前面下载的中间固件TR3000 V1.zip中提取“cudy_tr3000-v1-sysupgrade.bin”。

2. 使用网线将 Cudy TR3000 路由器与电脑连接。

3. 确保电脑的网络设置为自动获取 IP 地址（DHCP）。

4. 打开浏览器，输入192.168.10.1，访问路由器的管理界面。会提示设置管理员密码，随便输入一个好记忆的就行。高级设置中转到“升级固件”选项并使用文件“cudy_tr3000-v1-sysupgrade.bin”进行升级。等待 2 分钟。

5. 您现在使用的是中间固件。连接到以太网（OpenWrt 首次安装时不启用 Wi-Fi）并进入 LuCI （http://192.168.1.1/） 。

6. 保存mtdblock内容。系统-备份与升级-保存mtdblock内容，选择不同的block，分别保存，特别是FIP，一定要保存好，万一要恢复出厂设置要用到。

备份旅行路由器AX3000 Cudy TR3000
7. 上传固件。系统-备份与升级-刷写固件-浏览选择前面下载的OpenWrt或者ImmortalWrt的Sysupgrade固件，上传。

8. 上传成功后，路由器会自动开始刷入固件，等待约 5-10 分钟，直到指示灯停止闪烁，并且路由器正常重启。

9. 重新进入LuCI （http://192.168.1.1/），路由器就刷写好了。注意默认用户名为root，密码为空。

步骤四：安装必备软件包
1. 在 OpenWrt 的 Web 界面中，进入 System > Software。

2. 使用 OpenWrt 软件包管理器安装你需要的额外软件包，例如 DDNS、网络安全等。

步骤三：配置 OpenWrt
1. 刷机完成后，路由器会自动启动并进入 OpenWrt 系统。

3. 设置管理员密码，并根据需要配置路由器的网络设置（如无线网络、LAN、WAN 设置等）。

2. 打开浏览器，访问 192.168.1.1，你将看到 OpenWrt 的初始设置页面。

旅行路由器Cudy TR3000刷入ImmortalWrt
步骤四：安装必备软件包
1. 在 OpenWrt 的 Web 界面中，进入 System > Software。

2. 使用 OpenWrt 软件包管理器安装你需要的额外软件包，例如 DDNS、网络安全等。

步骤四：安装必备软件包
1. 在 OpenWrt 的 Web 界面中，进入 System > Software。

2. 使用 OpenWrt 软件包管理器安装你需要的额外软件包，例如 DDNS、网络安全等。

步骤五：测试
测试路由器的网络连接和性能，确保一切正常工作。

常见问题解决
1. 无法进入恢复模式：

确保按住恢复按钮的时间足够长，直到路由器指示灯开始快速闪烁。
尝试更换网线或更换电脑的网络端口。
2. 刷机失败或路由器无法启动：

尝试重新进入恢复模式，并确保上传的固件版本是正确的。
检查是否有网络连接问题。
3. 无线网络不稳定：

安装最新的 OpenWrt 固件，或者检查是否需要安装额外的驱动程序或软件包。
