进入终端界面，切换为root身份,查看路由表：
#route
```
Destination Gateway Genmask Flags Metric Ref Use Iface
default　　10.122.163.254　　0.0.0.0　　UG　　200　　0　　0　　ens33
default　　192.168.1.1　　0.0.0.0　　UG　　600　　0　　0　　wlx640980616015
10.1.162.0　　0.0.0.0　　255.255.255.0　　U　　100　　0　　0　　ens33
192.168.1.0     0.0.0.0         255.255.255.0   U     600    0        0 wlx640980616015
```
两块网卡都连接上时，会产生两个默认路由，所以默认使用第一个默认路由，只能访问内网，第二个默认路由没有用，外网无法访问，要访问外网，就要关闭内网，留下第二个默认路由，并从此路由访问，如果要内外网都在线，并能各自走自己的路由，那就非常完美了，为此我们要删除掉内网默认路由，并配置一个内网访问时走的路由，在终端输入：
```
#route del -net default netmask 0.0.0.0 dev ens33
#route add -net 10.0.0.0 netmask 255.0.0.0 gw 10.122.163.254 dev ens33
10.0.0.0 : 访问的ip为10.所有的地址  
255.0.0.0 子网掩码 ps:都是255.0.0.0
gw 10.1.162.1 网关
dev ens33 指定的网卡
```
第一条语句是删除掉默认内网的路由，第二条语句添加10打头的网段（内网）都走此路由，重启网络服务：
```
#systemctl restart networking.service
```
ps:即可实现内外网皆可访问，但这样的修改在操作系统重启之后，就又会还原为以前的路由状态，
# 自动开机启动设置：
ubuntu设置
```
把上面两条语句放到/etc/rc.local中，实现启动时就修改路由
```
deepin设置：
```
#systemctl enable NetworkManager-dispatcher.service
然后在/etc/NetworkManager/dispatcher.d/目录下新建一个脚本文件02myroutes，内容如下：
#!/bin/bash
route del -net default netmask 0.0.0.0 dev ens33
route add -net 10.0.0.0 netmask 255.0.0.0 gw 10.1.162.1 dev ens33
保存后，重启系统验证成功，我想只要通过NetworkManager管理网络的linux系统都可以照此法设置。
```
