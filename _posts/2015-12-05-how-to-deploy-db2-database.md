---
id: 1500
title: DB2数据库部署指南
date: 2015-12-05T06:12:10+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=1500
permalink: /how-to-deploy-db2-database/
categories:
  - 我的世界
tags:
  - 数据库
---
<section class="post-content">前些日子，根据公司安排，割接公司下的一个系统至客户资源池，应用中间件是tongweb，可以说真的不好使，没有weblogic好用，稳定。而数据库则是沿用以前的DB2数据库，而该数据库，我还真没有操作过，也没有用过，更别说什么部署了，以下文档是我同事转给我的，是以前公司部署环境的文档，修改后，发表至这里，希望对某些人有用。服务器系统是`solaris 10`，建议使用之前，将默认shell修改为bash，通过修改.profile完成，以及在`.profile`中增加一些常用的命令alias。[TOC]

<div id="toc_container" class="toc_white no_bullets">
  <p class="toc_title">
    文章目录
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#i">一、硬件检查</a><ul>
        <li>
          <a href="#1">1. 检查内存</a>
        </li>
        <li>
          <a href="#2">2. 检查硬盘</a>
        </li>
        <li>
          <a href="#3_swap">3. 检查swap</a>
        </li>
        <li>
          <a href="#4_tmp">4. 检查tmp</a>
        </li>
        <li>
          <a href="#5_64">5. 检查操作系统是否64位</a>
        </li>
        <li>
          <a href="#6">6. 检查操作系统版本</a>
        </li>
        <li>
          <a href="#7">7. 检查文件包有无安装</a>
        </li>
        <li>
          <a href="#8">8. 检查补丁安装</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#_DB2">二、 DB2安装要求</a>
    </li>
    <li>
      <a href="#i-2">三、创建用户组(可选)</a><ul>
        <li>
          <a href="#1-2">1. 操作步骤</a>
        </li>
        <li>
          <a href="#2-2">2. 验证方法</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#i-3">四、上传安装文件</a>
    </li>
    <li>
      <a href="#JDKJDK6">五、JDK安装配置（JDK6）</a><ul>
        <li>
          <a href="#1-3">1. 安装步骤</a>
        </li>
        <li>
          <a href="#2-3">2.部署验证</a>
        </li>
        <li>
          <a href="#3">3.程序卸载（非必要）</a>
        </li>
      </ul>
    </li>
    
    <li>
      <a href="#DB2">六、DB2数据库安装配置</a><ul>
        <li>
          <a href="#1_DB2">1. DB2数据库安装</a>
        </li>
        <li>
          <a href="#2-4">2. 操作系统参数调整</a>
        </li>
        <li>
          <a href="#3-2">3. 启动数据库相关服务</a>
        </li>
        <li>
          <a href="#4">4. 备份数据库</a>
        </li>
        <li>
          <a href="#5">5. 还原数据库</a>
        </li>
        <li>
          <a href="#6_license">6. 更新license</a>
        </li>
        <li>
          <a href="#7-2">7. 部署验证</a>
        </li>
        <li>
          <a href="#8-2">8. 程序卸载</a>
        </li>
        <li>
          <a href="#9">9. 常见问题</a>
        </li>
      </ul>
    </li>
  </ul>
</div>

# <span id="i">一、硬件检查</span> {#}

## <span id="1">1. 检查内存</span> {#1}

    /usr/sbin/prtconf | grep -i memory
    

内存至少是1GB 
  
![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214647/120415_0430_1_dfnios.png?w=720&ssl=1)

## <span id="2">2. 检查硬盘</span> {#2}

    #df –k
    

硬盘3GB以上。![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214646/120415_0430_2_qj6il0.png?w=720&ssl=1)

## <span id="3_swap">3. 检查swap</span> {#3swap}

    # swap -l
    

Swap大小如下： 
   
内存1GB–2GB,SWAP是内存的1.5倍。 
   
内存2GB–8GB,SWAP是内存的1倍,即与内存同大小。 
   
内存8GB以上,SWAP是内存的0.75倍。 
  
![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214645/120415_0430_3_qui8zq.png?w=720&ssl=1)

## <span id="4_tmp">4. 检查tmp</span> {#4tmp}

    # df -k /tmp
    

/tmp大小应大于400MB. 
  
![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214645/120415_0430_4_mcjbls.png?w=720&ssl=1)

## <span id="5_64">5. 检查操作系统是否64位</span> {#564}

    #isainfo -b
    

![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214644/120415_0430_5_m5a7re.png?w=720&ssl=1)

## <span id="6">6. 检查操作系统版本</span> {#6}

    #uname –a
    

![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214643/120415_0430_6_uhpngw.png?w=720&ssl=1)

## <span id="7">7. 检查文件包有无安装</span> {#7}

    #less /var/sadm/install/contents
    

![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449214642/120415_0430_7_j0p68p.png?w=720&ssl=1)![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211645/120415_0430_8_mscnud.png?w=720&ssl=1)

## <span id="8">8. 检查补丁安装</span> {#8}

    # patchadd -p
    

（检查系统的补丁情况）![](https://i2.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211644/120415_0430_9_hhwxrz.png?w=720&ssl=1)

    # showrev -p
    

（查看所有已经安装的patch）![](https://i2.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211643/120415_0430_10_ydxsn5.png?w=720&ssl=1)![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211643/120415_0430_12_jhi6b2.png?w=720&ssl=1)

# <span id="_DB2">二、 DB2安装要求</span> {#db2}

![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211642/120415_0430_13_i0zzqo.png?w=720&ssl=1)

# <span id="i-2">三、创建用户组(可选)</span> {#}

xxxx系统使用的操作系统用户统一规定如下： 
   
|用户名（UID）|用户组（GID）|home目录 | 说明 | 
   
|————|————|———-|———| |db2inst1（300）|bcdl（300）|/opt/db2inst1|JDK、DB2用户| |cmbbcd（304）|bcdl（300）|/opt/cmbbcd |连接数据库用户|

## <span id="1-2">1. 操作步骤</span> {#1}

  * 以root登录系统
  * 创建bcdl用户组

    groupadd -g 300 bcdl  
    

  * 创建cmbbcd用户

    useradd -g bcdl -u 300 -d /opt/cmbbcd -m cmbbcd  
    

  * 设定bcdl用户密码

    passwd cmbbcd  
    

输入两次用户密码

## <span id="2-2">2. 验证方法</span> {#2}

  * 以cmbbcd用户登录系统
  * 输入以下命令确认UID、GID、用户组正确

    $ id
    uid=300(cmbbcd) gid=300(bcdl)  
    

# <span id="i-3">四、上传安装文件</span> {#}

为方便部署，在开始部署系统前将相关软件上传到solaris主机上，存放目录统一定为`/opt/software`。

  1. 以root登录系统 
  2. 创建`/opt/software`目录 
  
    > mkdir /opt/software</li> 
    > 
    >   * 将software目录赋予bcdl用户 
    
    >     chown -R bcdl:bcdl /opt/software
    >   * ftp连接solaris主机，以用户bcdl登录，切换目录到`/opt/software`，将后续部署需要的JDK、DB2安装文件上传</ol> 
    > 
    > # <span id="JDKJDK6">五、JDK安装配置（JDK6）</span> {#jdkjdk6}</blockquote> 
    > 
    > ## <span id="1-3">1. 安装步骤</span> {#1}
    > 
    >   1. 以root用户登录系统 
    >   2. 进入JDK安装文件所在目录`/opt/software` 
  
    >     `cd /opt/software<br></br>`
    >   3. 将JDK安装文件复制到`/opt/bcdl` 
  
    >     `cp jdk*.sh /opt/bcdl`
    >   4. 进入/opt/bcdl目录，为JDK安装文件增加执行权限 
  
    >     `cd /opt/bcdl chmod +x jdk*.sh`
    >   5. 将JDK安装文件赋予bcdl用户 
  
    >     `chown bcdl:bcdl jdk*.sh<br></br>`
    >   6. 切换到bcdl用户 
  
    >     `su – bcdl`
    >   7. 执行以下命令安装JDK 
  
    >     `cd /opt/bcdl ./jdk-6u7-solaris-sparc.sh`
    >   8. 按多次空格确认协议，直至出现以下界面，输入yes并回车 
  
    >     ![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211641/120415_0430_14_nx8k6t.png?w=720&ssl=1)
    >   9. 安装开始后出现以下信息表示安装结束 
  
    >     ![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211640/120415_0430_15_pl5ww4.png?w=720&ssl=1)
    > 
    > ## <span id="2-3">2.部署验证</span> {#2}
    > 
    >   1. 进入JDK安装目录
    > 
    >     cd /opt/bcdl/jdk1.6.0_07/bin  
    >     
    > 
    >   1. 输入以下命令
    > 
    >     ./java -version
    >     
    > 
    > 显示以下信息
    > 
    > ![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211639/120415_0430_16_wkdvzm.png?w=720&ssl=1)
    > 
    > ## <span id="3">3.程序卸载（非必要）</span> {#3}
    > 
    > jdk程序的卸载只要删除安装目录下的所有文件即可.
    > 
    > 命令如下:
    > 
    >     # cd /opt/bcdl //进入bcdl文件夹
    >     # rm -Rf jdk1.6.0_07 //删除JDK
    >     
    > 
    > # <span id="DB2">六、DB2数据库安装配置</span> {#db2}
    > 
    > ## <span id="1_DB2">1. DB2数据库安装</span> {#1db2}
    > 
    >   1. 启动Xmanager连接solaris主机，以root用户登录 
    >   2. 右键点击桌面，在菜单中选择Hosts 》 Terminal Console打开命令行窗口 
    >   3. 进入DB2安装文件所在目录`/opt/software` 
  
    >     `cd /opt/software<br></br>`
    >   4. 解压DB2压缩包 
  
    >     `tar xvf db2_v95_sun64_server.tar<br></br>`
    >   5. 解压后进入server目录 
  
    >     `cd server<br></br>`
    >   6. 启动DB2安装程序（**务必用root用户启动安装程序**） 
  
    >     `./db2setup`
    >   7. 选择”安装新产品(p)”，如下图 
  
    >      ![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211639/120415_0430_17_owdyma.png?w=720&ssl=1)
  
    >     选择安装”DB2 Enterprise Server Edition Version 9.5″版本
    >   8. 进入安装向导，点击”下一步”； 
  
    >     ![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211638/120415_0430_18_vnfkzt.png?w=720&ssl=1)
    >   9. 接受软件许可证协议，点击”下一步”； 
  
    >     ![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211637/120415_0430_19_awfkd3.png?w=720&ssl=1)
    >  10. 选择典型安装类型(Typical)，点击”下一步”； 
  
    >     ![](https://i2.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211635/120415_0430_20_o5poib.png?w=720&ssl=1)
    >  11. 选择”在此计算机安装DB2企业服务器版本9.5并将设置保存在响应文件中”，点击”下一步”； 
  
    >     ![](https://i2.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211634/120415_0430_21_zhwms9.png?w=720&ssl=1)
    >  12. 选择安装的路径，安装目录统一定为`/opt/IBM/db2/V9.5`； 
  
    >     ![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211631/120415_0430_22_jjlapy.png?w=720&ssl=1)
    >  13. 设定DB2管理服务器的用户：取消`Use default UID`和`Use default GID`前的选择，修改`UID`和`GID`为`301`，设置`dasusr1`密码，选择”下一步”（指定`UID`和GID`是为了对DB2作集群，DB2集群时需要保证各机器上相同用户及组拥有相同的`UID`和`GID\`，对15步和16步的操作原因与此相同）。 
  
    >     ![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211633/120415_0430_23_xzbsd4.png?w=720&ssl=1)
    >  14. 选择”创建DB2实例”，选择下一步； 
  
    >     ![](https://i2.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211632/120415_0430_24_xlgkts.png?w=720&ssl=1)
    >  15. 设定DB2实例的用户：取消`Use default UID`和`Use default GID`前的选择，给db2实例的用户设置密码，设置`UID`和`GID`为`302`，点击”下一步” 
  
    >     ![](https://i2.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211630/120415_0430_25_oahep1.png?w=720&ssl=1)
    >  16. 设定DB2用户自定义函数的用户：取消`Use default UID`和`Use default GID`前的选择，设置`UID`和`GID`为`303`，点击”下一步” 
  
    >     ![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211629/120415_0430_26_ufc3ek.png?w=720&ssl=1)
    >  17. 选择”在此计算机上不准备DB2工具目录”，点击”下一步” 
  
    >     ![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211628/120415_0430_27_nd2ips.png?w=720&ssl=1)
    >  18. 选择”不将DB2服务器设置为此时发送通知”，点击”下一步” 
  
    >     ![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211542/120415_0617_DB228_kcnhju.png?w=720&ssl=1)
    >  19. 总结信息，确认各项参数是否正确（用户、UID、GID等）。点击”完成” 
  
    >     ![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211542/120415_0617_DB229_knabxx.png?w=720&ssl=1)
    >  20. 安装进度提示 
  
    >     ![](https://i2.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211541/120415_0617_DB230_cyjjl5.png?w=720&ssl=1)
    >  21. DB2软件安装完成，将”Log File”文本框内容进行备份，以备后续使用。 
  
    >     ![](https://i2.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211539/120415_0617_DB231_sn1yk4.png?w=720&ssl=1)![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211539/120415_0617_DB232_w1kqtj.png?w=720&ssl=1)
    >  22. 为便于后续操作，将db2inst1用户的语言环境设为中文
    > 
    > `/opt/software`
    > 
    > 将光标移动到文件最后，增加以下内容，保存并退出vi
    > 
    > `/opt/software`
    > 
    > ## <span id="2-4">2. 操作系统参数调整</span> {#2}
    > 
    > **此步骤请务必慎重操作！**
    > 
    >   1. 以root用户登录 
    >   2. 输入以下命令获取DB2建议的操作系统参数值cd /opt/IBM/db2/V9.5/bin ./db2osconf 
    >   3. 执行命令后会显示建议参数值（实际环境参数值可能与图中有差异，以实际环境显示为准）  ![](https://i0.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211538/120415_0617_DB233_ojmd6f.png?w=720&ssl=1)
    >   4. 编辑系统参数文件 
  
    >     vi /etc/system将光标移动到文件最后，将上图中set开头的行复制到文件中（实际环境参数值可能与图中有差异，以实际环境显示为准）![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211537/120415_0617_DB234_zi7w40.png?w=720&ssl=1)保存并退出vi
    >   5. 重新启动操作系统
    > 
    > `cd /opt/software<br></br>`
    > 
    > ## <span id="3-2">3. 启动数据库相关服务</span> {#3}
    > 
    >   1. 以root用户登录 
    >   2. 启动DB2管理服务器 
  
    >     `. /export/home/dasusr1/das/dasprofile db2admin start`看到以下信息表示DB2管理服务器启动成功![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211536/120415_0617_DB235_jcq05m.png?w=720&ssl=1)如看到以下信息表示DB2管理服务器已经在运行中![](https://i2.wp.com/www.bestzhou.org/wp-content/uploads/2015/12/db2%E6%95%B0%E6%8D%AE%E5%BA%93%E9%83%A8%E7%BD%B2%E6%8C%87%E5%8D%97-1449706604-3.png?w=720&ssl=1)
    >   3. 切换到db2inst1
    > 
    > `/opt/bcdl`
    > 
    > 启动db2数据库实例
    > 
    > `cp jdk*.sh /opt/bcdl`
    > 
    > 看到以下信息表示DB2数据库实例启动成功![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211535/120415_0617_DB237_fdg0ft.png?w=720&ssl=1)如看到以下信息表示DB2数据库实例已经在运行中![](https://i2.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211533/120415_0617_DB238_xojxxj.png?w=720&ssl=1)
    > 
    > ## <span id="4">4. 备份数据库</span> {#4}
    > 
    > `cd /opt/bcdl chmod +x jdk*.sh`
    > 
    > ## <span id="5">5. 还原数据库</span> {#5}
    > 
    > 用UE编辑`creatab.sql`，修改”数据库连接名称”、”表的schame”等信息。另外，还需要编辑`DB2MOVE`文件夹下的`db2move.lst`文件，修改”表的schame”等信息。
    > 
    > 将修改过的这两个文件上传至新服务器:
    > 
    > `chown bcdl:bcdl jdk*.sh<br></br>`
    > 
    > ## <span id="6_license">6. 更新license</span> {#6license}
    > 
    >   1. 以root用户登录系统 
    >   2. 准备好db2 license文件，执行以下命令导入license
    > 
    > `su – bcdl`
    > 
    > 执行后如无异常，会显示以下结果![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211326/120415_0617_DB239_dqpykf.png?w=720&ssl=1)
    > 
    >   1. 显示当前部署的license信息
    > 
    > `cd /opt/bcdl ./jdk-6u7-solaris-sparc.sh`
    > 
    > 如下图所示，请确认license类型和过期时间![](https://i1.wp.com/res.cloudinary.com/the-backyard-of-stanley/image/upload/v1449211324/sdfasd1_bnofsx.png?w=720&ssl=1)
    > 
    > ## <span id="7-2">7. 部署验证</span> {#7}
    > 
    >   1. 测试是否可以成功连接bcdl数据库，用db2inst1用户登录solaris，执行以下命令： 
  
    >     `$ db2 connect to bcdl user cmbbcd using cmbbcd`
    >   2. 查询用户表，测试是否可以查出记录 
  
    >     `$ db2 select * from cmcs3_user`
    > 
    > ## <span id="8-2">8. 程序卸载</span> {#8}
    > 
    >   1. 以root用户登录 
    >   2. 初始化环境变量 
  
    >     `. /export/home/dasusr1/das/dasprofile`
    >   3. 停止db2管理服务器 
  
    >     `db2admin stop`
    >   4. 删除db2管理服务器 
  
    >     `/opt/IBM/db2/V9.5/instance/dasdrop`看到以下信息表示删除成功> DBI1070I Program dasdrop completed successfully.
    >   5. 切换到db2inst1用户 
  
    >     `su – db2inst1`停止db2服务 
  
    >     `$ db2stop force $ db2 terminate`退回root用户 `$ exit`
    >   6. 删除db2实例 
  
    >     `/opt/IBM/db2/V9.5/instance/db2idrop db2inst1`看到以下信息表示删除成功> DBI1070I Program db2idrop completed successfully.
    >   7. 删除db2应用 
  
    >     `/opt/IBM/db2/V9.5/install/db2_deinstall -a`看到以下信息表示成功> The execution completed successfully.
    >   8. 删除db2相关用户 
    >         cd /opt/bcdl/jdk1.6.0_07/bin  
    >         
    >     
    >     groupdel dasadm1  
    >     </br>
  
    >     groupdel db2iadm1  
    >     </br> 
  
    >     groupdel db2fadm1  
    >     </br>&#8220;\`</li> </ol> 
    >     
    >     ## <span id="9">9. 常见问题</span> {#9}
    >     
    >       1. 检查实例的诊断文件`db2diag.log`文件（默认放在DB2安装目录下），可定位错误。 
    >       2. 查DB2联机帮助,系统会给出基本解决方法。 
  
    >         `$ db2 ? sql0001`
    >       3. 查看IBM的信息中心 
  
    >         <http://publib.boulder.ibm.com/infocenter/db2luw/v9r5/index.jsp?topic=/com.ibm.db2.luw.admin.config.doc/doc/r0006012.html> 请下载各操作系统下DB2V9.5的补丁，可用补丁直接安装，不用原安装文件。 下载：[DB2 Server Fix Pack](http://www-1.ibm.com/support/docview.wss?rs=71&uid=swg21288088)</section> <footer class="post-footer"> <figure class="author-image"></figure> <section class="author">#### 
    >     
    >     [Stanley Zhou](https://www.bestzhou.org/author/stanley/)</section> </footer>