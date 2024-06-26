---
id: 1266
title: CentOS5-快速搭建ftp服务器
date: 2010-04-29T20:09:53+00:00
author: Stanley
layout: post
guid: http://www.mstz.us/?p=1266
permalink: /centos5-howto-setup-vsftpd/
categories:
  - 我的世界
tags:
  - CentOS
  - vsftpd
---
默认CentOS安装后，不启用vsftpd，且版本过旧。 

<div id="toc_container" class="toc_white no_bullets">
  <p class="toc_title">
    文章目录
  </p>
  
  <ul class="toc_list">
    <li>
      <a href="#1VSFTPD">1.安装VSFTPD</a>
    </li>
    <li>
      <a href="#2">2.编辑配置文件</a>
    </li>
    <li>
      <a href="#3">3.创建必需文件和启动服务</a>
    </li>
    <li>
      <a href="#4">4.添加用户</a>
    </li>
    <li>
      <a href="#5chroot_list">5.增加新建用户到chroot_list文件</a>
    </li>
    <li>
      <a href="#6">6.给防火墙添加规则</a>
    </li>
    <li>
      <a href="#7">7.测试</a>
    </li>
  </ul>
</div>

## <span id="1VSFTPD">1.安装VSFTPD</span> {#1vsftpd}

    yum install vsftpd  
    

## <span id="2">2.编辑配置文件</span> {#2}

    # Example config file /etc/vsftpd/vsftpd.conf  
     #  
     # The default compiled in settings are fairly paranoid. This sample file  
     # loosens things up a bit, to make the ftp daemon more usable.  
     # Please see vsftpd.conf.5 for all compiled in defaults.  
     #  
     # READ THIS: This example file is NOT an exhaustive list of vsftpd options.  
     # Please read the vsftpd.conf.5 manual page to get a full idea of vsftpd’s  
     # capabilities.  
     #  
     # Allow anonymous FTP? (Beware – allowed by default if you comment this out).  
     anonymous_enable=NO  
     #  
     # Uncomment this to allow local users to log in.  
     local_enable=YES  
     #  
     # Uncomment this to enable any form of FTP write command.  
     write_enable=YES  
     #  
     # Default umask for local users is 077. You may wish to change this to 022,  
     # if your users expect that (022 is used by most other ftpd’s)  
     local_umask=022  
     #  
     # Uncomment this to allow the anonymous FTP user to upload files. This only  
     # has an effect if the above global write enable is activated. Also, you will  
     # obviously need to create a directory writable by the FTP user.  
     anon_upload_enable=NO  
     #  
     # Uncomment this if you want the anonymous FTP user to be able to create  
     # new directories.  
     anon_mkdir_write_enable=NO  
     #  
     # Activate directory messages – messages given to remote users when they  
     # go into a certain directory.  
     dirmessage_enable=YES  
     #  
     # The target log file can be vsftpd_log_file or xferlog_file.  
     # This depends on setting xferlog_std_format parameter  
     xferlog_enable=YES  
     #  
     # Make sure PORT transfer connections originate from port 20 (ftp-data).  
     connect_from_port_20=YES  
     #  
     # If you want, you can arrange for uploaded anonymous files to be owned by  
     # a different user. Note! Using “root” for uploaded files is not  
     # recommended!  
     chown_uploads=NO  
     #chown_username=whoever  
     #  
     # The name of log file when xferlog_enable=YES and xferlog_std_format=YES  
     # WARNING – changing this filename affects /etc/logrotate.d/vsftpd.log  
     xferlog_file=/var/log/vsftpd-xferlog  
     #  
     # Switches between logging into vsftpd_log_file and xferlog_file files.  
     # NO writes to vsftpd_log_file, YES to xferlog_file  
     xferlog_std_format=YES  
     #  
     # You may change the default value for timing out an idle session.  
     idle_session_timeout=600  
     #  
     # You may change the default value for timing out a data connection.  
     data_connection_timeout=120  
     #  
     # It is recommended that you define on your system a unique user which the  
     # ftp server can use as a totally isolated and unprivileged user.  
     #nopriv_user=ftpsecure  
     #  
     # Enable this and the server will recognise asynchronous ABOR requests. Not  
     # recommended for security (the code is non-trivial). Not enabling it,  
     # however, may confuse older FTP clients.  
     async_abor_enable=YES  
     #  
     # By default the server will pretend to allow ASCII mode but in fact ignore  
     # the request. Turn on the below options to have the server actually do ASCII  
     # mangling on files when in ASCII mode.  
     # Beware that on some FTP servers, ASCII support allows a denial of service  
     # attack (DoS) via the command “SIZE /big/file” in ASCII mode. vsftpd  
     # predicted this attack and has always been safe, reporting the size of the  
     # raw file.  
     # ASCII mangling is a horrible feature of the protocol.  
     ascii_upload_enable=YES  
     ascii_download_enable=YES  
     #  
     # You may fully customise the login banner string:  
     ftpd_banner=Welcome to longxin FTP service.  
     #  
     # You may specify a file of disallowed anonymous e-mail addresses. Apparently  
     # useful for combatting certain DoS attacks.  
     #deny_email_enable=YES  
     # (default follows)  
     #banned_email_file=/etc/vsftpd/banned_emails  
     #  
     # You may specify an explicit list of local users to chroot() to their home  
     # directory. If chroot_local_user is YES, then this list becomes a list of  
     # users to NOT chroot().  
     chroot_local_user=NO  
     chroot_list_enable=YES  
     # (default follows)  
     chroot_list_file=/etc/vsftpd/chroot_list  
     #  
     # You may activate the “-R” option to the builtin ls. This is disabled by  
     # default to avoid remote users being able to cause excessive I/O on large  
     # sites. However, some broken FTP clients such as “ncftp” and “mirror” assume  
     # the presence of the “-R” option, so there is a strong case for enabling it.  
     #ls_recurse_enable=YES  
     #  
     # When “listen” directive is enabled, vsftpd runs in standalone mode and  
     # listens on IPv4 sockets. This directive cannot be used in conjunction  
     # with the listen_ipv6 directive.  
     listen=YES  
     #  
     # This directive enables listening on IPv6 sockets. To listen on IPv4 and IPv6  
     # sockets, you must run two copies of vsftpd whith two configuration files.  
     # Make sure, that one of the listen options is commented !!  
     #listen_ipv6=YES
    
    pam_service_name=vsftpd  
    tcp_wrappers=YES  
    

## <span id="3">3.创建必需文件和启动服务</span> {#3}

    touch /etc/vsftpd/chroot_list  
    service vsftpd start  
    chkconfig vsftpd on  
    

## <span id="4">4.添加用户</span> {#4}

vsftp默认使用系统用户, 现在添加用户`ftpuser`, 指向目录`/home/ftpuser`, **没有shell登录权限**。 

    useradd ftpuser -d /home/ftpuser -s /sbin/nologin  
    passwd ftpuser  
    

## <span id="5chroot_list">5.增加新建用户到chroot_list文件</span> {#5chroot_list}

    echo "ftpuser" >> /etc/vsftpd/chroot_list  
    service vsftpd restart  
    

## <span id="6">6.给防火墙添加规则</span> {#6}

    iptables -I INPUT -p tcp -m state –state NEW -m tcp –dport 21 -j ACCEPT  
    iptables -I INPUT -p tcp -m state –state NEW -m tcp –dport 20 -j ACCEPT  
    

`vi /etc/sysconfig/iptables-config`

> IPTABLES_MODULES=&#8221;ip_conntrack_ftp ip_nat_ftp ip_conntrack_netbios_ns&#8221;

## <span id="7">7.测试</span> {#7}

从客户端测试吧。