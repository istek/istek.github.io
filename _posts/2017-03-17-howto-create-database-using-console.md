---
title: oracle静默方式建库
date: 2017-03-17T19:24:59+00:00
layout: post
categories:
  - Linux
tags:
  - Oracle
---

有时候，真的不愿意用xmanager连服务器，再用GUI界面去创建、删除数据库，所以摸索了一下静默方式。以下内容是以oracle11gR2为参照版本的。

## 1、静默方式建库

oracle用户的主目录touch一个db_create.rsp文件，编辑该文件，内容如下：
```
[GENERAL]
RESPONSEFILE_VERSION = "11.2.0.1.0"  
OPERATION_TYPE = "createDatabase"  
[CREATEDATABASE]
GDBNAME = "tasdb"  
TEMPLATENAME = "General_Purpose.dbc"  
CHARACTERSET = "ZHS16GBK"  
oracle.install.db.InstallEdition = EE  
SYSPASSWORD = oracle  
SYSTEMPASSWORD = oracle  
MEMORYPERCENTAGE = "15"  
```

> responsefile_version 响应文件版本，其实就是数据库版本

> operation_type 静默操作类型

余下的内容请看稍后的响应文件例子。之后，执行这个命令开始建库:
```
dbca -silent -responseFile ./db_create.rsp
```
<!--more-->
## 2、静默方式删库

oracle用户的主目录touch一个`db_remove.rsp`文件，编辑该文件，内容如下：
```
[GENERAL]
RESPONSEFILE_VERSION = "11.2.0.1.0"  
OPERATION_TYPE = "deleteDatabase"  
[DELETEDATABASE]
SOURCEDB = "orcl"  
SYSDBAUSERNAME = "sys"  
SYSDBAPASSWORD = "oracle"  
```

之后，执行下面的命令开始删库
```
dbca -silent -deleteDatabase -responseFile ./db_remove.resp
```

###  附响应文件：
```
##############################################################################
##                                                                          ##
##                            DBCA response file                            ##
##                            ------------------                            ##
## Copyright   1998, 2007, Oracle Corporation. All Rights Reserved.         ##
##                                                                          ##
## Specify values for the variables listed below to customize Oracle        ##
## Database Configuration installation.                                     ##
##                                                                          ##
## Each variable is associated with a comment. The comment identifies the   ##
## variable type.                                                           ##
##                                                                          ##
## Please specify the values in the following format :                      ##
##          Type       :  Example                                           ##
##          String     :  "<value>"                                         ##
##          Boolean    :  True or False                                     ##
##          Number     :  <numeric value>                                   ##
##          StringList :  {"<value1>","<value2>"}                           ##
##                                                                          ##
## Examples :                                                               ##
##     1. dbca -progress_only -responseFile <response file>                 ##
##        Display a progress bar depicting progress of database creation    ##
##        process.                                                          ##
##                                                                          ##
##     2. dbca -silent -responseFile <response file>                        ##
##        Creates database silently. No user interface is displayed.        ##
##                                                                          ##
##     3. dbca -silent -createDatabase -cloneTemplate                       ##
##             -responseFile <response file>                      ##
##        Creates database silently with clone template. The template in    ##
##      responsefile is a clone template.                         ##
##                                                                          ##
##     4. dbca -silent -deleteDatabase -responseFile <response file>        ##
##        Deletes database silently.                                        ##
##############################################################################

#-----------------------------------------------------------------------------
# GENERAL section is required for all types of database creations.
#-----------------------------------------------------------------------------
[GENERAL]

#-----------------------------------------------------------------------------
# Name          : RESPONSEFILE_VERSION
# Datatype      : String
# Description   : Version of the database to create
# Valid values  : "11.1.0"
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
RESPONSEFILE_VERSION = "11.2.0"

#-----------------------------------------------------------------------------
# Name          : OPERATION_TYPE
# Datatype      : String
# Description   : Type of operation
# Valid values  : "createDatabase" \ "createTemplateFromDB" \ "createCloneTemplate" \ "deleteDatabase" \ "configureDatabase" \ "addInstance" (RAC-only) \ "deleteInstance" (RAC-only)
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
OPERATION_TYPE = "createDatabase"

#-----------------------*** End of GENERAL section ***------------------------

#-----------------------------------------------------------------------------
# CREATEDATABASE section is used when OPERATION_TYPE is defined as "createDatabase".
#-----------------------------------------------------------------------------
[CREATEDATABASE]

#-----------------------------------------------------------------------------
# Name          : GDBNAME
# Datatype      : String
# Description   : Global database name of the database
# Valid values  : <db_name>.<db_domain> - when database domain isn't NULL
#                 <db_name>             - when database domain is NULL
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
GDBNAME = "orcl11g.us.oracle.com"

#-----------------------------------------------------------------------------
# Name          : POLICYMANAGED
# Datatype      : Boolean
# Description   : Set to true if Database is policy managed and
#          set to false if  Database is admin managed
# Valid values  : TRUE\FALSE
# Default value : FALSE
# Mandatory     : No
#-----------------------------------------------------------------------------
#POLICYMANAGED = "false"

#-----------------------------------------------------------------------------
# Name          : CREATESERVERPOOL
# Datatype      : Boolean
# Description   : Set to true if new server pool need to be created for database
#          if this option is specified then the newly created database
#          will use this newly created serverpool.
#          Multiple serverpoolname can not be specified for database
# Valid values  : TRUE\FALSE
# Default value : FALSE
# Mandatory     : No
#-----------------------------------------------------------------------------
#CREATESERVERPOOL = "false"

#-----------------------------------------------------------------------------
# Name          : FORCE
# Datatype      : Boolean
# Description   : Set to true if new server pool need to be created by force
#          if this option is specified then the newly created serverpool
#          will be assigned server even if no free servers are available.
#          This may affect already running database.
#          This flag can be specified for Admin managed as well as policy managed db.
# Valid values  : TRUE\FALSE
# Default value : FALSE
# Mandatory     : No
#-----------------------------------------------------------------------------
#FORCE = "false"

#-----------------------------------------------------------------------------
# Name          : SERVERPOOLNAME
# Datatype      : String
# Description   : Only one serverpool name need to be specified
#           if Create Server Pool option is specified.
#           Comma-separated list of Serverpool names if db need to use
#           multiple Server pool
# Valid values  : ServerPool name
# Default value : None
# Mandatory     : No [required in case of RAC service centric database]
#-----------------------------------------------------------------------------
#SERVERPOOLNAME =

#-----------------------------------------------------------------------------
# Name          : CARDINALITY
# Datatype      : Number
# Description   : Specify Cardinality for create server pool operation
# Valid values  : any positive Integer value
# Default value : Number of qualified nodes on cluster
# Mandatory     : No [Required when a new serverpool need to be created]
#-----------------------------------------------------------------------------
#CARDINALITY =

#-----------------------------------------------------------------------------
# Name          : SID
# Datatype      : String
# Description   : System identifier (SID) of the database
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : <db_name> specified in GDBNAME
# Mandatory     : No
#-----------------------------------------------------------------------------
SID = "orcl11g"

#-----------------------------------------------------------------------------
# Name          : NODELIST
# Datatype      : String
# Description   : Comma-separated list of cluster nodes
# Valid values  : Cluster node names
# Default value : None
# Mandatory     : No (Yes for RAC database-centric database )
#-----------------------------------------------------------------------------
#NODELIST=

#-----------------------------------------------------------------------------
# Name          : TEMPLATENAME
# Datatype      : String
# Description   : Name of the template
# Valid values  : Template file name
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
TEMPLATENAME = "General_Purpose.dbc"

#-----------------------------------------------------------------------------
# Name          : OBFUSCATEDPASSWORDS
# Datatype      : Boolean
# Description   : Set to true if passwords are encrypted
# Valid values  : TRUE\FALSE
# Default value : FALSE
# Mandatory     : No
#-----------------------------------------------------------------------------
#OBFUSCATEDPASSWORDS = FALSE


#-----------------------------------------------------------------------------
# Name          : SYSPASSWORD
# Datatype      : String
# Description   : Password for SYS user
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
#SYSPASSWORD = "password"

#-----------------------------------------------------------------------------
# Name          : SYSTEMPASSWORD
# Datatype      : String
# Description   : Password for SYSTEM user
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
#SYSTEMPASSWORD = "password"

#-----------------------------------------------------------------------------
# Name          : EMCONFIGURATION
# Datatype      : String
# Description   : Enterprise Manager Configuration Type
# Valid values  : CENTRAL|LOCAL|ALL|NOBACKUP|NOEMAIL|NONE
# Default value : NONE
# Mandatory     : No
#-----------------------------------------------------------------------------
#EMCONFIGURATION = "NONE"

#-----------------------------------------------------------------------------
# Name          : DISABLESECURITYCONFIGURATION
# Datatype      : String
# Description   : Database Security Settings
# Valid values  : ALL|NONE|AUDIT|PASSWORD_PROFILE
# Default value : NONE
# Mandatory     : No
#-----------------------------------------------------------------------------
#DISABLESECURITYCONFIGURATION = "NONE"


#-----------------------------------------------------------------------------
# Name          : SYSMANPASSWORD
# Datatype      : String
# Description   : Password for SYSMAN user
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : Yes, if LOCAL specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#SYSMANPASSWORD = "password"

#-----------------------------------------------------------------------------
# Name          : DBSNMPPASSWORD
# Datatype      : String
# Description   : Password for DBSNMP user
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : Yes, if EMCONFIGURATION is specified
#-----------------------------------------------------------------------------
#DBSNMPPASSWORD = "password"

#-----------------------------------------------------------------------------
# Name          : CENTRALAGENT
# Datatype      : String
# Description   : Grid Control Central Agent Oracle Home
# Default value : None
# Mandatory     : Yes, if CENTRAL is specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#CENTRALAGENT =

#-----------------------------------------------------------------------------
# Name          : HOSTUSERNAME
# Datatype      : String
# Description   : Host user name for EM backup job
# Default value : None
# Mandatory     : Yes, if ALL or NOEMAIL are specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#HOSTUSERNAME =

#-----------------------------------------------------------------------------
# Name          : HOSTUSERPASSWORD
# Datatype      : String
# Description   : Host user password for EM backup job
# Default value : None
# Mandatory     : Yes, if ALL or NOEMAIL are specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#HOSTUSERPASSWORD=

#-----------------------------------------------------------------------------
# Name          : BACKUPSCHEDULE
# Datatype      : String
# Description   : Daily backup schedule in the form of hh:mm
# Default value : 2:00
# Mandatory     : Yes, if ALL or NOEMAIL are specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#BACKUPSCHEDULE=

#-----------------------------------------------------------------------------
# Name          : SMTPSERVER
# Datatype      : String
# Description   : Outgoing mail (SMTP) server for email notifications
# Default value : None
# Mandatory     : Yes, if ALL or NOBACKUP are specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#SMTPSERVER =

#-----------------------------------------------------------------------------
# Name          : EMAILADDRESS
# Datatype      : String
# Description   : Email address for email notifications
# Default value : None
# Mandatory     : Yes, if ALL or NOBACKUP are specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#EMAILADDRESS =

#-----------------------------------------------------------------------------
# Name          : DVOWNERNAME
# Datatype      : String
# Description   : DataVault Owner
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : Yes, if DataVault option is chosen
#-----------------------------------------------------------------------------
#DVOWNERNAME = ""

#-----------------------------------------------------------------------------
# Name          : DVOWNERPASSWORD
# Datatype      : String
# Description   : Password for DataVault Owner
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : Yes, if DataVault option is chosen
#-----------------------------------------------------------------------------
#DVOWNERPASSWORD = ""

#-----------------------------------------------------------------------------
# Name          : DVACCOUNTMANAGERNAME
# Datatype      : String
# Description   : DataVault Account Manager
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : No
#-----------------------------------------------------------------------------
#DVACCOUNTMANAGERNAME = ""

#-----------------------------------------------------------------------------
# Name          : DVACCOUNTMANAGERPASSWORD
# Datatype      : String
# Description   : Password for  DataVault Account Manager
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : No
#-----------------------------------------------------------------------------
#DVACCOUNTMANAGERPASSWORD = ""



#-----------------------------------------------------------------------------
# Name          : DATAFILEJARLOCATION
# Datatype      : String
# Description   : Location of the data file jar
# Valid values  : Directory containing compressed datafile jar
# Default value : None
# Mandatory     : No
#-----------------------------------------------------------------------------
#DATAFILEJARLOCATION =

#-----------------------------------------------------------------------------
# Name          : DATAFILEDESTINATION
# Datatype      : String
# Description   : Location of the data file's
# Valid values  : Directory for all the database files
# Default value : $ORACLE_BASE/oradata
# Mandatory     : No
#-----------------------------------------------------------------------------
#DATAFILEDESTINATION =

#-----------------------------------------------------------------------------
# Name          : RECOVERYAREADESTINATION
# Datatype      : String
# Description   : Location of the data file's
# Valid values  : Recovery Area location
# Default value : $ORACLE_BASE/flash_recovery_area
# Mandatory     : No
#-----------------------------------------------------------------------------
#RECOVERYAREADESTINATION=

#-----------------------------------------------------------------------------
# Name          : STORAGETYPE
# Datatype      : String
# Description   : Specifies the storage on which the database is to be created
# Valid values  : FS (CFS for RAC), ASM
# Default value : FS
# Mandatory     : No
#-----------------------------------------------------------------------------
#STORAGETYPE=FS

#-----------------------------------------------------------------------------
# Name          : DISKGROUPNAME
# Datatype      : String
# Description   : Specifies the disk group name for the storage
# Default value : DATA
# Mandatory     : No
#-----------------------------------------------------------------------------
#DISKGROUPNAME=DATA

#-----------------------------------------------------------------------------
# Name          : ASMSNMP_PASSWORD
# Datatype      : String
# Description   : Password for ASM Monitoring
# Default value : None
# Mandatory     : No
#-----------------------------------------------------------------------------
#ASMSNMP_PASSWORD=""

#-----------------------------------------------------------------------------
# Name          : RECOVERYGROUPNAME
# Datatype      : String
# Description   : Specifies the disk group name for the recovery area
# Default value : RECOVERY
# Mandatory     : No
#-----------------------------------------------------------------------------
#RECOVERYGROUPNAME=RECOVERY


#-----------------------------------------------------------------------------
# Name          : CHARACTERSET
# Datatype      : String
# Description   : Character set of the database
# Valid values  : Check Oracle11g National Language Support Guide
# Default value : "US7ASCII"
# Mandatory     : NO
#-----------------------------------------------------------------------------
#CHARACTERSET = "US7ASCII"

#-----------------------------------------------------------------------------
# Name          : NATIONALCHARACTERSET
# Datatype      : String
# Description   : National Character set of the database
# Valid values  : "UTF8" or "AL16UTF16". For details, check Oracle11g National Language Support Guide
# Default value : "AL16UTF16"
# Mandatory     : No
#-----------------------------------------------------------------------------
#NATIONALCHARACTERSET= "UTF8"

#-----------------------------------------------------------------------------
# Name          : REGISTERWITHDIRSERVICE
# Datatype      : Boolean
# Description   : Specifies whether to register with Directory Service.
# Valid values  : TRUE \ FALSE
# Default value : FALSE
# Mandatory     : No
#-----------------------------------------------------------------------------
#REGISTERWITHDIRSERVICE= TRUE

#-----------------------------------------------------------------------------
# Name          : DIRSERVICEUSERNAME
# Datatype      : String
# Description   : Specifies the name of the directory service user
# Mandatory     : YES, if the value of registerWithDirService is TRUE
#-----------------------------------------------------------------------------
#DIRSERVICEUSERNAME= "name"

#-----------------------------------------------------------------------------
# Name          : DIRSERVICEPASSWORD
# Datatype      : String
# Description   : The password of the directory service user.
#          You can also specify the password at the command prompt instead of here.
# Mandatory     : YES, if the value of registerWithDirService is TRUE
#-----------------------------------------------------------------------------
#DIRSERVICEPASSWORD= "password"

#-----------------------------------------------------------------------------
# Name          : WALLETPASSWORD
# Datatype      : String
# Description   : The password for wallet to created or modified.
#          You can also specify the password at the command prompt instead of here.
# Mandatory     : YES, if the value of registerWithDirService is TRUE
#-----------------------------------------------------------------------------
#WALLETPASSWORD= "password"

#-----------------------------------------------------------------------------
# Name          : LISTENERS
# Datatype      : String
# Description   : Specifies list of listeners to register the database with.
#          By default the database is configured for all the listeners specified in the
#          $ORACLE_HOME/network/admin/listener.ora     
# Valid values  : The list should be space separated names like "listener1 listener2".
# Mandatory     : NO
#-----------------------------------------------------------------------------
#LISTENERS = "listener1 listener2"

#-----------------------------------------------------------------------------
# Name          : VARIABLESFILE
# Datatype      : String
# Description   : Location of the file containing variable value pair
# Valid values  : A valid file-system file. The variable value pair format in this file
#          is <variable>=<value>. Each pair should be in a new line.
# Default value : None
# Mandatory     : NO
#-----------------------------------------------------------------------------
#VARIABLESFILE =

#-----------------------------------------------------------------------------
# Name          : VARIABLES
# Datatype      : String
# Description   : comma separated list of name=value pairs. Overrides variables defined in variablefile and templates
# Default value : None
# Mandatory     : NO
#-----------------------------------------------------------------------------
#VARIABLES =

#-----------------------------------------------------------------------------
# Name          : INITPARAMS
# Datatype      : String
# Description   : comma separated list of name=value pairs. Overrides initialization parameters defined in templates
# Default value : None
# Mandatory     : NO
#-----------------------------------------------------------------------------
#INITPARAMS =

#-----------------------------------------------------------------------------
# Name          : MEMORYPERCENTAGE
# Datatype      : String
# Description   : percentage of physical memory for Oracle
# Default value : None
# Mandatory     : NO
#-----------------------------------------------------------------------------
#MEMORYPERCENTAGE = "40"

#-----------------------------------------------------------------------------
# Name          : DATABASETYPE
# Datatype      : String
# Description   : used for memory distribution when MEMORYPERCENTAGE specified
# Valid values  : MULTIPURPOSE|DATA_WAREHOUSING|OLTP
# Default value : MULTIPURPOSE
# Mandatory     : NO
#-----------------------------------------------------------------------------
#DATABASETYPE = "MULTIPURPOSE"

#-----------------------------------------------------------------------------
# Name          : AUTOMATICMEMORYMANAGEMENT
# Datatype      : Boolean
# Description   : flag to indicate Automatic Memory Management is used
# Valid values  : TRUE/FALSE
# Default value : TRUE
# Mandatory     : NO
#-----------------------------------------------------------------------------
#AUTOMATICMEMORYMANAGEMENT = "TRUE"

#-----------------------------------------------------------------------------
# Name          : TOTALMEMORY
# Datatype      : String
# Description   : total memory in MB to allocate to Oracle
# Valid values  :
# Default value :
# Mandatory     : NO
#-----------------------------------------------------------------------------
#TOTALMEMORY = "800"


#-----------------------*** End of CREATEDATABASE section ***------------------------

#-----------------------------------------------------------------------------
# createTemplateFromDB section is used when OPERATION_TYPE is defined as "createTemplateFromDB".
#-----------------------------------------------------------------------------
[createTemplateFromDB]
#-----------------------------------------------------------------------------
# Name          : SOURCEDB
# Datatype      : String
# Description   : The source database from which to create the template
# Valid values  : The format is <host>:<port>:<sid>
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
SOURCEDB = "myhost:1521:orcl"

#-----------------------------------------------------------------------------
# Name          : SYSDBAUSERNAME
# Datatype      : String
# Description   : A user with DBA role.
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
SYSDBAUSERNAME = "system"

#-----------------------------------------------------------------------------
# Name          : SYSDBAPASSWORD
# Datatype      : String
# Description   : The password of the DBA user.
#          You can also specify the password at the command prompt instead of here.
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
#SYSDBAPASSWORD = "password"

#-----------------------------------------------------------------------------
# Name          : TEMPLATENAME
# Datatype      : String
# Description   : Name for the new template.
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
TEMPLATENAME = "My Copy TEMPLATE"

#-----------------------*** End of createTemplateFromDB section ***------------------------

#-----------------------------------------------------------------------------
# createCloneTemplate section is used when OPERATION_TYPE is defined as "createCloneTemplate".
#-----------------------------------------------------------------------------
[createCloneTemplate]
#-----------------------------------------------------------------------------
# Name          : SOURCEDB
# Datatype      : String
# Description   : The source database is the SID from which to create the template.
#          This database must be local and on the same ORACLE_HOME.
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
SOURCEDB = "orcl"

#-----------------------------------------------------------------------------
# Name          : SYSDBAUSERNAME
# Datatype      : String
# Description   : A user with DBA role.
# Default value : none
# Mandatory     : YES, if no OS authentication
#-----------------------------------------------------------------------------
#SYSDBAUSERNAME = "sys"

#-----------------------------------------------------------------------------
# Name          : SYSDBAPASSWORD
# Datatype      : String
# Description   : The password of the DBA user.
#          You can also specify the password at the command prompt instead of here.
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
#SYSDBAPASSWORD = "password"

#-----------------------------------------------------------------------------
# Name          : TEMPLATENAME
# Datatype      : String
# Description   : Name for the new template.
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
TEMPLATENAME = "My Clone TEMPLATE"

#-----------------------------------------------------------------------------
# Name          : DATAFILEJARLOCATION
# Datatype      : String
# Description   : Location of the data file jar
# Valid values  : Directory where the new compressed datafile jar will be placed
# Default value : $ORACLE_HOME/assistants/dbca/templates
# Mandatory     : NO
#-----------------------------------------------------------------------------
#DATAFILEJARLOCATION =

#-----------------------*** End of createCloneTemplate section ***------------------------

#-----------------------------------------------------------------------------
# DELETEDATABASE section is used when DELETE_TYPE is defined as "deleteDatabase".
#-----------------------------------------------------------------------------
[DELETEDATABASE]
#-----------------------------------------------------------------------------
# Name          : SOURCEDB
# Datatype      : String
# Description   : The source database is the SID
#          This database must be local and on the same ORACLE_HOME.
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
SOURCEDB = "orcl"

#-----------------------------------------------------------------------------
# Name          : SYSDBAUSERNAME
# Datatype      : String
# Description   : A user with DBA role.
# Default value : none
# Mandatory     : YES, if no OS authentication
#-----------------------------------------------------------------------------
#SYSDBAUSERNAME = "sys"

#-----------------------------------------------------------------------------
# Name          : SYSDBAPASSWORD
# Datatype      : String
# Description   : The password of the DBA user.
#          You can also specify the password at the command prompt instead of here.
# Default value : none
# Mandatory     : YES, if no OS authentication
#-----------------------------------------------------------------------------
#SYSDBAPASSWORD = "password"
#-----------------------*** End of deleteDatabase section ***------------------------

#-----------------------------------------------------------------------------
# GENERATESCRIPTS section
#-----------------------------------------------------------------------------
[generateScripts]
#-----------------------------------------------------------------------------
# Name          : TEMPLATENAME
# Datatype      : String
# Description   : Name of the template
# Valid values  : Template name as seen in DBCA
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
TEMPLATENAME = "New Database"

#-----------------------------------------------------------------------------
# Name          : GDBNAME
# Datatype      : String
# Description   : Global database name of the database
# Valid values  : <db_name>.<db_domain> - when database domain isn't NULL
#                 <db_name>             - when database domain is NULL
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
GDBNAME = "orcl11.us.oracle.com"

#-----------------------------------------------------------------------------
# Name          : SCRIPTDESTINATION
# Datatype      : String
# Description   : Location of the scripts
# Valid values  : Directory for all the scripts
# Default value : None
# Mandatory     : No
#-----------------------------------------------------------------------------
#SCRIPTDESTINATION =

#-----------------------*** End of deleteDatabase section ***------------------------

#-----------------------------------------------------------------------------
# CONFIGUREDATABASE section is used when OPERATION_TYPE is defined as "configureDatabase".
#-----------------------------------------------------------------------------
[CONFIGUREDATABASE]

#-----------------------------------------------------------------------------
# Name          : SOURCEDB
# Datatype      : String
# Description   : The source database is the SID
#          This database must be local and on the same ORACLE_HOME.
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
#SOURCEDB = "orcl"

#-----------------------------------------------------------------------------
# Name          : SYSDBAUSERNAME
# Datatype      : String
# Description   : A user with DBA role.
# Default value : none
# Mandatory     : YES, if no OS authentication
#-----------------------------------------------------------------------------
#SYSDBAUSERNAME = "sys"


#-----------------------------------------------------------------------------
# Name          : SYSDBAPASSWORD
# Datatype      : String
# Description   : The password of the DBA user.
#          You can also specify the password at the command prompt instead of here.
# Default value : none
# Mandatory     : YES, if no OS authentication
#-----------------------------------------------------------------------------
#SYSDBAPASSWORD =

#-----------------------------------------------------------------------------
# Name          : REGISTERWITHDIRSERVICE
# Datatype      : Boolean
# Description   : Specifies whether to register with Directory Service.
# Valid values  : TRUE \ FALSE
# Default value : FALSE
# Mandatory     : No
#-----------------------------------------------------------------------------
#REGISTERWITHDIRSERVICE= TRUE

#-----------------------------------------------------------------------------
# Name          : UNREGISTERWITHDIRSERVICE
# Datatype      : Boolean
# Description   : Specifies whether to unregister with Directory Service.
# Valid values  : TRUE \ FALSE
# Default value : FALSE
# Mandatory     : No
#-----------------------------------------------------------------------------
#UNREGISTERWITHDIRSERVICE= TRUE

#-----------------------------------------------------------------------------
# Name          : REGENERATEDBPASSWORD
# Datatype      : Boolean
# Description   : Specifies whether regenerate database password in OID/Wallet
# Valid values  : TRUE \ FALSE
# Default value : FALSE
# Mandatory     : No
#-----------------------------------------------------------------------------
#REGENERATEDBPASSWORD= TRUE

#-----------------------------------------------------------------------------
# Name          : DIRSERVICEUSERNAME
# Datatype      : String
# Description   : Specifies the name of the directory service user
# Mandatory     : YES, if the any of the reg/unreg/regenPasswd options specified
#-----------------------------------------------------------------------------
#DIRSERVICEUSERNAME= "name"

#-----------------------------------------------------------------------------
# Name          : DIRSERVICEPASSWORD
# Datatype      : String
# Description   : The password of the directory service user.
#          You can also specify the password at the command prompt instead of here.
# Mandatory     : YES, if the any of the reg/unreg/regenPasswd options specified
#-----------------------------------------------------------------------------
#DIRSERVICEPASSWORD= "password"

#-----------------------------------------------------------------------------
# Name          : WALLETPASSWORD
# Datatype      : String
# Description   : The password for wallet to created or modified.
#          You can also specify the password at the command prompt instead of here.
# Mandatory     : YES, if the any of the reg/unreg/regenPasswd options specified
#-----------------------------------------------------------------------------
#WALLETPASSWORD= "password"

#-----------------------------------------------------------------------------
# Name          : DISABLESECURITYCONFIGURATION
# Datatype      : String
# Description   : Database Security Settings
# Valid values  : ALL|NONE|AUDIT|PASSWORD_PROFILE
# Default value : NONE
# Mandatory     : No
#-----------------------------------------------------------------------------
#DISABLESECURITYCONFIGURATION = "NONE"



#-----------------------------------------------------------------------------
# Name          : ENABLESECURITYCONFIGURATION
# Datatype      : String
# Description   : Database Security Settings
# Valid values  : true|false
# Default value : true
# Mandatory     : No
#-----------------------------------------------------------------------------
#ENABLESECURITYCONFIGURATION = "true"


#-----------------------------------------------------------------------------
# Name          : EMCONFIGURATION
# Datatype      : String
# Description   : Enterprise Manager Configuration Type
# Valid values  : CENTRAL|LOCAL|ALL|NOBACKUP|NOEMAIL|NONE
# Default value : NONE
# Mandatory     : No
#-----------------------------------------------------------------------------
#EMCONFIGURATION = "NONE"

#-----------------------------------------------------------------------------
# Name          : SYSMANPASSWORD
# Datatype      : String
# Description   : Password for SYSMAN user
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : Yes, if LOCAL specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#SYSMANPASSWORD = "password"

#-----------------------------------------------------------------------------
# Name          : DBSNMPPASSWORD
# Datatype      : String
# Description   : Password for DBSNMP user
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : Yes, if EMCONFIGURATION is specified
#-----------------------------------------------------------------------------
#DBSNMPPASSWORD = "password"

#-----------------------------------------------------------------------------
# Name          : CENTRALAGENT
# Datatype      : String
# Description   : Grid Control Central Agent Oracle Home
# Default value : None
# Mandatory     : Yes, if CENTRAL is specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#CENTRALAGENT =

#-----------------------------------------------------------------------------
# Name          : HOSTUSERNAME
# Datatype      : String
# Description   : Host user name for EM backup job
# Default value : None
# Mandatory     : Yes, if ALL or NOEMAIL are specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#HOSTUSERNAME =

#-----------------------------------------------------------------------------
# Name          : HOSTUSERPASSWORD
# Datatype      : String
# Description   : Host user password for EM backup job
# Default value : None
# Mandatory     : Yes, if ALL or NOEMAIL are specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#HOSTUSERPASSWORD=

#-----------------------------------------------------------------------------
# Name          : BACKUPSCHEDULE
# Datatype      : String
# Description   : Daily backup schedule in the form of hh:mm
# Default value : 2:00
# Mandatory     : Yes, if ALL or NOEMAIL are specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#BACKUPSCHEDULE=

#-----------------------------------------------------------------------------
# Name          : SMTPSERVER
# Datatype      : String
# Description   : Outgoing mail (SMTP) server for email notifications
# Default value : None
# Mandatory     : Yes, if ALL or NOBACKUP are specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#SMTPSERVER =

#-----------------------------------------------------------------------------
# Name          : EMAILADDRESS
# Datatype      : String
# Description   : Email address for email notifications
# Default value : None
# Mandatory     : Yes, if ALL or NOBACKUP are specified for EMCONFIGURATION
#-----------------------------------------------------------------------------
#EMAILADDRESS =

#-----------------------*** End of CONFIGUREDATABASE section ***------------------------


#-----------------------------------------------------------------------------
# ADDINSTANCE section is used when OPERATION_TYPE is defined as "addInstance".
#-----------------------------------------------------------------------------
[ADDINSTANCE]

#-----------------------------------------------------------------------------
# Name          : DB_UNIQUE_NAME
# Datatype      : String
# Description   : DB Unique Name of the RAC database
# Valid values  : <db_unique_name>
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
DB_UNIQUE_NAME = "orcl11g.us.oracle.com"

#-----------------------------------------------------------------------------
# Name          : INSTANCENAME
# Datatype      : String
# Description   : RAC instance name to be added
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : <sid_prefix>+<highest_current_thread+1>
# Mandatory     : No
#-----------------------------------------------------------------------------
#INSTANCENAME = "orcl1"

#-----------------------------------------------------------------------------
# Name          : NODELIST
# Datatype      : String
# Description   : Node on which to add new instance
#                 (in 10gR2, instance addition is supported on 1 node at a time)
# Valid values  : Cluster node name
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
NODELIST=

#-----------------------------------------------------------------------------
# Name          : OBFUSCATEDPASSWORDS
# Datatype      : Boolean
# Description   : Set to true if passwords are encrypted
# Valid values  : TRUE\FALSE
# Default value : FALSE
# Mandatory     : No
#-----------------------------------------------------------------------------
#OBFUSCATEDPASSWORDS = FALSE

#-----------------------------------------------------------------------------
# Name          : SYSDBAUSERNAME
# Datatype      : String
# Description   : A user with DBA role.
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
SYSDBAUSERNAME = "sys"

#-----------------------------------------------------------------------------
# Name          : SYSDBAPASSWORD
# Datatype      : String
# Description   : The password of the DBA user.
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
#SYSDBAPASSWORD = "password"

#-----------------------*** End of ADDINSTANCE section ***------------------------


#-----------------------------------------------------------------------------
# DELETEINSTANCE section is used when OPERATION_TYPE is defined as "deleteInstance".
#-----------------------------------------------------------------------------
[DELETEINSTANCE]

#-----------------------------------------------------------------------------
# Name          : DB_UNIQUE_NAME
# Datatype      : String
# Description   : DB Unique Name of the RAC database
# Valid values  : <db_unique_name>
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
DB_UNIQUE_NAME = "orcl11g.us.oracle.com"

#-----------------------------------------------------------------------------
# Name          : INSTANCENAME
# Datatype      : String
# Description   : RAC instance name to be deleted
# Valid values  : Check Oracle11g Administrator's Guide
# Default value : None
# Mandatory     : Yes
#-----------------------------------------------------------------------------
INSTANCENAME = "orcl11g"

#-----------------------------------------------------------------------------
# Name          : NODELIST
# Datatype      : String
# Description   : Node on which instance to be deleted (SID) is located
# Valid values  : Cluster node name
# Default value : None
# Mandatory     : No
#-----------------------------------------------------------------------------
#NODELIST=

#-----------------------------------------------------------------------------
# Name          : OBFUSCATEDPASSWORDS
# Datatype      : Boolean
# Description   : Set to true if passwords are encrypted
# Valid values  : TRUE\FALSE
# Default value : FALSE
# Mandatory     : No
#-----------------------------------------------------------------------------
#OBFUSCATEDPASSWORDS = FALSE

#-----------------------------------------------------------------------------
# Name          : SYSDBAUSERNAME
# Datatype      : String
# Description   : A user with DBA role.
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
SYSDBAUSERNAME = "sys"

#-----------------------------------------------------------------------------
# Name          : SYSDBAPASSWORD
# Datatype      : String
# Description   : The password of the DBA user.
# Default value : none
# Mandatory     : YES
#-----------------------------------------------------------------------------
#SYSDBAPASSWORD = "password"


#---------------------*** End of DELETEINSTANCE section ***-----------------------
```