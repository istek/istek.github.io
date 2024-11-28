---
title: "ESXI安装群晖后的一些设置"
date: 2024-05-20T09:24:35+08:00
type: post
category: 
 - Linux
tags:
 - 云存储
---

1. USB硬盘直通后，按照HDD进行挂载。
需要重启群晖至bootloader，修改synoinfo,修改如下两个配置项目
```
internalportcfg="0xffffffff"
usbportcfg="0x0000"
```
<!--more-->
2. 群晖Photo安装后，无法显示HEVC，HEIF文件，需要安装`Advanced Media Extensions`，安装后还需要进行patch，才能显示对应文件的缩略图。
```
# DSM7.1 AME版本3.0.1-2004
curl http://code.imnks.com/ame3patch/ame71-2004.py | python
# DSM7.2 AME版本3.1.0-3005
curl http://code.imnks.com/ame3patch/ame72-3005.py | python
```
patch以后，上传的照片就可以自动显示缩略图了。
3. Synology Photos人脸识别补丁(仅支持Synology Photos 1.6.x版本)
1、先停用Synology Photos套件
2、SSH连接群晖执行修复
```
wget http://code.imnks.com/face/PatchELFSharp
chmod +x PatchELFSharp
# support face and concept
./PatchELFSharp "/var/packages/SynologyPhotos/target/usr/lib/libsynophoto-plugin-platform.so.1.0" "_ZN9synophoto6plugin8platform20IsSupportedIENetworkEv" "B8 00 00 00 00 C3"
# force to support concept
./PatchELFSharp "/var/packages/SynologyPhotos/target/usr/lib/libsynophoto-plugin-platform.so.1.0" "_ZN9synophoto6plugin8platform18IsSupportedConceptEv" "B8 01 00 00 00 C3"
# force no Gpu
./PatchELFSharp "/var/packages/SynologyPhotos/target/usr/lib/libsynophoto-plugin-platform.so.1.0" "_ZN9synophoto6plugin8platform23IsSupportedIENetworkGpuEv" "B8 00 00 00 00 C3"
```
3、重建索引