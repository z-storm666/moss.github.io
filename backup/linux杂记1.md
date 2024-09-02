**解决Windows直接拖动文件进虚拟机ubuntu**

`https://www.jianshu.com/p/80c360fa4fcf`

**在VMware16中Ubuntu 20.04 不能全屏显示的解决方法**

更新源

`sudo apt update`

接着安装open-vm-tools

`sudo apt-get install open-vm-tools`
重启

**网络**
```
sudo service network-manager stop
sudo rm /var/lib/NetworkManager/NetworkManager.state
sudo service NetworkManager start
ping www.baidu.com
```

**安装搜狗**

设置系统语言为汉语

然后安装fcitx
```
sudo apt install fcitx-bin
sudo apt-get install fcitx-table
fictx --version
```
然后
`sudo dpkg -i xxx.deb`
系统报错，缺少依赖
然后执行`sudo apt install -f  `，系统会自动安装所需要的依赖包，然后重新执行`sudo dpkg -i xxx.deb`
然后

`https://shurufa.sogou.com/linux/guide`

**版本查看**

`cat /proc/version`

**Linux 中使用 Xrandr 配置显示器**

添加分辨率

`cvt 800 1280`

查看支持的分辨率

`xrandr`

查找有关显示器的信息

`xrandr -q`

`xrandr -s 800x1280`

`sudo vim /etc/profile` #在结尾添加两行

> X Error of failed request: BadName (named color or font does not exist)
> Major opcode of failed request: 140 (RANDR)
> Minor opcode of failed request: 16 (RRCreateMode)
> Serial number of failed request: 27
> Current serial number in output stream: 27
>
> Try 'xrandr --help' for more information
>
> xrandr: unrecognized option '-1'
>
> Try 'xrandr --help' for more information

`source /etc/profile` #让设置生效

**分辨率修改**

`https://blog.csdn.net/qq_51585635/article/details/119642107`

`https://blog.csdn.net/gongrulin/article/details/80096467`


