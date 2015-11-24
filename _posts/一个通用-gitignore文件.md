title: 一个通用.gitignore文件
tags: Git
categories: 项目管理工具
feature: img/git.jpg
description: 一个git仓库通用的.gitignore文件.
comments: true
recommend: true
declaration: true
toc: true
date: 2015-11-24 16:59:41
---

### 简介
在eclipse中创建maven项目时会有很多的项目描述文件及编译文件，如`.settings`、`.classpath`、`.project`、`target`等，但是在push时不希望将这些文件push到Git远程仓库，那么这时可以使用`.gitignore`文件进行配置。

<!--more-->
### 使用
只需要在项目的根目录下创建一个`.gitignore`文件，将下面的文件内容复制进去即可，在win下是无法直接创建`.gitignore`文件，可以使用编辑器（如editplus、sublime text等，不建议使用记事本编辑）的另存为保存为`.gitignore`。

### 文件内容
```
## .gitignore for Grails 1.2 and 1.3

# .gitignore for maven 
target/
*.releaseBackup

# web application files
#/web-app/WEB-INF
 
# IDE support files
/.classpath
/.launch
/.project
/.settings
/*.launch
/*.tmproj
/ivy*
/eclipse
 
# default HSQL database files for production mode
/prodDb.*
 
# general HSQL database files
*Db.properties
*Db.script
 
# logs
/stacktrace.log
/test/reports
/logs
*.log
*.log.*
 
# project release file
/*.war
 
# plugin release file
/*.zip
/*.zip.sha1
 
# older plugin install locations
/plugins
/web-app/plugins
/web-app/WEB-INF/classes
 
# "temporary" build files
target/
out/
build/
 
# other
*.iws
 
#.gitignore for java
*.class
 
# Package Files #
*.jar
*.war
*.ear
 
## .gitignore for eclipse
 
*.pydevproject
.project
.metadata
bin/**
tmp/**
tmp/**/*
*.tmp
*.bak
*.swp
*~.nib
local.properties
.classpath
.settings/
.loadpath
 
# External tool builders
.externalToolBuilders/
 
# Locally stored "Eclipse launch configurations"
*.launch
 
# CDT-specific
.cproject
 
# PDT-specific
.buildpath
 
## .gitignore for intellij
 
*.iml
*.ipr
*.iws
.idea/
 
## .gitignore for linux
.*
!.gitignore
*~
 
## .gitignore for windows
 
# Windows image file caches
Thumbs.db
ehthumbs.db
 
# Folder config file
Desktop.ini
 
# Recycle Bin used on file shares
$RECYCLE.BIN/
 
## .gitignore for mac os x
 
.DS_Store
.AppleDouble
.LSOverride
Icon
 
 
# Thumbnails
._*
 
# Files that might appear on external disk
.Spotlight-V100
.Trashes

## hack for graddle wrapper
!wrapper/*.jar
!**/wrapper/*.jar
```
