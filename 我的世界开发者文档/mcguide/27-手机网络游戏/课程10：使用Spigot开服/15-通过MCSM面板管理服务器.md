# 通过MCSM面板管理服务器

![封面](/dev/mcmanual/mc-dev/assets/img/10.8bb6e2d6.png)

## 准备阶段

  1. PE/PC网络服入驻通过，并且拿到网易开发机/正式机。
  2. 自备服务器（非网易机器），最低要求1c1g，建议安装宝塔
  3. 下载所需要的文件（相关文件已上传内容库）
  4. 完成上一章节《部署服务器》
  5. MCSM面板中文指南：<https://docs.mcsmanager.com/zh_cn/>[ (opens new window)](<https://docs.mcsmanager.com/zh_cn/>)


## 特别提示

本教程不会在网易机器部署web面板及自建服务。

本教程将默认开发者已经熟悉MCSM面板，基础内容不再介绍。

开始前请熟知并遵守《高危操作警告说明》：

  * 网易机器禁止私自开放附录名单内端口对外

  * 网易机器禁止私自把无任何认证校验服务直接对外

  * 网易机器禁止私自把机器交付时开放的业务端口用于其他用途，如数据库或自建服务等


![《高危操作警告说明》文档](/dev/mcmanual/mc-dev/assets/img/20.f40b8a0e.png)

## 原理说明

在网易机器部署mcsm daemon守护进程，通过自备机器协议转发SSH隧道与网易机器通信，从而达到使用mcsm面板管理网易机器。

![原理图](/dev/mcmanual/mc-dev/assets/img/30.11c60861.png)

## 操作步骤

### （一）在网易机器

#### 1\. 无root安装node.js

**强制要求安装node16+的版本，不然守护进程跑不起来**
```python
    # 下载nodejs
    cd ~/downloads
    wget https://nodejs.org/dist/v16.20.2/node-v16.20.2-linux-x64.tar.xz
    
    # 创建工作目录并解压
    mkdir -p ~/apps/node-v16.20.2
    tar -xJf node-v16.20.2-linux-x64.tar.xz --no-wildcards-match-slash --anchored --exclude */CHANGELOG.md --exclude */LICENSE --exclude */README.md  --strip 1 -C ~/apps/node-v16.20.2
    
    # 添加path变量
    export PATH=~/apps/node-v16.20.2/bin:$PATH
    source ~/.bashrc
    
    # 检查nodejs版本
    node -v
    npm -v
    
    # 至此安装完成
```
#### 2\. 守护进程mcsm daemon上传部署

从github下载压缩包（内容库也已上传）

<https://github.com/MCSManager/MCSManager/releases/latest/download/mcsmanager_linux_daemon_only_release.tar.gz>[ (opens new window)](<https://github.com/MCSManager/MCSManager/releases/latest/download/mcsmanager_linux_daemon_only_release.tar.gz>)
```python
    # 创建mcsm daemon工作目录
    cd ~
    mkdir -p ~/mcsm
    
    # 上传到这个目录（Xftp, FinalShell均可）
    
    # 解压
    cd ~/mcsm
    tar -zxvf mcsmanager_linux_daemon_only_release.tar.gz
    cd ~/mcsm/mcsmanager
    
    # 设置 npm 镜像源为国内淘宝源
    npm config set registry https://registry.npmmirror.com
    npm config get registry
    
    # 新增screen
    screen -S mcsm
    
    # 安装依赖后部署
    sh start-daemon.sh
```
#### 3\. 检查24444端口和秘钥

执行上一步骤后，服务将会启动成功，请使用**ctrl+a、d** 来后台运行screen

启动成功后，你会看到类似 [INFO] Key: xxxxxxx 和 Port: 24444 的信息。

（记录key秘钥，后续将用于自备机器节点配置。）
```python
    # 检查端口是否正常
    netstat -tulnp | grep 24444
```
### （二）在自备机器

#### 1\. 自动安装mcsm+开启守护进程
```python
    # 请确保root登录，有sudo权限
    sudo su -c "wget -qO- https://script.mcsmanager.com/setup_cn.sh | bash"
    
    # 先启动面板守护进程。
    # 这是用于进程控制，终端管理的服务进程。
    systemctl start mcsm-daemon.service
    # 再启动面板 Web 服务。
    # 这是用来实现支持网页访问和用户管理的服务。
    systemctl start mcsm-web.service
    
    # 以下命令已列出（如有需要请自取）
    # 重启面板命令
    systemctl restart mcsm-daemon.service
    systemctl restart mcsm-web.service
    
    # 停止面板命令
    systemctl stop mcsm-web.service
    systemctl stop mcsm-daemon.service
```
如果 systemctl 命令无法启动面板，可以参考手动安装（<https://docs.mcsmanager.com/zh_cn/>[ (opens new window)](<https://docs.mcsmanager.com/zh_cn/>)） 中的 启动方式 来启动 MCSManager。 但这需要你用其他后台运行程序来接管它，否则当你的 SSH 终端断开之时，手动启动的 MCSManager 面板也会随之被系统强制结束。

面板 Web 服务是提供用户管理与网页访问功能的服务，守护进程是提供进程管理和容器管理的服务，两者缺一不可。如果某个功能不正常，可以只重启这一部分的服务来热修复问题。

#### 2\. 配置协议转发（autossh）

下面将以宝塔面板进行操作：

① 进入/root/.ssh/目录上传网易机器秘钥

② 配置秘钥密码和隧道转发
```python
    # 设置秘钥权限（请自行替换）
    chmod 600 /root/.ssh/netease.key
    
    # 启动 ssh-agent
    eval `ssh-agent -s`
    
    # 添加密钥并输入一次密码（请自行替换）
    ssh-add /root/.ssh/netease.key
    
    # 如无autossh，请自行安装
    apt-get update
    apt-get install autossh -y
    
    # 执行隧道命令（请自行替换）
    # 将ssh隧道连接的网易机器24444端口转发到自备机器的24445端口
    # 注意 -L 后面的 0.0.0.0:24445
    autossh -M 0 -o "ServerAliveInterval 30" -o "ServerAliveCountMax 3" -p 32200 -i /root/.ssh/netease.key -CNf -L 0.0.0.0:24445:127.0.0.1:24444 fuzhu@<网易机器IP>
    
    # 验证是否成功
    netstat -tunlp | grep 24445
    
    # 如果你看到类似下面的输出：
    # tcp 0 0 127.0.0.1:24445 0.0.0.0:* LISTEN 1234/ssh
    # 那么就成功了
    
    # 至此隧道转发结束
```
③ 防火墙和安全组开放端口（仅在自备机器，网易机器禁止开放端口）

mcsm面板web端口（建议nginx转发改端口，安全一些） | 23333  
---|---  
**ssh协议转发端口（设置端口白名单，否则有安全问题）** | 24445  
  
#### 3\. 配置mcsm节点

![节点位置](/dev/mcmanual/mc-dev/assets/img/40.86b8b113.png)

![节点配置](/dev/mcmanual/mc-dev/assets/img/50.002a56c0.png)

这里的“远程节点秘钥”请替换为网易机器守护进程输出的key秘钥。

添加后，显示节点状态为正常，网页直连为正常，可以读取内存和处理器使用率后节点配置成功。

#### 4\. 检查实例创建和文件管理是否正常

如需迁移网易机器的screen至mcsm，请使用“直接创建”，配置服务端目录、实例类型和启动命令后，即可迁移成功。

![创建实例](/dev/mcmanual/mc-dev/assets/img/60.fa844533.png) ![文件管理](/dev/mcmanual/mc-dev/assets/img/70.14cb57a1.png)

#### 5\. 常见的问题

① 无法连接到远程节点

检查自备机器端口/安全组是否开放，是否需要配置nginx反向代理。

最简单的方法就是直接复制地址，浏览器访问，看是否有显示daemon状态。

![无法连接到远程节点](/dev/mcmanual/mc-dev/assets/img/80.3fe21c08.png) ![守护进程状态](data:image/png;base64,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)

② 多网易机器部署

按照此方法，在不同机器上配置mcsm daemon守护进程，配置多个SSH隧道协议转发，然后新增节点即可。

③ mcsm面板传输文件出现错误

显示network error

解决方法：升级daemon为最新版，不要使用3.x的版本

## 特别鸣谢

教程作者：初云

灵感和帮助：Soldier

全过程步骤指导：Gemini3 Pro

教程参考指南：MCSManager团队

MCSM常见问题解答：混合、封神、MuFeng、西瓜、星汉

调试与测试：千阙云庭服务器团队、初云杯服务器团队、风之谷服务器团队

如您在操作过程中有困难，欢迎您随时联系我们~

感谢帮助过我们的人，期待网络服环境蒸蒸日上！

入门

60分钟