---
title: VirtualBox
date: 2020-10-04 21:16:32
categories: daily
tags: virtual box
---

# VirtualBox

## 为 CentOS7 安装增强功能

CentOS Server 无法从【设备】--【安装增强功能】菜单为其安装增强功能，可以手动安装。

* 安装依赖包

  ```shell
  [root@localhost ~]# yum install -y gcc gcc-c++ make kernel-headers kernel-devel
  [root@localhost ~]# reboot
  ```

* 从菜单中挂载`VBoxGuestAdditions.iso`

  确保正确设置光驱

  <img src="https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202012/120454.png" alt="image-20201205120454258" style="zoom:50%;" />

  ![image-20201205120535941](/Users/xianglin/Library/Application Support/typora-user-images/image-20201205120535941.png)
  
* 挂载光驱

  ```shell
  # 创建挂载点
  [root@localhost ~]# mkdir /media/cdrom
  # 挂载光驱
  [root@localhost ~]# mount /dev/sr0 /media/cdrom
  mount: /dev/sr0 写保护，将以只读方式挂载
  
  [root@localhost ~]# ll /media/cdrom/
  总用量 46826
  -r--r--r--. 1 root root      763 2月  20 2020 AUTORUN.INF
  -r-xr-xr-x. 1 root root     6384 10月 15 22:42 autorun.sh
  dr-xr-xr-x. 2 root root      792 10月 15 22:48 cert
  dr-xr-xr-x. 2 root root     1824 10月 15 22:48 NT3x
  dr-xr-xr-x. 2 root root     2652 10月 15 22:48 OS2
  -r-xr-xr-x. 1 root root     4821 10月 15 22:42 runasroot.sh
  -r--r--r--. 1 root root      547 10月 15 22:48 TRANS.TBL
  -r--r--r--. 1 root root  3830063 10月 15 22:41 VBoxDarwinAdditions.pkg
  -r-xr-xr-x. 1 root root     3949 10月 15 22:41 VBoxDarwinAdditionsUninstall.tool
  -r-xr-xr-x. 1 root root  7413172 10月 15 22:42 VBoxLinuxAdditions.run
  -r--r--r--. 1 root root  9401856 10月 15 23:41 VBoxSolarisAdditions.pkg
  -r-xr-xr-x. 1 root root 16950792 10月 15 22:45 VBoxWindowsAdditions-amd64.exe
  -r-xr-xr-x. 1 root root   270616 10月 15 22:42 VBoxWindowsAdditions.exe
  -r-xr-xr-x. 1 root root 10057608 10月 15 22:43 VBoxWindowsAdditions-x86.exe
  ```

* 执行安装命令

  ```shell
  [root@localhost cdrom]# sh ./VBoxLinuxAdditions.run
  Verifying archive integrity... All good.
  Uncompressing VirtualBox 6.1.16 Guest Additions for Linux........
  VirtualBox Guest Additions installer
  Removing installed version 6.1.16 of VirtualBox Guest Additions...
  Copying additional installer modules ...
  Installing additional modules ...
  VirtualBox Guest Additions: Starting.
  VirtualBox Guest Additions: Building the VirtualBox Guest Additions kernel
  modules.  This may take a while.
  VirtualBox Guest Additions: To build modules for other installed kernels, run
  VirtualBox Guest Additions:   /sbin/rcvboxadd quicksetup <version>
  VirtualBox Guest Additions: or
  VirtualBox Guest Additions:   /sbin/rcvboxadd quicksetup all
  VirtualBox Guest Additions: Building the modules for kernel
  3.10.0-1160.6.1.el7.x86_64.
  VirtualBox Guest Additions: Running kernel modules will not be replaced until
  the system is restarted
  
  [root@localhost ~]# reboot
  ```

## 为 CentOS7 的`Host-Only`网络指定静态 IP

* 查看 `host-only`的网卡

  ```shell
  # 使用 ip addr 同理
  [root@localhost ~]# ifconfig
  enp0s3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
          inet 10.0.2.15  netmask 255.255.255.0  broadcast 10.0.2.255
  
  enp0s8: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
          inet 192.168.56.106  netmask 255.255.255.0  broadcast 192.168.56.255
  
  lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
          inet 127.0.0.1  netmask 255.0.0.0
  ```

* 编辑位于`/etc/sysconfig/network-scripts`的配置文件

  ```shell
  [root@localhost ~]# vim /etc/sysconfig/network-scripts/ifcfg-enp0s8
  ```

* 修改为如下配置

  ```shell
  TYPE=Ethernet
  PROXY_METHOD=none
  BROWSER_ONLY=no
  # 设置为静态 IP
  BOOTPROTO=static
  DEFROUTE=yes
  IPV4_FAILURE_FATAL=no
  IPV6INIT=yes
  IPV6_AUTOCONF=yes
  IPV6_DEFROUTE=yes
  IPV6_FAILURE_FATAL=no
  IPV6_ADDR_GEN_MODE=stable-privacy
  NAME=enp0s8
  UUID=76b90704-1ecd-4c8e-a411-9c776cb7834d
  DEVICE=enp0s8
  # 设置开启启动
  ONBOOT=yes
  
  # 添加静态 IP 项
  IPADDR=192.168.56.106
  NETMASK=255.255.255.0
  GATEWAY=192.168.56.1
  ```

* 可以在`/etc/sysconfig/network`中添加 DNS 配置

  ```shell
  # Created by anaconda
  DNS1=192.168.56.1
  DNS2=8.8.8.8
  ```

* 重启网络配置

  ```shell
  [root@localhost ~]# service network restart
  Restarting network (via systemctl):                        [  确定  ]
  ```

  

## VBoxManage 基础命令

### `VBoxManage list`

* 列出已安装的虚拟机

  ```shell
  ➜  ~ VBoxManage list vms
  "fedora" {b9daec39-7e85-485f-969b-629fa52bd549}
  "centos" {49074fd5-386b-42ca-9e25-af29f5e42f1f}
  ```

* 列出正在运行的虚拟机

  ```shell
  ➜  ~ VBoxManage list runningvms
  "centos" {49074fd5-386b-42ca-9e25-af29f5e42f1f}
  ```

### `VBoxManage startvm`

* 命令格式

  ```shell
  ➜  ~ VBoxManage startvm
  Usage:
  
  VBoxManage startvm          <uuid|vmname>...
                              [--type gui|headless|separate]
                              [-E|--putenv <NAME>[=<VALUE>]]
  ```

* 以窗口模式启动虚拟机

  ```shell
  ➜  ~ VBoxManage startvm fedora --type  gui
  Waiting for VM "fedora" to power on...
  VM "fedora" has been successfully started.
  ```

* 以无窗口模式启动虚拟机

  `Starts a VM without a window for remote display only.`

  ```shell
  ➜  ~ VBoxManage startvm centos --type headless
  ```

### `VBoxManage controlvm`

* 命令格式

  ```shell
  ➜  ~ VBoxManage controlvm
  Usage:
  
  VBoxManage controlvm        <uuid|vmname>
                              pause|resume|reset|poweroff|savestate|
                              acpipowerbutton|acpisleepbutton|
  ```

* 暂停虚拟机

  ```shell
  ➜  ~ VBoxManage controlvm fedora pause
  ```

  <img src="https://cdn.jsdelivr.net/gh/xianglin2020/gallery@master/202012/114348.png" alt="image-20201205114348345" style="zoom:50%;" />

* 恢复已暂停的虚拟机

  ```shell
  ➜  ~ VBoxManage controlvm fedora resume
  ```

* 关闭虚拟机：相当于拉闸断电

  ```shell
  ➜  ~ VBoxManage controlvm fedora poweroff
  0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
  ```

* 关闭虚拟机：正常关闭

  ```shell
  ➜  ~ VBoxManage controlvm fedora acpipowerbutton
  ```

* 休眠虚拟机

  ```shell
  ➜  ~ VBoxManage controlvm fedora savestate
  0%...10%...20%...30%...40%...50%...60%...70%...80%...90%...100%
  ```

## `VBoxManage guestproperty`

* 获取或者设置正在运行的虚拟机的属性

* 命令格式

  ```shell
  ➜  ~ VBoxManage guestproperty
  Usage:
  
  VBoxManage guestproperty    get <uuid|vmname>
                              <property> [--verbose]
  
  VBoxManage guestproperty    set <uuid|vmname>
                              <property> [<value> [--flags <flags>]]
  
  VBoxManage guestproperty    delete|unset <uuid|vmname>
                              <property>
  
  VBoxManage guestproperty    enumerate <uuid|vmname>
                              [--patterns <patterns>]
  
  VBoxManage guestproperty    wait <uuid|vmname> <patterns>
                              [--timeout <msec>] [--fail-on-timeout]
  ```

* 获取当前运行虚拟机的 IP 地址

  ```shell
  ➜  ~ VBoxManage guestproperty enumerate centos | grep "Net.*V4.*IP"
  Name: /VirtualBox/GuestInfo/Net/0/V4/IP, value: 10.0.2.15, timestamp: 1607138767607688000, flags:
  Name: /VirtualBox/GuestInfo/Net/1/V4/IP, value: 192.168.56.106, timestamp: 1607138767607908000, flags:
  
  ➜  ~ VBoxManage guestproperty get centos '/VirtualBox/GuestInfo/Net/1/V4/IP'
  Value: 192.168.56.106
  ```







