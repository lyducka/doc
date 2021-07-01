# N1盒子刷Armbian（转自张大妈-来杯黄桃酸奶）



#### 感谢原作者非常详细的说明，转发作为备用。



**创作立场声明**：一个写完之后很累很累的年轻人

**追加修改(2020-03-31 22:49:31):**
安装系统至eMMC代码为“nand-sata-install”，前发有一个标点错误，会导致写入失败，感谢13楼值友@sharphe

看过我上周文章的朋友可能还记得，刚刚入手不过两个小时的N1盒子甚至还没有写开箱就被我刷成了砖，本来是第一篇的“N1盒子刷Armbian”迫不得已成了第二篇，不过值得庆祝的是，得益于翻车，终于顺利的把Armbian5.77刷入N1的那些坑全部总结完成。

> [N1盒子不拆机救砖，10分钟从ArmBian刷回原厂固件（站内首发）](https://post.smzdm.com/p/akmg5dv4/)

**这是一篇全过程指导书，也是对我个人玩N1不断翻车、不断学习的见证。**

**首先，帮助各位新手理清两个关系。**

N1盒子目前有两个系统用的比较多，**安卓（Android）和Linux**

**安卓系统**中大家比较熟悉的应该是**盒子的原生系统**、**YYF系统**以及**w大的官改系统**

**Linux系统**中比较熟悉的应该是**“小钢炮”**和**Armbian**

**今天，我们主要说的就是Armbian**

Armbian在我站渐渐兴起，@阿文菌等大佬们功不可没，对**OMV和HomeAssistant等Docker**的安装和介绍，着实拯救了一批压箱底的盒子和疫情没事情做的值友，顺便还拯救了一波JS。

这是后话，再聊。

**在Linux系统中安装Docker和在安卓系统里安装APP是一样的道理，不同的只不过是安装方式和使用习惯。**

**所以，选一个稳定的系统至关重要。**

**我只推荐Armbian_5.77_Aml-s905_Ubuntu_bionic_default_5.0.2_20190401.img.xz**

我用了两天时间从5.3挨个版本刷到5.9，虽然，没有再继续，但是，我认为这就是最好的版本。

这也是目前恩山论坛最广泛肯定的版本，我就不该自己瞎折腾（摔）![N1盒子刷Armbian，看这一篇就够了——Armbian全方位安装指导书](https://raw.githubusercontent.com/lyducka/pic/master/uPic/36.png)

官网下载，软硬件功能均正常，运行稳定，而我们要做的只不过是替换下@xiangsm大佬修改的dtd文件，然后映射一下。

大佬的**[原文链接](https://www.right.com.cn/forum/thread-510423-1-1.html)**在此。

这就开始吧

### **第一步：软硬件准备（已经帮你们在网盘里准备好了）**

[**网盘地址**](https://pan.baidu.com/s/1F2e_2i0gICaadYuAfwBHOw) **提取码 vyi7**

1、N1盒子一个

2、[键盘](https://www.smzdm.com/fenlei/jianpan/)、[鼠标](https://www.smzdm.com/fenlei/shubiao/)、[显示器](https://www.smzdm.com/fenlei/xianshiqi/)、HDMI线、USB双公线、8G以上U盘

3、电脑，能开机的

4、软件：PuTTY （SSH）或者Xshell，Win32DiskImager（把ArmBian写入U盘），N1-ADBU盘启动，WinScp（往盒子里上传还原备份），N1-T1降刷机助手v1.1版本、Amlogic USB Burning Tool_v2.1.6.8

5.ArmBian5.77

\6. xiangsm大佬修改的dtd文件（meson-gxl-s905d-phicomm-n1-xiangsm.dtb）

**第二步：给N1盒子降级（这个操作N1盒子这辈子都只用一次）**

PS：我站真的很多这个方面的文章，但是为了装X，说自己的是指导书，只能再重新写一遍了。

和上面一样，为了避免选择困难症，我只会给大家提供一个工具**N1-T1降刷机助手v1.1版本**

**1.安装Amlogic USB Burning Tool_v2.1.6.8烧录程序**

这个软件会提供给我们一共驱动，刷机要用。

[![就是这个adb设备驱动](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e817d950c1a36232.png_e1080.jpg)]就是这个adb设备驱动




设为头图

**2.打开盒子的ADB功能**

盒子插电开机，插上鼠标（有线无线都行）、[网线](https://www.smzdm.com/fenlei/wangxian/)，HDMI线连接支持HDMI的[显示器](https://www.smzdm.com/fenlei/xianshiqi/)或者电视，USB公对公线连接N1和电脑主机，进入主页后，用鼠标左键单击四下【固件版本】即可开启ADB。

[![顺便看下版本号](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e817c84d79615461.jpg_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_3/)顺便看下版本号

**这里看一下版本号，如果2.2以下就不用了，顺便记一下IP地址，后面会用。**



[![插上网线](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e817ccb1b5501598.jpg_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_4/)插上网线

**3.打开N1-T1降刷机助手v1.1输入地址**

选择需要降级的设备名然后输入设备IP地址，请注意盒子需要打开adb模式才能降级，否则会一直提示连接超时

[![开始](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e817d94a7e792905.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_5/)开始



[![这是失败案例](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e817d94e10955853.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_6/)这是失败案例

**4.开始降级**

直接点击开始降级后软件会实时监听设备连接状态：当设备连接成功后会自动刷入降级文件。

[![结束刷机](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e817d952958d5904.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_7/)结束刷机

**注意：此工具降级时不需要将adb.exe等文件放到系统[文件夹](https://www.smzdm.com/fenlei/wenjianjia/)下，降级工具使用的是绝对路径。仅需要放在软件的根目录即可。**

给大家再甩一篇值友的详细介绍

> [N1盒子系列 篇一：N1简明降级&刷机教程](https://post.smzdm.com/p/a99vxp9e/)

### **第二步：制作系统U盘（我只准备了一个软件，没别的选）**

将一个8G以上的U盘插入电脑，然后选择下载的Armbian_5.77镜像写入，注意别写错U盘。

[![选择好U盘](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e81801ea26e54233.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_8/)选择好U盘

[![别的都不动，写入](https://qnam.smzdm.com/202003/30/5e81801ea2a374040.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_9/)别的都不动，写入

[![写入成功](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e81801e8c3714579.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_10/)写入成功

[![提示格式化](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8180541a3746844.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_11/)提示格式化

**这里千万别格式化！千万别格式化！千万别格式化！**

**第三步：修改引导dtb**

有的img可能是已经修改好的，但是我保证，我这个真没改，这是官网上直接下的。**![N1盒子刷Armbian，看这一篇就够了——Armbian全方位安装指导书](https://res.smzdm.com/images/emotions/45.png)**

**进我的电脑，找到BOOT**

[![这就是格式化之后可以显示的boot分区](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e818148c7e86903.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_12/)这就是格式化之后可以显示的boot分区

**把下载的的dtb文件拷进去**



[![把下载的的dtb文件拷进去](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e818148dc6803841.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_13/)把下载的的dtb文件拷进去

**打开uEnv.ini,修改配置**

[![打开uEnv.ini,](https://qnam.smzdm.com/202003/30/5e818148eaea59096.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_14/)打开uEnv.ini,

[![添加框里的文字](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8181491bdae3583.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_15/)添加框里的文字

**我知道看着打字比较麻烦，所以直接抄就好了。完事CTRL+S保存**

> dtb_name=/dtb/meson-gxl-s905d-phicomm-n1-xiangsm.dtb

### **第四步：引导从U盘启动**

**1.N1-ADBU盘启动工具进行引导**



[![输入网址 我的是0.60](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8182c58fe327047.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_16/)输入网址 我的是0.60

[![然后ok](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8182dc7874a8404.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_17/)然后ok

**2.将U盘插入靠近hdmi的U口里，上电重启，会自动进入U盘系统。**

然后开始疯狂跑代码

**下面的图片是手机拍的，见谅。**



[![提示登陆](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e81839df0c2c404.jpg_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_18/)提示登陆

**默认用户名：root 密码：1234**

**输入后会要求提示修改密码，先输入一遍旧密码1234，再输两遍新密码。**

**密码长度大于8个字符，****修改后，提示新建账户，直接按Ctrl+C跳过，再次输入登录。**

### 第五步：镜像写入eMMC

1.电脑上打开Putty或者Xshell，我用Putty，免安装。当然，我也只提供了putty，嘿嘿。![N1盒子刷Armbian，看这一篇就够了——Armbian全方位安装指导书](https://raw.githubusercontent.com/lyducka/pic/master/uPic/188.gif)

[![只输入ip地址，别的不用动](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8185a94d59f3432.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_19/)只输入ip地址，别的不用动

**为了保证不输错，保险起见，你可以看下你的N1**

[![这里面有IP](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8185ea5ca578887.jpg_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_20/)这里面有IP

[![提示你，选是](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e81861044b24235.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_21/)提示你，选是

**选是！选是！选是！****不选是进不去**



[![输入账号密码](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8186708e45c6555.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_22/)输入账号密码

**输入的是你已经改过的账号密码。**





输入代码，安装镜像至EMMC

**这里的命令各家可能不同，咱也不敢多说，按我这条命令输，别玩两天没法进系统了**

**CTRL+C 复制命令后，在命令窗口右键即可复制，一个一个字打容易出错**

> nand–sata-install

它可以完成所有刷写eMMC所需的事情

**出现"Complete copy OS to eMMC"，就代表写入完成了**

完成后，输入命令关机，拔电源也行

> shutdown

关机拔掉U盘，所有刷机工作就结束了。

**这里多说一句，如果你想重刷Armbian系统，那么还是U盘启动Armbian，重新执行nand-sata-install即可完整覆盖掉emmc中的数据，不必重刷安卓再刷Armbian。**

### 第六步：优化**Armbian（是不是还有人是第一次见这个，不优化会时常[开小差](https://pinpai.smzdm.com/58238/)的）**

**1.优化apt源[服务器](https://www.smzdm.com/fenlei/fuwuqi/)**

Debian/Ubuntu的apt源默认服务器在欧洲，到国内速度很慢，需要替换成国内的源才能正常安装一些软件。

首先确认你的 Linux 发行版，我的是Ubuntu 的，若你使用其他镜像，则可能是基于 Debian 的。在你登陆时的欢迎信息中会显示。

[![我的是Ubuntu](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8188937b0852556.jpg_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_23/)我的是Ubuntu

> Welcome to ARMBIAN 5.77 user-built Ubuntu 18.04.2 LTS 5.0.2-aml-s905

**用WinSCP编辑/etc/apt/sources.list**



[![输入ip地址，n1欢迎页上有](https://qnam.smzdm.com/202003/30/5e8189b24285f6167.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_24/)输入ip地址，n1欢迎页上有

[![选是](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8189b21e82b7786.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_25/)选是

[![找到文件](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8189b23c45a9421.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_26/)找到文件

[![复制](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e8189b279ec33413.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_27/)复制

**如图将其中的源网址替换为下面的，还是复制比较保险**

**若是 Ubuntu ，将文件内容替换成以下内容：**



> deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
>
> \#deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic main restricted universe multiverse
>
> deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse
>
> \#deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-security main restricted universe multiverse
>
> deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
>
> \#deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-updates main restricted universe multiverse
>
> deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse
>
> \#deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ bionic-backports main restricted universe multiverse

**若是 Debian，将文件内容替换成以下内容：**



> deb [ arch=arm64,armhf ] https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch main contrib non-free
>
> \#deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch main contrib non-free
>
> deb [ arch=arm64,armhf ] https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch-updates main contrib non-free
>
> \#deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch-updates main contrib non-free
>
> deb [ arch=arm64,armhf ] https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch-backports main contrib non-free
>
> \#deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ stretch-backports main contrib non-free
>
> deb [ arch=arm64,armhf ] https://mirrors.tuna.tsinghua.edu.cn/debian-security/ stretch/updates main contrib non-free
>
> \#deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security/ stretch/updates main contrib non-free
>
> \#deb [ arch=arm64,armhf ] https://mirrors.tuna.tsinghua.edu.cn/debian/ sid main contrib non-free

**修改完成后，putty执行更新命令更新软件和软件包，不怕麻烦可以直接n1**



> apt-get update
>
> apt-get upgrade

**2.设置时区为北京时间**

> ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone

**3.网络配置（该部分的关闭默认DNS来源于@Luminous）**

**首先关闭ARMBIAN的默认DNS，让它跟随路由分配**

> rm /etc/resolvconf/resolv.conf.d/head && touch /etc/resolvconf/resolv.conf.d/head

**putty进入ssh，在ssh连接后，执行**

> nmtui

**选择第二个Activate a connection，按提示选择并输入密码即可**

**首先我们用ifconfig指令查看系统当前的MAC并记录下来**

[![@Luminous](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e818bde122145207.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_28/)@Luminous

然后用winscp编辑网卡etc/network/interfaces，在iface eth0 inet dhcp下添加一行并保存

[![找到文件](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e818c5c2d8b05197.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_29/)找到文件

[![添加命令](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e818ce43d08a2569.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_30/)添加命令

> pre-up ifconfig eth0 hw ether 1A:33:E6:90:1F:27

**其中1A:33:E6:90:1F:27替换为你之前保存的MAC信息，重启N1后MAC即固定为此值**

[![@Luminous](https://raw.githubusercontent.com/lyducka/pic/master/uPic/5e818ba6232db5152.png_e1080.jpg)](https://post.smzdm.com/p/alpwnxmp/pic_31/)@Luminous

**4.配置时区**

执行armbian-config，选择personal，然后配置Timezone为chongqing即可。

**全部优化完毕，好累，以上就是指导书的全部内容了**

**想要备份的话请参考我第一篇N1文章。![N1盒子刷Armbian，看这一篇就够了——Armbian全方位安装指导书](https://raw.githubusercontent.com/lyducka/pic/master/uPic/185.gif)**

