
## **树莓派搭建NAS服务器（OMV搭建）**

[OpenMediaVault](https://baike.baidu.com/item/OpenMediaVault/5404831)，是一个开源的基于[Debian](https://baike.baidu.com/item/Debian/748667?fromModule=lemma_inlink) Linux的下一代网络附加存储([NAS](https://baike.baidu.com/item/NAS/3465615?fromModule=lemma_inlink))解决方案

### **准备工作**

安装lite版系统（没有界面，省内存空间），烧录工具使用树莓派镜像烧录器(烧录方式参照Batocera系统的烧录过程)，烧录时要注意设置好用户名和密码以及WIFI网络（登录时，安装依赖时都会用到）

> https://www.raspberrypi.com/software/operating-systems/
>
>
>![](https://files.mdnice.com/user/47467/ad4fde2a-a41f-488f-ab51-266cd3b75567.png)


**外接显示屏的情况下：**

确保树莓派 WiFi 已经连上后。

`ifconfig`

可以直接查看到ip地址，把这个地址记下来

> **inet：192.168.5.156**
>
> netmask: 255.255.255.0
>
> broadcast: 192.168.5.255



**如果没有屏幕：**就需要登录到自己家的路由器中查看，一般是`192.168.1.1`

或者用`Advanced ip scanner`工具来扫描出树莓派的ip，名字一般是r`apsberrypi`

> https://www.advanced-ip-scanner.com/cn/



我是使用的外接显示屏





### **打开树莓派的ssh功能**

ssh功能可以在制作SD卡时一并打开，在SD卡分区的空白位置建立名为ssh的空白文件即可，不需要后缀名。

或者在终端中输入`sudo raspi-config`,选择`interface`选项卡开启

### **putty访问**

常用的远程访问方式就是`ssh`访问，使用`putty`工具。

如果你有图形化界面，可以使用windows自带的远程桌面或者`vnc viewer`访问

如果和我一样是`lite`版，可以直接选择使用`putty`

> putty:  https://www.putty.org/?hl=zh-cn

首先做一些设置

> putty界面汉化：https://www.huwangyun.cn/blog/change-putty-language

![](https://files.mdnice.com/user/47467/1bb56868-989b-4772-b6c2-a05e93c7e164.png)


load----save----open
![](https://files.mdnice.com/user/47467/82a49004-2eed-468d-baaf-3d370b363136.png)


> 在系统注册表缓存中没有找到该服务器密钥，不能保证该服务器是能够正确访问的计算机该服务器的 rsa2 密钥指纹为:ssh-rsa 2048 13:d0:8d:44:18:7e:80:7b:ee:9d:2b:e0:7f:9c:86:2d如果信任该主机，请点击"是"增加密钥到 PuTTY 缓存中并继续连接如果仅仅只希望进行本次连接，而不将密钥储存，请点击"否"。如果不信任该主机，请点击"取消"放弃连接,

Accept

然后关闭大黑框，关闭putty软件

重新打开putty软件

重复上面操作

然后输入用户名，密码

![](https://files.mdnice.com/user/47467/c3e3c092-1386-4c81-93e2-e33680876666.png)


连接成功

 **ps:如果你安装了图形化界面**

>那么也可以选择远程桌面来访问（推荐VNC viewer，效果不错。需要在interface选项卡打开vnc服务）

> 然后在电脑端下载安装一个`vnc viewer`的客户端，直接输入`ip`地址即可连接。第一次需要输入密码。
然后我们就拥有一个可以远程访问且连入局域网的树莓派了

### 换源


在安装前，还需要最后一步，就是换源。换成国内源下载会快很多。



**1、首先编辑hosts文件（#号就是注释掉的）**

`sudo nano /etc/hosts`

![](https://files.mdnice.com/user/47467/d7ba48fc-13df-44a7-aec0-63806fd24705.png)


文本末添加

```
199.232.45.194 github.global.ssl.fastly.net 
20.205.243.166 github.com
```

修改后使用 Ctrl+X，

提示：save modified buffer ...?    ，选择 ：yes

又提示：file name to write ：***hosts   ，选择：Ctrl+T

在下一个界面用 “上下左右” 按键 选择要保存的文件名，

 然后直接点击 “Enter” 按键即可保存。

**ps:后面步骤出现类似下面的错误**

>![](https://files.mdnice.com/user/47467/3b984314-26b5-4ec8-a27d-a61f4175df44.png)
>是因为改网址是被墙的
>
> https://ping.chinaz.com/github.global.ssl.fastly.net
>
> ![](https://files.mdnice.com/user/47467/f5a853ba-f452-4c92-8868-78cd78843fb7.png)
>
> 换一个能ping通的IP





**2、树莓派默认下载源很慢，需要换源**

软件源是指` debian `系操作系统的应用程序安装包仓库，很多的软件都会这收录到这个仓库里面。而树莓派的 `raspberrypi `操作系统也是基于` debian` 的，所以树莓派也有自己的软件源，用来收录各种树莓派应用程序。 默认情况下，树莓派软件源地址是` http://archive.raspbian.org`，位于欧洲，在国内访问是非常慢的，经常只有几 k 每秒的下载速率。所以我们在玩转树莓派之前，强烈推荐替换成国内的软件源。 树莓派的所有软件源地址可以从这里找到：`https://www.raspbian.org/RaspbianMirrors`

一般我们找个国内的就行了，比如清华大学的

首先需要确定自己的树莓派系统版本，树莓派系统常用的有debian 9 和debian 10，输入如下指令可以查询

>
>
>`lsb_release -a`
>
> 该指令的作用是查看操作系统版本 (适用于所有的linux，包括Redhat、SuSE、Debian等发行版，但是在debian下要安装lsb，树莓派默认已安装)
>
>![](https://files.mdnice.com/user/47467/fc0b9d9c-de1f-4995-96f2-c7f4e1693f13.png)
>
>我用的是debian 12 bookworm版本的

**注意这个Codename**，Codename是什么就要在源地址对应字段更改一下

> debian 9 stretch版本：
>
> ![](https://files.mdnice.com/user/47467/b013be4b-478a-40b8-aef7-1f7659503c9e.png)
>
> debian 10 buster版本：

然后
`sudo nano /etc/apt/sources.list`

#号注释掉以前的部分，添加两个清华源

`deb https://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ bookworm main non-free contrib  rpi`

`deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ bookworm main non-free contrib rpi `

![](https://files.mdnice.com/user/47467/9d08676f-dcb7-48c7-be9d-28651c38a0e8.png)

修改后使用 Ctrl+X，

提示：save modified buffer ...?    ，选择 ：yes

又提示：file name to write ：....sources.list，选择：Ctrl+T

在下一个界面用 “上下左右” 按键 选择要保存的文件名，

然后直接点击 “Enter” 按键即可保存。

-----

`sudo nano /etc/apt/sources.list.d/raspi.list`

\#号注释掉以前的部分，添加一个清华源

`deb http://mirrors.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui`

记得保存，然后换源就完成了

![](https://files.mdnice.com/user/47467/420ab934-a567-4681-b027-e63363ccca6e.png)



## OpenMediaVault（OMV）安装及设置

使用配置要求不高，而且免费开源的OMV作为NAS服务器的架构（也支持阵列）。

后期也可以使用OMV安装插件，或者使用docker来完善NAS。

我们选择使用命令行这种更快捷的安装方法

**安装前先更新（否则会出现依赖错误导致安装失败）**

`sudo apt-get update`

`sudo apt-get upgrade`

更新完成后就可以安装了

### 命令行安装

命令行安装omv是脚本解析安装的，所以会调用github中的脚本

用CDN的源安装（需要大概半个小时左右）

`wget -O - https://cdn.jsdelivr.net/gh/OpenMediaVault-Plugin-Developers/installScript@master/install | sudo bash`

或者直接调用github中的脚本解析安装（我是连不上，你们可以试试看）

`wget -O - https://raw.githubusercontent.com/OpenMediaVault-Plugin-Developers/installScript/master/install | sudo bash`

![](https://files.mdnice.com/user/47467/0236e1eb-430f-4145-9013-d60eeb25be54.png)


**安装完成后系统会自动重启，这个时候putty也会失去连接，需要重连**

![](https://files.mdnice.com/user/47467/153f0db0-d7bd-4610-923c-169f1116bb01.png)


**重启后需要注意：**

因为脚本重置了各项设置，网络需要重新配置。

这个时候需要插网线，重新调整ip，然后输入以下命令

`sudo apt install -y dhcpcd5 network-manager`

`sudo systemctl enable dhcpcd`

`sudo systemctl start dhcpcd`

`sudo reboot`

wifi就可以用了。

ps:

> 也可以在安装时跳过网络设置（没试过）
>
> `wget https://raw.githubusercontent.com/OpenMediaVault-Plugin-Developers/installScript/master/install`
>
> `chmod +x install	`
>
> `sudo ./install -n`

> 实测可行的方法是，在编辑hosts文件时加入这一行`185.199.108.133 raw.githubusercontent.com`
>
> 脚本执行完后在最后就会不停的连接githubusercontent.com但是就是连不上，这时`CTRL+C`取消即可，完美跳过了重启过程

>`openmediavault`提示安装之后可能会出现ip地址变化的情况，保险起见用`ifconfig`命令查看一下当前IP




## OMV设置

在浏览器中输入ip地址，就可以进入omv的网页端了，需要登录。

用户名默认为admin，密码为openmediavault。


![](https://files.mdnice.com/user/47467/1b809aec-9993-4d7b-9345-b49fb8d46e35.jpg)





