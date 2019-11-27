```
1.安装hostapd命令：sudo apt-get install hostapd
```
```
2.安装dnsmasq命令：sudo apt-get install dnsmasq
```
```
3.安装iptables?命令：sudo apt-get install iptables4.下载create_ap，
ps:地址：https://github.com/oblique/create_ap?，下载zip
```
```
4.在create_ap-master文件夹下打开终端执行命令：
sudo make install
```
```
5.创建wifi热点命令：create_ap wlan0 eth0 username password
ps:wlan0是你要作为开启热点的网卡名。(有ip的)
   eth0是你连接到internet的网卡名。
   username 是创建的wifi名，password是密码。
```
用ifconfig命令查看电脑的网卡信息。?
