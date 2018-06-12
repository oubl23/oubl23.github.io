---
title: qemu-kvm
date: 2018-03-25 17:32:54
categories: ffcs
tags:
---
qemu-kvm 是一款虚拟机软件，我用他来在archlinux安装linux

# qemu
## 创建磁盘
作为我安装的系统盘，命令
```sh
qemu-img create -f raw win 25G
```
* -f raw 表示硬盘模式
* win 表示硬盘存储在当前系统的文件名
* 25G 表示硬盘大小

## 桥连网卡
linux 下新建桥连网络后主机的网络连接不再由eth0负责，而是转到br0负责
### netctl方式建立
使用netctl配置的方式来建立桥连网络
```sh
# cp /etc/netctl/examples/bridge /etc/netctl/kvm-bridge 
// 复制bridge并编辑 在配置中指定需要桥连的网卡以及dhcp/static
# netctl start kvm-bridge
# netctl enabel kvm-bridge
//启动
```
### ip link 方式
使用ip linke命令临时建立网卡
[gentoo wiki上的教程](https://wiki.gentoo.org/wiki/QEMU#Host_configuration)
```sh
# ip link add br0 type bridge
# ip link set br0 up
# ip linke set eth0 master br0
```

## 安装系统
使用`qemu-system-x86_64`来创建并启动虚拟机

```sh
/usr/bin/qemu-system-x86_64  -m 4096 \
                    -enable-kvm \
                    -cdrom `isoimage.iso` \
                    -drive file=/opt/windows10/win,format=raw,index=0 \
                    -cpu host -smp cores=4 \
                    -net nic -net bridge,br=br0 \
                    -vnc :10 \
                    -nographic \
                    -usbdevice tablet
```
* -m 表示内存
* -enable-kvm 启用kvm具体效果不知
* -cdrom 指定cdrom的位置
* -drive 指定挂在硬盘的位置，生产方式见第一步
* -net 指定网卡 `-net nic -net bridge,br=br0` nic生产mac地址，bridge桥连模式指定桥连网卡br0; `-net nic -net user` nat方式联网
* -cpu 指定cpu核心
* -vnc :10 表示启用vnc :10 表示端口 5900 +10
* -nograhic 不显示 和vnc配合
* -usbdevice 使用usb驱动 其中tablet用来解决虚拟机鼠标驱动

# libvirt方式
libvirt 我把他看做是一项用于管理qemu虚拟机器的服务使用xml来保存机器数据，我喜欢libvirt能在suspend机器

## 创建机器
如下建立的参数与不细说
```sh
exec virt-install \
--connect qemu:///system \
--virt-type=kvm \
--name=win \
--ram=4096 \
--vcpus=4 \
--cdrom=/mnt/data/Downloads/windows10.iso \
--network bridge=br0 \
## nat网络 --network user \
--disk /opt/windows10/win,size=20,bus=ide,format=raw \
--disk /dev/sda1,bus=ide,format=raw \
--disk /home/xxxxx/ISO/virtio-win-0.1.141_amd64.vfd,device=floppy \
--graphics vnc,listen=0.0.0.0,port=5910 \
--input tablet,bus=usb
```

# 其他
## qemu 参数转变libvirt
可以使用如下命令进行装换
```sh
virsh domxml-from-native qemu-argv winnat.sh >> start1.xml
```
## virsh 常用命令
由于virsh需要连接 qemu 所以命令中需要加入 virsh -c qemu:///system 可以在sh中alias下

* edit 编辑xml
* suspend 停住机器
* destroy/start 关机/开机
* undefine列表中删除
* autostart 自启动

## virtio驱动
为了更好的硬件性能,使用该驱动可以解决`linux read only filesystem`问题
系统中需要事先安装好驱动不要然连硬盘都无法加载
[驱动链接](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/latest-virtio/virtio-win_x86.vfd)
