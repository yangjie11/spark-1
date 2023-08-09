# smart vscode 插件

RT-Thread 官方提供了 vscode 插件用来开发用户 app 代码。下面分享一下 Vscode 插件 ART-Pi Smart 的使用体验。

### 安装插件

打开 vscode，在扩展中搜索 RT-Thread Smart

![00.png](figures/4.png)

### 创建工程

- 使用快捷键 `ctrl + shift + p`, 选择创建 RT-Thread Smart 工程

  ![01.png](figures/5.png)

- 选择 RT-Thread Smart SDK 的根目录，回车

  ![02.png](figures/6.png)

  gitee 下载下来的源码目录结构如下

  ![03.png](figures/7.png)

- 新建一个 `hello`工程，回车

  ![04.png](figures/8.png)

- 选择编译工具，目前 windows 下只有 scons。如果开发环境中没有安装 scons，需要使用命令 `pip install scons ` 安装 scons 工具。

  ![05.png](figures/9.png)

- 通过上面的步骤，一个用户 app 示例就创建完成了

  ![06.png](figures/10.png)

### 下载用户态代码

vscode 支持下载用户代码到 Smart 开发板。

- 在终端输入命令 `ifconfig` 获取开发板 IP 地址。这里的 MAC 在代码中是写死的，不知道为什么要使用这种方法。如果你的局域网中插入两块 Smart 开发板，可能会有问题，因为这两块开发板的 IP 地址是一样的。

  ```shell
  msh />ifconfig
  network interface device: e1 (Default)
  MTU: 1500
  MAC: a8 5e 45 91 92 93
  FLAGS: UP LINK_UP INTERNET_UP DHCP_ENABLE ETHARP BROADCAST IGMP
  ip address: 192.168.110.34
  gw address: 192.168.110.1
  net mask  : 255.255.255.0
  ipv6 link-local: FE80::AA5E:45FF:FE91:9293 VALID
  ipv6[1] address: 0.0.0.0 INVALID
  ipv6[2] address: 0.0.0.0 INVALID
  dns server #0: 211.136.150.66
  dns server #1: 211.136.112.50
  
  network interface device: w0
  MTU: 1500
  MAC: fc 58 4a 2c 50 01
  FLAGS: UP LINK_DOWN INTERNET_DOWN DHCP_ENABLE ETHARP BROADCAST IGMP
  ip address: 0.0.0.0
  gw address: 0.0.0.0
  net mask  : 0.0.0.0
  ipv6 link-local: FE80::FE58:4AFF:FE2C:5001 VALID
  ipv6[1] address: 0.0.0.0 INVALID
  ipv6[2] address: 0.0.0.0 INVALID
  dns server #0: 211.136.150.66
  dns server #1: 211.136.112.50
  
  network interface device: w1
  MTU: 1500
  MAC: fc 58 4a 2c 50 00
  FLAGS: UP LINK_DOWN INTERNET_DOWN DHCP_ENABLE ETHARP BROADCAST IGMP
  ip address: 0.0.0.0
  gw address: 0.0.0.0
  net mask  : 0.0.0.0
  ipv6 link-local: FE80::FE58:4AFF:FE2C:5000 VALID
  ipv6[1] address: 0.0.0.0 INVALID
  ipv6[2] address: 0.0.0.0 INVALID
  dns server #0: 211.136.150.66
  dns server #1: 211.136.112.50
  ```

- 编译代码

  ![08.png](figures/11.png)

- 设置 IP 地址，使用快捷键 `ctrl + shift + p` 选择打开 RT-Thread Smart 设置

  ![09.png](figures/124.png)

  填入开发板 IP 地址，保存设置并退出

  ![010.png](figures/132.png)

- 下载代码

  ![011.png](figures/14.png)

  当终端显示以下界面时，表示代码下载成功

  ![012.png](figures/15.png)

- 运行可执行文件

  ![013.png](figures/16.png)

### 调试用户态代码

- 使用快捷键 `F5` 进入调试模式，可以在代码处设置断点

  ![014.png](figures/17.png)

- 可以使用快捷键 `F11`  进行单步调试

### UDB 工具

在下载代码和调试代码时，在终端发现 vscode smart 插件使用的是一个叫 **udb** 的工具。udb 工具位于 **ART-Pi-smart\tools\udb-tools**。RT-Thread 提供了windows 和 linux 两个版本的 udb 工具。

![015.png](figures/18.png)

windows 下目录结构：

- server.log ：udb 的运行日志
- udb.exe    ：可执行文件
- udb.ini      ：tcp 配置文件，保存了 smart 开发板的 ip  地址

#### udb 命令

在该目录下打开 windows 终端，输入命令 `udb.exe --help ` 查看 udb 命令。

![016.png](figures/19.png)

#### udb devices

查看当前 udb 链路设备

```bash
PS D:\repo\gitee\ART-Pi-smart\tools\udb-tools\windows> .\udb.exe devices
List of devices attached
serial              mtu            version        state
192.168.110.34:5555 1024           2.0.0          active
```

#### udb push

推送本地文件到远端设备

![7.gif](figures/20.gif)

![018.png](figures/21.png)

#### udb pull

拉取远端文件到本地

![8.gif](figures/22.gif)