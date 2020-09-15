## 1.测试脚本
### 1.1 traceroute脚本

```
wget https://zhujiwiki.com/wp-content/uploads/2018/05//linuxtest.sh -N --no-check-certificate && bash linuxtest.sh

wget https://raw.githubusercontent.com/chiakge/Linux-Server-Bench-Test/master/linuxtest.sh -N --no-check-certificate && bash linuxtest.sh 
```
### 1.2 Superbench脚本
```
wget -qO- https://raw.githubusercontent.com/oooldking/script/master/superbench.sh | bash
```
### 1.3 柠檬
```
wget -qO- https://ilemonrain.com/download/shell/LemonBench.sh | bash -s full
```
## 2、端口转发
```
wget https://zhujiwiki.com/wp-content/uploads/2018/12/socat.sh && bash socat.sh

文件传输

scp -P 22 /home/wwwroot/default/metars.sql root@51.158.150.165:/root
```
纯IPV6环境搭建
vi /etc/hosts
装lnmp一键包，加这条hosts
2600:3c01::f03c:91ff:fe92:1a06 soft.vpser.net
装宝塔，加这一条(自己弄的反代，介意别用
2a01:4f8:191:48c:2018:2019:0:1f9 download.bt.cn

## 3、[transmission安装](https://lala.im/3024.html)
### 3.1 一键rt/ru
```
wget https://lala.im/static/script/rTorrent0.9.7CentOS7install.sh && chmod +x rTorrent0.9.7CentOS7install.sh
./rTorrent0.9.7CentOS7install.sh
```
### 3.2 或者yum安装tr
```
yum -y install epel-release
yum -y install transmission-daemon
```
虽然Transmission的安装非常简单，但是Transmission有一些反人类的设置，比如配置文件必须要首次启动软件后才会生成。所以这里，我们要先启动：
```
systemctl start transmission-daemon
```
其次，默认的配置文件并不是最佳的配置，尤其是当我们使用WEBGUI的时候，肯定是需要自己修改的，但是Transmission的配置文件你想让更改生效就必须要先停止Transmission的运行，所以这里我们还要把刚启动的Transmission给停止运行：
```
systemctl stop transmission-daemon
```
看到这里，是不是觉得Transmission这玩意是真有点蛋疼啊，没事，更蛋疼的还在后面。
查找一下配置文件到底在哪里：
```
find / -name settings.json
vi /var/lib/transmission/.config/transmission-daemon/settings.json
```
找到了，路径如下：
/var/lib/transmission/.config/transmission-daemon/settings.json
编辑它
```
vi /var/lib/transmission/.config/transmission-daemon/settings.json
```
需要更改的地方如下图红框所示：
```
1、rpc-authentication-required的值改为true。
2、rpc-host-whitelist-enabled的值改为false。
3、rpc-password请设置一个高强度的密码。
4、rpc-username是你的WEBGUI登录用户名，这里可以自定义填写。
5、rpc-whitelist-enabled的值改为false。
OK，配置文件改完后，我们现在重新运行Transmission：
systemctl start transmission-daemon
```
现在通过浏览器访问你的VPS公网IP+端口9091应该就可以访问到Transmission的WEB界面了，并且是需要进行登录验证的，而用来验证的账号密码就是刚才在配置文件内设置的：
## 4、[PLEX安装](https://lala.im/3010.html)
最新的RPM安装包可在官网上找到：https://www.plex.tv/downloads/
首先我们来安装Plex，在CentOS7上安装Plex非常简单，只需要两条命令即可：
```
wget https://downloads.plex.tv/plex-media-server/1.12.3.4973-215c28d86/plexmediaserver-1.13.0.5023-31d3c0c65.x86_64.rpm
yum -y install plexmediaserver-1.15.4.994-107756f7e.x86_64.rpm
```

```
安装完成后就可以启动Plex了：
systemctl start plexmediaserver

输入如下命令查看运行状态，请确保是Active：
systemctl status plexmediaserver

把Plex加入开机启动：
systemctl enable plexmediaserver
```
请先去官网注册一个Plex账号：

请注意由于Plex初次安装完成后默认是不允许远程客户端连接的，也就是说我们现在虽然把Plex安装在服务器上了，但现在也就只能通过这台服务器本地来使用Plex，如果我们使用和这台服务器不同网络的其他客户端，比如PC客户端、安卓客户端、iOS客户端等等，只要和这台服务器不是同一个网络，我们都是无法连接上的。
所以为了让我们能够正常在各个客户端上使用Plex，这里我们需要建立一条SSH隧道。
本文使用Xshell来建立，首先找到你当前终端的会话设置，按下图来设置：

接着按照提示来设置端口号：
注：
1、侦听端口这里可以随便填写，只要不和你本地电脑上的端口有冲突就行。但目标端口请一定设置为32400。
2、要让SSH隧道生效，必须关闭Xshell然后重新启动并连接。
现在我们打开浏览器输入如下网址：
localhost:15888/web
不出意外的话，我们现在就能通过这个地址访问到我们服务器上的Plex了，登录我们刚注册好的账号，可以看到Plex的欢迎界面了：

接着Plex会告诉我们发现了一个服务器，请注意一定要勾选如图红框所标注的按钮：然后我们开始添加资料库，这里选择其他影片：
现在我们就可以关闭SSH隧道，直接使用服务器的公网IP+端口32400来访问Plex了：
至此，Plex的安装和配置就大功告成了，开始使用你的客户端来连接这台服务器吧~
由于我里面存的都是小姐姐资源，这里就不是很好截图了。。。但是Plex界面真的是挺好看的。

搭媒体服务器，除了硬盘要够大以外，网络也一定要快才行，
## 5、Rclone 挂载
```
yum -y install wget unzip screen fuse fuse-devel

curl -O https://downloads.rclone.org/rclone-current-linux-amd64.zip

cd  rclone-v1.49.0-linux-amd64
```
运行Rclone开始配置：
```
./rclone config
```
第一步选择n，然后回车输入一个name，建议这个name设置的简单好记一点，如图所示：gd

接着client_id、client_secret、service_account_file都留空直接回车，Use auto config?这里我们选择n，如图所示：
现在rclone会在终端内给我们回显一个GoogleDrive的授权登录地址，如图所示：
接着复制如下图所示的授权代码：
回到终端内粘贴授权代码然后回车，继续按如下图操作，依次输入n、y、q：
全部完成后，现在新建一个你要挂载的目录：
```
mkdir -p /gdsw/gdsw
```
用screen创建一个新的会话：
```
screen -S rclone
```
执行如下命令：
```
./rclone mount gd: /gdsw/gdsw --allow-other --allow-non-empty --vfs-cache-mode writes

./rclone mount gd: /gd/gdsw --allow-other --allow-non-empty --vfs-cache-mode writes
```
不出意外的话，就挂载成功了：
```
rclone mount DriveName:Folder LocalFolder --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
df -h
```
Q：VPS重启后挂载盘就没了？
默认是这样子的，当然你可以把挂载命令写到开机启动项里面，或者如果你用的Rclone可以写一个服务来启动。假设这里我用的Rclone挂载的，服务可以这样子写：
先把rclone的可执行文件复制到/usr/bin：
```
cp /root/rclone-v1.49.0-linux-amd64/rclone /usr/bin/gdswrclone
```
新建一个rclone.service文件：
```
vi /usr/lib/systemd/system/gdswrclone.service
```
```
[Unit]
Description=gdswrclone
    
[Service]
User=root
ExecStart=/usr/bin/gdswrclone mount gd: /gdsw/gdsw --allow-other --allow-non-empty --vfs-cache-mode writes
Restart=on-abort
    
[Install]
WantedBy=multi-user.target
```
重载daemon，让新的服务文件生效：
```
systemctl daemon-reload
```
现在就可以用systemctl来启动rclone了：
```
systemctl start gdswrclone
```
设置开机启动：
```
systemctl enable gdswrclone
```
停止、查看状态可以用：
```
systemctl status gdswrclone
systemctl stop gdswrclone
```
重启你的VPS，然后查看一下rclone的服务起来没，接着查看一下盘子挂上去没：
```
reboot
df -h
```
OD
```
mkdir -p /od/odp
```
用screen创建一个新的会话：
```
screen -S rclone
```
执行如下命令：
```
./rclone mount od: /od/odp --allow-other --allow-non-empty --vfs-cache-mode writes

./rclone mount od: /od/odp --allow-other --allow-non-empty --vfs-cache-mode writes
```
不出意外的话，就挂载成功了：
```
rclone mount DriveName:Folder LocalFolder --copy-links --no-gzip-encoding --no-check-certificate --allow-other --allow-non-empty --umask 000
df -h
```
Q：VPS重启后挂载盘就没了？
默认是这样子的，当然你可以把挂载命令写到开机启动项里面，或者如果你用的Rclone可以写一个服务来启动。假设这里我用的Rclone挂载的，服务可以这样子写：
先把rclone的可执行文件复制到/usr/bin：
cp /root/rclone-v1.42-linux-amd64/rclone /usr/bin/rclone1
新建一个rclone.service文件：
```
vi /usr/lib/systemd/system/rclone1.service

[Unit]
Description=rclone1
    
[Service]
User=root
ExecStart=/usr/bin/rclone1 mount od: /od/odp --allow-other --allow-non-empty --vfs-cache-mode writes
Restart=on-abort
    
[Install]
WantedBy=multi-user.target
```

继续进行类似gd的操作

## 6、安装bt
```
安装
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install.sh && sh install.sh
更新
wget -O update.sh http://download.bt.cn/install/update.sh && sh update.sh
```

