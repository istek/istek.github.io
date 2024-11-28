---
title: "PVE硬件直通之网卡直通"
date: 2024-05-20T09:28:35+08:00
type: post
category: 
 - Linux
---

硬件直通这块，因为设备不同，不能硬套如下步骤，具体要多查一下谷歌，不要人云亦云。因为我的网卡是由PCIE转接的四个I226-V网卡，按照PVE官方的步骤，打开IOMMU之后，分配了网卡，但是无法启动，后来终于从google上查到了结果。

以下硬件直通步骤仅针对PVE 8.0以上版本。

<!--more-->
## 打开IOMMU

```
nano /etc/default/grub
```
英特尔CPU使用下面的引导参数:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on"
```
AMD CPU使用下面的引导参数:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on"
```

特别注意，如果你的网卡是PCIE转接的网卡，请多增加一个参数：
我的主机CPU是J4125,参数如下
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on pcie_port_pm=off"
```

之后，更新grub配置文件

```
update-grub
```

2. 增加内核模块

```
nano /etc/modules
```
在`modules`文件中增加如下内容:

```
vfio
vfio_iommu_type1
vfio_pci
```

**pve8.0以上版本不需要vfio_virqfd这个模块，因为模块目录压根就没有！！！**

3. 更新 initramfs 归档文件

```
update-initramfs -k -u all
```

4. 重启pve

```
reboot
```

5. 验证IOMMU是否开启

```
dmesg | grep -e DMAR -e IOMMU
```
会看到 
```
DMAR: IOMMU ENABLED
```

6. 可以新增虚拟机了，分配PCI设备，开机！

USB硬盘的直通，PVE确实很方便，直接qm disk set就可以了。


7. 如果你觉得local-lvm 和 lvm 两个分区容易造成空间浪费，可以合并成一个

	1、备份虚拟机
	2、删除虚拟机
	3、删除local-lvm
	命令：lvremove pve/data
	4、把local-lvm空间分配给Local
	命令：
	lvextend -l +100%FREE -r pve/root
	resize2fs /dev/mapper/pve-root
	5、删除local-lvm
	网页登陆，数据中心-存储-删除local-lvm
	6、编辑local，内容里添加 磁盘映像和容器，保存
	7、恢复虚拟机