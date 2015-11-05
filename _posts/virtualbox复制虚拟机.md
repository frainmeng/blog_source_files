title: virtualbox复制虚拟机
date: 2015-11-04 10:37:34
tags: virtualbox
categories: linux
feature: /img/virtualbox_oralce_001.jpg
description: 如何复制Virtualbox中的虚拟机.
toc: true
recommend: true
declaration: true
comments: true
---

因为需要两台一样的linux机器进行测试，而我的virtualbox中已经装了一个centos的虚机，现在想要clone这样一台虚拟机，就省去了重新安装和配置的麻烦。

<!--more-->

### 克隆虚机

想要克隆很简单，virtualbox也提供了这样一个功能：
1.  选择要复制的虚拟机，右键菜单选择复制
    
    ![复制虚拟机](/img/clone_vm_000001.png "clone vm")

2.  在弹出菜单中按下图选择后点击复制
    
    ![复制虚拟机](/img/clone_vm_000002.png "clone vm")

3.  复制完成后就可以启动了，但是网络可能会连不上，需要进行一些配置    
    这是因为我们复制的时候，把机器的网卡的Mac地址配置也复制过来了，
    所以下面我们进行一些配置

### 配置

启动复制的虚机并登陆（用户名和密码和复制源机器相同）

1.  查看网卡
    
    ```
    [root@localhost ~]# cd /etc/sysconfig/network-scripts/
    [root@localhost network-scripts]# ls
    ```

    列出的文件中ifcfg-ethx(x为数字)便是你的网卡了

2.  修改网卡Mac地址配置
    
    可以通过下面的方式查看你的机器的Mac地址，选中你的虚机，右键，设置
    ![查看Mac地址](/img/clone_vm_000003.png "clone vm")
    在设置中查看
    ![查看Mac地址](/img/clone_vm_000004.png "clone vm")

    修改网卡的Mac地址之前建议备份一下（我用的是eth1）
    
    ```bash
    [root@localhost network-scripts]# cp ifcfg-eth1 ifcfg-eth1bak20151104
    ```
    现在可以修改了：
    ```bash
    [root@localhost network-scripts]# vi ifcfg-eth1 
    ```
    修改HAWADDR（即上面查出来的Mac地址，注意格式）
    ![修改Mac地址](/img/clone_vm_000005.png "clone vm")
    修改好后保存退出，重启网络服务
    ```bash
    [root@localhost network-scripts]# service network restart
    ```
    如果服务重启失败那就重启机器
    如果还是不行就查看下面的配置
    
3.  修改网卡配置
    
    ```bash
    [root@localhost rules.d]# cd /etc/udev/rules.d/
    [root@localhost rules.d]# ls
    60-raw.rules  70-persistent-cd.rules  70-persistent-net.rules  99-fuse.rules
    [root@localhost rules.d]# vi 70-persistent-net.rules
    ```
    查看你对应的网卡的Mac地址对不对，如果不对，就修改然后再重启。



