title: PL/SQL Developer使用Oracle instant client配置
tags: SQL
categories: 开发工具使用
feature: img/plsql_feature.png
description: PL/SQL Developer配置Instant client.
comments: true
recommend: true
declaration: true
toc: true
date: 2015-11-19 12:32:23
---
### 问题
在使用PL/SQL Developer工具时会遇到这种情况：不想装Oracle的客户端，想使用Instant client，但是在配置的时候会遇到不少麻烦。我就在使用时也遇到该问题，记下解决的方法

<!--more-->
### 解决方法
两种解决方法都很简单

#### 方式一，通过写脚本解决
在PL/SQL Developer的安装目录（plsqldev.exe所在目录）下新建文件一个txt文件，然后重命名为`start.bat`（文件名随便，只要文件类型为`bat`就行），然后输入以下内容：
```
@echo off 
set path=D:\oracle_client_32\instantclient_11_2
set ORACLE_HOME=D:\oracle_client_32\instantclient_11_2
set TNS_ADMIN=D:\oracle_client_32\instantclient_11_2
set NLS_LANG=AMERICAN_AMERICA.AL32UTF8
start plsqldev.exe
```
其中：
1. `path` 和 `ORACLE_HOME`：为Oracle instant client的根目录
2. `TNS_ADMIN`：为你的`tnsnames.ora`文件的目录
3. `NLS_LANG`：设定字符集

#### 方式二，设置快捷方式属性
安装完PL/SQL Developer后在桌面应该有个快捷方式（如果没有自己添加），然后右键->属性->快捷方式->目标，在内容后面**追加**以下内容：
```
InstantClient=D:\oracle_client_32\instantclient_11_2 TNS_ADMIN=D:\oracle_client_32\instantclient_11_2 NLS_LANG=AMERICAN_AMERICA.AL32UTF8
```
其中：
1. `InstantClient`：为Oracle instant client的根目录
2. `TNS_ADMIN`和`NLS_LANG`：上面已经说过了。
3. 个属性之间要用空格隔开

