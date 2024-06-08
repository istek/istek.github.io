---
id: 864
title: 使用了几天Ubuntu之后
date: 2007-10-21T07:06:30+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=864
permalink: /a-few-days-after-using-ubuntu/
categories:
  - 我的世界
tags:
  - Ubuntu
---
7月18日，一个非常特殊的日子，对于所有使用[Ubuntu](http://www.ubuntu.org.cn)发行版的朋友来说，因为Ubuntu在这一天全球发布了Ubuntu 7.10<span class="postbody">(开发代号Gutsy Gibbon)。虽然我也使用Ubuntu，但是从来没有碰到过这样特殊的日子，18号一天都在电脑前等着呢，说实话，从来没有这么期盼过一个操作系统的诞生，在Ubuntu中文论坛有朋友说，Windows Vista发布也没有这么热闹过。很多朋友耐心的等待，终于在18号晚上Ubuntu官方网站的首页改版，放出了Release。</p> 

<p>
  使用了几天之后，我觉得可能我有些月没有用过了，还是对命令和文件系统生疏了许多。在这一版里，Ubuntu默认启用了NTFS文件系统的读写支持以及打开了Compiz Fusion桌面效果，当时我看到后就一个感想，太酷了，效果真的棒的没法说，当然了，前提条件是显卡要支持，我的电脑配置有点过时了，内存1G，显卡是Nvdia Geforce2 MX400，支持没有问题，速度不慢.
</p>

<p>
  本着Linux自由、分享的精神，下面我把我遇到的一些问题及解决办法贴出来，相信有很多朋友也遇到过。呵呵。</span>
</p>

<p>
  1.关于源的问题。
</p>

<p>
  我个人使用tw的源。具体地址如下:
</p>

<pre><code>deb http://tw.archive.ubuntu.com/ubuntu gutsy main restricted universe multiverse deb http://tw.archive.ubuntu.com/ubuntu gutsy-security main restricted universe multiverse deb http://tw.archive.ubuntu.com/ubuntu gutsy-updates main restricted universe multiverse deb http://tw.archive.ubuntu.com/ubuntu gutsy-backports main restricted universe multiverse deb http://tw.archive.ubuntu.com/ubuntu gutsy-proposed main restricted universe multiverse deb-src http://tw.archive.ubuntu.com/ubuntu gutsy main restricted universe multiverse deb-src http://tw.archive.ubuntu.com/ubuntu gutsy-security main restricted universe multiverse deb-src http://tw.archive.ubuntu.com/ubuntu gutsy-updates main restricted universe multiverse deb-src http://tw.archive.ubuntu.com/ubuntu gutsy-backports main restricted universe multiverse deb-src http://tw.archive.ubuntu.com/ubuntu gutsy-proposed main restricted universe multiverse  
</code></pre>

<p>
  2.安装系统后，窗口标题栏没有了！
</p>

<p>
  解决办法有三个:
</p>

<p>
  １.先备份原来的配置文件
</p>

<pre><code>cp /etc/X11/xorg.conf /etc/X11/xorg.conf.bak  
</code></pre>

<p>
  然后在xorg.conf中Module段加入
</p>

<pre><code>SubSection "extmod"  
Option "omit xfree86-dga"  
EndSubSection  
</code></pre>

<p>
  在Device段中加入
</p>

<pre><code>Option "DisableGLXRootClipping" "True"  
Option "AddARGBGLXVisuals" "True"  
Option "AllowGLXWithComposite" "True"  
Option "RenderAccel" "True"  
</code></pre>

<p>
  在配置文件的最后加入
</p>

<pre><code>Section "Extensions"  
Option "Composite" "Enable"  
EndSection  
</code></pre>

<p>
  ２.如果试N卡的话, 运行下面的命令, 再重启X.
</p>

<pre><code>sudo nvidia-xconfig --add-argb-glx-visuals #解决没有窗口边框的问题  
</code></pre>

<p>
  ３.<code>sudo dpkg-reconfigure -phigh xserver-xorg</code>
</p>

<pre><code>OPTION ADDRGBGLXVirtual "true"  
</code></pre>

<p>
  3.雅黑字体的安装。
</p>

<p>
  默认的字体效果其实可以，但是为了有Windows Vista的界面字体效果，可以使用下面的办法。
</p>

<pre><code>#创建文件夹yahei
sudo mkdir /usr/share/fonts/yahei  
#将微软雅黑（MSYH.ttf&MSYHBD.ttf）字体放入/usr/share/fonts/yahei文件夹
sudo cp /home/xxxx/*.ttf /usr/share/fonts/yahei  
sudo chmod +rx  /usr/share/fonts/yahei/*  
#编辑/etc/X11/xorg.conf
sudo gedit /etc/X11/xorg.conf  
#加入一行 
FontPath "/usr/share/fonts/yahei"  
#示例 
#----------------------------------------------------------------- 
Section "Files"  
RgbPath "/usr/lib/X11/rgb"  
FontPath "/usr/share/fonts/vista"  
EndSection  
#----------------------------------------------------------------- #
建立字体缓存信息
fc-cache -fv  
#修改/etc/fonts/language-selector.conf
sudo gedit /etc/fonts/language-selector.conf  
#分别在(不含"!"号) 
!&lt;family&gt;Bitstream Vera Serif&lt;/family&gt; 
!&lt;family&gt;Bitstream Vera Sans&lt;/family&gt; 
!&lt;family&gt;Bitstream Vera Sans Mono&lt;/family&gt; 
#此为默认字体,如果更改过字体,&lt;family&gt;标签中的即为你更改过的字体 #下一行添加 &lt;family&gt;Microsoft YaHei&lt;/family&gt; 
#示例 
#----------------------------------------------------------------- 
&lt;alias&gt;  
&lt;family&gt;serif&lt;/family&gt;  
&lt;family&gt;Microsoft YaHei&lt;/family&gt;  
&lt;prefer&gt;  
&lt;family&gt;Bitstream Vera Serif&lt;/family&gt;  
&lt;family&gt;Microsoft YaHei&lt;/family&gt;  
&lt;family&gt;DejaVu Serif&lt;/family&gt;  
&lt;family&gt;AR PL ShanHeiSun Uni&lt;/family&gt;  
&lt;family&gt;WenQuanYi Bitmap Song&lt;/family&gt;  
&lt;family&gt;AR PL ZenKai Uni&lt;/family&gt;  
&lt;/prefer&gt;  
&lt;/alias&gt;  
&lt;alias&gt;  
&lt;family&gt;sans-serif&lt;/family&gt;  
&lt;family&gt;Microsoft YaHei&lt;/family&gt;  
&lt;prefer&gt; &lt;family&gt;Bitstream Vera Sans&lt;/family&gt; &lt;family&gt;Microsoft YaHei&lt;/family&gt;  
&lt;family&gt;DejaVu Sans&lt;/family&gt;  
&lt;family&gt;AR PL ShanHeiSun Uni&lt;/family&gt;  
&lt;family&gt;WenQuanYi Bitmap Song&lt;/family&gt;  
&lt;family&gt;AR PL ZenKai Uni&lt;/family&gt;  
&lt;/prefer&gt;  
&lt;/alias&gt;  
&lt;alias&gt;  
&lt;family&gt;monospace&lt;/family&gt;  
&lt;family&gt;Microsoft YaHei&lt;/family&gt;  
&lt;prefer&gt;  
&lt;family&gt;Bitstream Vera Sans Mono&lt;/family&gt;  
&lt;family&gt;Microsoft YaHei&lt;/family&gt;  
&lt;family&gt;DejaVu Sans Mono&lt;/family&gt;  
&lt;family&gt;AR PL ShanHeiSun Uni&lt;/family&gt;  
&lt;family&gt;WenQuanYi Bitmap Song&lt;/family&gt;  
&lt;family&gt;AR PL ZenKai Uni&lt;/family&gt;  
&lt;/prefer&gt;  
&lt;/alias&gt;  
#------------------------------------------------------------------------ 
</code></pre>

<p>
  然后在系统->首选项->字体中选择微软雅黑即可
</p>

<p>
  4.关于w32codecs和libdvdcss2的问题。
</p>

<p>
  毫无疑问，w32codecs是播放wmv,wma,rmvb等等文件不可缺少的解码包，而libdvdcss2似乎源里没有。现在把这两个包的安装办法记录下来。
</p>

<pre><code>sudo gedit /etc/apt/sources.list  
</code></pre>

<p>
  加入
</p>

<pre><code>deb http://medibuntu.sos-sts.com/repo/ feisty free non-free  
deb-src http://medibuntu.sos-sts.com/repo/ feisty free non-free  
</code></pre>

<p>
  加入key
</p>

<pre><code>wget -q http://packages.medibuntu.org/medibuntu-key.gpg -O- | sudo apt-key add -  
</code></pre>

<p>
  更新
</p>

<pre><code>sudo apt-get update  
</code></pre>

<p>
  安装
</p>

<pre><code>sudo apt-get install w32codecs libdvdcss2  
</code></pre>

<p>
  5.什么播放器比较适合自己？
</p>

<p>
  现在播放器有那么多，到底哪个适合自己呢？答案是肯定的，我列出我的选择，smplayer+audacious这两个就可以很好的处理视频和音频文件了，smplayer类似于win下面的暴风影音工具。安装步骤我再不叙述，安装samplayer之前应该安装mplayer，然后就是安装audacious了。
</p>

<p>
  smplayer点击<a href="http://smplayer.sourceforge.net/">这个网站</a>下载，其他的源里都有。
</p>

<p>
  6.scim输入法与很多软件冲突，有没有办法解决呢？
</p>

<p>
  scim输入法是ubuntu下的一种输入软件，相似的还有小企鹅输入法，因为目前小企鹅输入法已经暂停维护了，所以还是使用scim吧，即便有不兼容的情况发生。
</p>

<pre><code>im-switch -s scim -z default  
sudo apt-get install scim-qtimm  
sudo apt-get install scim scim-pinyin scim-tables-zh im-switch scim-qtimm scim-bridge scim-bridge-client-gtk scim-bridge-client-qt scim-bridge-agent  
</code></pre>

<p>
  编辑im-switch生成的scim配置文件
</p>

<pre><code># gksu gedit /etc/X11/xinit/xinput.d/scim 
</code></pre>

<p>
  将默认的<code>GTK_IM_MODULE=scim</code>修改为<code>GTK_IM_MODULE="scim-bridge"</code>。保存退出. 在scim输入法中进行了如下设定： scim设置－>全局设置－>将预编辑字符串嵌入到客户端中 前的勾去掉 scim设置->gtk->嵌入式候选词标的勾去掉.<br /> 重启scim<br /> 打开终端,输入<code>pkill scim</code>然后输入<code>scim -d</code>
</p>

<p>
  7.GnuPG默认安装了，但是是字符界面的命令，是否有图形界面的程序呢？
</p>

<p>
  有！是seahorse！可以从源里安装。
</p>

<p>
  8.如何修改菜单中的菜单或者某个程序的图标？
</p>

<p>
  系统－首选项－主菜单。点击某个应用程序的属性来修改。
</p>

<p>
  9.IPv6的关闭。
</p>

<p>
  Ubuntu默认是打开IPv6协议的。关闭步骤如下：
</p>

<pre><code>sudo gedit /etc/modprobe.d/aliases  
</code></pre>

<p>
  将<code>alias net-pf-10 ipv6</code>改为<code>alias net-pf-10 off</code>
</p>

<pre><code>sudo gedit /etc/sysctl.conf  
</code></pre>

<p>
  加入
</p>

<pre><code>net.ipv4.tcp_ecn = 0  
</code></pre>

<p>
  打开firefox, about:config，将ipv6禁用。
</p>

<p>
  10.所有的菜单文件的位置。
</p>

<pre><code>/usr/share/applications/
</code></pre>

<p>
  11.一些好用的软件。
</p>

<p>
  eva 类似QQ；sylpheed轻量级的邮件客户端，也支持新闻组；streamtuner在线广播软件；adobe reader不用我说了吧，pdf真是好东西！
</p>

<p>
  未完待续。
</p>

<p>
  附上一张我的桌面图吧，呵呵，没有加什么主题。本文感谢Cash给予极大的帮助！
</p>