1.安装windows10系统 企业版或者专业版（支持hyper-v）
注意：企业版不要安装G版（不支持hyper-v）
2.开启bios虚拟化技术
3.安装docker报错
```
Hardware assisted virtualization and data execution protection must be enabled …
```
4.cmd管理员运行
```
dism.exe /Online /Enable-Feature:Microsoft-Hyper-V /All
```
和
```
bcdedit /set hypervisorlaunchtype auto
```
5.关闭docker自启动
```
1)我的电脑->服务->hyper-v虚拟机管理 改为手动
2)任务管理器中->启动 把docker改为禁用
```
pc:
彻底关闭hpyer-v
```
bcdedit /set hypervisorlaunchtype off 
```
打开hpyer-v
```
bcdedit / set hypervisorlaunchtype auto
```
