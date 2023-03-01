# ax6s-openwrt

有意无意的把家里的ax6s玩死了两回了。避免后续的麻烦。我把过程记录一下。

```shell
$ md5sum miwifi_rb03_firmware_stable_1.2.7.bin
5eedf1632ac97bb5a6bb072c08603ed7  miwifi_rb03_firmware_stable_1.2.7.bin
```

因为 ax6s 刷 openwrt 要先刷 1.2.7 开发版固件，我为了方便直接拿1.2.7版固件救砖了。

如果不想刷openwrt，请去[www1.miwifi.com](https://www1.miwifi.com/miwifi_download.html)下载官方的 [Redmi路由器AX6S 稳定版
](https://cdn.cnbj1.fds.api.mi-img.com/xiaoqiang/rom/rb03/miwifi_rb03_firmware_83db5_1.0.57.bin) 来替代。

ax6s tftp+pxe 救砖：

1. 把下载的 .bin 结尾的rom scp到现有的openwrt的tftp root下，命名为 pxelinux.0 开启tftp，保存并应用
   手头没有openwrt的可以用其他的tftp服务代替。我试过pc上的dnsmasq和虚拟机里的openwrt都可行。
2. 把wan口连openwrt的lan口
3. 先关掉ax6s，用卡针戳着ax6s的reset孔开机，看到黄灯闪烁后松手
4. 几分钟后，小蓝灯会重新亮起。网线插ax6s的lan口即可使用 192.168.31.1 可以访问ax6s

刷openwrt：

因为ax6s官方固件内的 wget 不支持https协议，无法直接从官方源下载固件。

需要先在本地用 python -m http.server 起一个 http 服务。

把下载好的 [openwrt-22.03.3-mediatek-mt7622-xiaomi_redmi-router-ax6s-squashfs-factory.bin](https://openwrt.proxy.ustclug.org/releases/22.03.3/targets/mediatek/mt7622/openwrt-22.03.3-mediatek-mt7622-xiaomi_redmi-router-ax6s-squashfs-factory.bin) 重命名为 factory.bin
telnet 进入ax6s后用 wget 下载到 /tmp 目录，mtd 写入即可。

```shell
cd /tmp
wget http://<IP address of your computer>:8000/factory.bin
mtd -r write factory.bin firmware
```

参考文档:

Xiaomi Router AX6S / AX3200 OpenWRT [https://github.com/mikeeq/xiaomi_ax3200_openwrt](https://github.com/mikeeq/xiaomi_ax3200_openwrt)

[OpenWrt Wiki] Xiaomi AX3200 / Redmi AX6S [https://openwrt.org/toh/xiaomi/ax3200](https://openwrt.org/toh/xiaomi/ax3200)
