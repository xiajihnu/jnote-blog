# Windows装机技巧

Last updated August 20, 2025

---

笔者平时会有更换系统的需求，本文主要记录系统装机过程中的各种小tips和必要的资源信息，以备查阅。
![猫猫图片](cat1.png)

## Windows 系统和软件下载资源

[MSDN, 我告诉你](https://msdn.itellyou.cn/) 是一个收集了各种 Windows 系统和相关软件的服务器，提供磁力链下载，非常好用。

## Windows 11 系统安装

1. 想要绕开Win11的硬件检测，可以使用rufus制作启动盘，开始制作前，选择绕开检测即可，同时需要注意，用于制作启动盘的存储介质需要8G以上的容量。

2. 安装过程中，按下Shift+F10可开启CMD窗口，输入”start ms-cxh:localonly”，即可开始设置本机离线账户密码

3. 安装过程中，按下Shift+F10可开启CMD窗口，输入”regedit”，即可打开注册表编辑器，找到”HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\OOBE”。将”HideOnlineAccountScreens”的值改成’1’。此方法适用于Windows 11 Home及Pro版本。（此方法尚未验证，据说是用于企业无人值守安装，跳过联网登录过程）

## Windows 系统去水印

1. 方案一
使用打开windows系统自带的powershell或CMD命令行窗口，执行以下命令：
```
slmgr /skms kms.rbg.kr
```
此方法经过验证，可适用于Win10，需要验证是否适合Win11。

2. 方案二：
以管理员模式启动PowerShell，执行以下命令：
```
irm https://massgrave.dev/get | iex

```
不过上面这条命令似乎将要被弃用了，后续可以用下面的命令

```
irm https://get.activated.win | iex
```
此方法经过验证，可适用于Win10/Win11和office 365的激活。

