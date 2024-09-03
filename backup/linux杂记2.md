**DTS和DTSI**

> .dts文件是一种ASCII文本对Device Tree的描述，放置在内核的/arch/arm/boot/dts目录。一般而言，一个*.dts文件对应一个ARM的machine。

> ***.dtsi文件作用**：由于一个SOC可能有多个不同的电路板，而每个电路板拥有一个 *.dts。这些dts势必会存在许多共同部分，为了减少代码的冗余，设备树将这些共同部分提炼保存在*.dtsi文件中，供不同的dts共同使用。***.dtsi的使用方法：**类似于C语言的头文件，在dts文件中需要进行include *.dtsi文件。当然，dtsi本身也支持include 另一个dtsi文件。

**DTC**

> DTC为编译工具，它可以将.dts文件编译成.dtb文件。DTC的源码位于内核的scripts/dtc目录，内核选中CONFIG_OF，编译内核的时候，主机可执行程序DTC就会被编译出来。 即scripts/dtc/Makefile中

**DTB**

> DTC编译`*.dts`生成的二进制文件(`*.dtb`)，`bootloader`在引导内核时，会预先读取`*.dtb`到内存，进而由内核解析。

**Bootloader**

> Bootloader需要将设备树在内存中的地址传给内核。在ARM中通过`bootm`或`bootz`命令来进行传递。

**LKM**

> LKM（Linux kernel module）作为Linux内核的插件，其安装和卸载都很方便（热插拔），可以满足一些需要特殊内核操作而不想重新编译整个内核的场景，在存储和安全厂商的产品中，LKM使用非常广泛。

[[inux下编写和加载 .ko 文件（驱动模块文件） ](https://www.cnblogs.com/yuanqiangfei/p/10225115.html)](https://www.cnblogs.com/yuanqiangfei/p/10225115.html)

![image](https://github.com/user-attachments/assets/a0eaaa70-6284-49c2-96fe-7950f712b58d)






**/dts-v1/**

`/dts-v1/;` 声明了设备树的版本为 1.x，是设备树文件的标准开头，表明该文件遵循版本 1.x 的设备树语法。

**为什么include要加/**

```
/include/ "omap4.dtsi"
/include/ "elpida_ecb240abacn.dtsi"
```

在设备树（Device Tree）源文件中，`/include/` 语句用于引入其他设备树片段或文件。前面的 `/` 通常表示文件路径的根目录，或者是一个相对路径。

在设备树中使用 `/include/` 的前缀是为了确保正确地定位和引入依赖的文件。

-------



在设备树（Device Tree）源文件中，`model` 和 `compatible` 属性用于描述设备的特性和兼容性。这两个属性在设备树中非常重要，因为它们帮助操作系统识别和配置硬件设备，从而确保系统的正常运行。

**`model`属性**

> 描述设备类型

**`compatible`属性**

> 用于指示该设备与哪些驱动程序或硬件平台兼容

----

在设备树（Device Tree）中，`memory` 节点用于描述系统的内存区域