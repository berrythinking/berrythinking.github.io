---
layout: post
title: 利用海外VPS下载 youtube 高清视频并上传至多个网盘
categories: [IT&数码]
description: y2mate 为 youtube 视频转码很不错，但是下载的时候如果不能用 expressvpn 就会一直没有速度，导出 aria2 也是一样，多方研究发现可以用海外的 vps 加速下载。
keywords: Google, AWS, youtube, google drive, 百度云, onedrive, ffmpeg, aria2, 1080P
---
一直用 ```y2mate``` 从 youtube 下载1080P 高清视频，但是最近下载的时候 expressvpn 不能用了， 一直没有速度，导出 aria2 也是一样，多方研究发现可以用海外的 vps 加速下载。

# 安装 Youtube-dl
```
sudo wget https://yt-dl.org/downloads/latest/youtube-dl -O /usr/local/bin/youtube-dl
sudo chmod a+rx /usr/local/bin/youtube-dl
```
之后如果需要升级```youtube-dl```可以使用如下命令
```
sudo -H pip install --upgrade youtube-dl
```
# CentOS 中安装 ffmpeg
## 1.升级系统
```
sudo yum install epel-release -y
sudo yum update -y
```
## 2.安装必要组件
Centos：
```
yum install gcc gcc-c++ autoconf automake

yum -y install zlib zlib-devel openssl openssl-devel pcre pcre-devel
```
ubuntu
```
apt-get install gcc build-essential
```
系统中没有```yasm```的话也需要安装一下，懒人```yum```大法
CentOS
```
yum install yasm
```
## 3.安装Nux Dextop Yum 源
由于CentOS没有官方```FFmpeg rpm```软件包。但是，我们可以使用第三方YUM源```Nux Dextop```完成此工作。

```
sudo rpm –import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
sudo rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
```
## 4.安装FFmpeg 和 FFmpeg开发包
如果你不打算下载1080P 及以上分辨率的话，是没必要安装FFmpeg的。
```
sudo yum install ffmpeg ffmpeg-devel -y
```
## 5.测试是否安装成功
```
ffmpeg
```

# 下载 Youtube 视频

下载 youtube 的视频就很简单了，比如下面这条命令，会自动下载最高分辨率的视频

```
youtube-dl https://www.youtube.com/watch?v=SLaYPmhse30
```

但是，天杀的 youtube 1080P 分辨率及以上的视频，他们的音视频是分开的，所以，如果我们想下载高分辨率的时候，得把音频也下载下来，所以，得先看看该视频所有的可选的分辨率及音频：
```
youtube-dl -F https://www.youtube.com/watch?v=SLaYPmhse30
```

![11111111111.jpg](http://www.138vps.com/content/uploadfile/201709/dd0b1504928314.jpg)

结果如图，请注意 format-code：

audio only就是仅音频

video only就是仅视频


如果你只是想下载720P的视频，那么通过观察上图，下载的时候把 format-code 加上即可：
```
youtube-dl -f 22 https://www.youtube.com/watch?v=SLaYPmhse30
```

如果你想下载 1080P 的，那么除了下载视频外，还要把音频也下载下来，因为 youtube-dl 会自动调用 ffmpeg 合并音视频，所以我们只需下载就可以了，下载完成后，youtube-dl 会自动调用 ffmpeg 把音视频合并。
```
youtube-dl -f 137+140 https://www.youtube.com/watch?v=SLaYPmhse30
```

此外，youtube-dl 还可以下载整个视频列表，也可以下载字幕，功能相当强大，具体请自己通过百度或者谷歌进行下一步学习。

ffmpeg 的功能也非常强大，如果感兴趣，自己百度吧。

## 不足
虽然 youtube-dl 下载视频非常方便，但是缺点也很明显，就是只能单线程下载，找遍youtube-dl给的参数，都没发现自带有多线程。

这时候就需要我们和 Aria2 组合使用，实现多线程下载，会有更快的下载速度，节约等待时间。

# Aria2
## 一、安装aria2
### 1、安装aria2的方法很简单，一条命令就够了

centos 7：
```
yum install epel-release

yum install aria2
```

ubuntu：
```
apt-get install aria2
```
测试是否安装成功
```
aria2c 文件地址
```
![aria2下载](https://pic.mikucdn.com/wp-image/2019/01/20190114143245.png)
若出现如图所示的结果则说明Aria2已安装成功。

### 2、下载安装好了以后，远程下载文件就很简单了

单个文件下载
```
aria2c http://example.org/mylinux.iso
```
从两个来源（更多也可以）
```
aria2c http://a/f.iso ftp://b/f.iso
```
BitTorrent
```
aria2c http://example.org/mylinux.torrent
```
BitTorrent Magnet URI
```
aria2c 'magnet:?xt=urn:btih:248D0A1CD08284299DE78D5C1ED359BB46717D8C'
```
Metalink
```
aria2c http://example.org/mylinux.metalink
```
文本文件uri.text中的链接(URI)
```
aria2c -i uri.txt
```
显示种子中包含了哪些文件
```
aria2c -S bit.torrent
```
![aria2](https://pic1.zhimg.com/80/v2-3ed8cb46671da8b86ddcd0d656067ad8_hd.jpg)


### Youtube-dl 调用 Aria2 下载
Youtube-dl调用外部Aria2多线程下载工具的方法非常简单，我们就以这个视频地址为例吧：https://www.youtube.com/watch?v=LVBM8Gv3mSo

一般来说，使用这条命令就够了（如果你安装了FFmpeg，会直接下载4K分辨率）
```
youtube-dl    https://www.youtube.com/watch?v=LVBM8Gv3mSo   --external-downloader aria2c --external-downloader-args "-x 16  -k 1M"
```

参数说明：
```
--external-downloader aria2c     //调用外部下载工具aria2

--external-downloader-args      //外部下载工具指定参数

-x 16      //启用aria2 16个线程，最多就支持16线程

-K 1M      //指定块的大小

-f 22      //下载油管720p视频，用法请参考 
```
![youtube-dl](http://www.138vps.com/content/uploadfile/201807/c5361531961979.jpg)

我一般会配合 -P 参数来查看下载进度。

# VPS 与国外网盘之间进行文件传输
下载完成之后我们需要把他传到自己的网盘里面，如果是国外的网盘（主要是 google drive 和微软的 onedrive)，推荐使用 Rclone.

# 安装并设置Rclone
## 1、安装rclone
```
curl https://rclone.org/install.sh | sudo bash
```
或者使用下面命令安装```beta```版
```
curl https://rclone.org/install.sh | sudo bash -s beta
```
## 2、初始化配置
```
rclone config
```
会出现以下信息：
```
n) New remote
s) Set configuration password
q) Quit config
n/s/q> n
name> Berry  #配置名称，随便填
Type of storage to configure.
Enter a string value. Press Enter for the default ("").
Choose a number from below, or type in your own value
 1 / 1Fichier
   \ "fichier"
 2 / Alias for an existing remote
   \ "alias"
 3 / Amazon Drive
   \ "amazon cloud drive"
 4 / Amazon S3 Compliant Storage Provider (AWS, Alibaba, Ceph, Digital Ocean, Dreamhost, IBM COS, Minio, etc)
   \ "s3"
 5 / Backblaze B2
   \ "b2"
 6 / Box
   \ "box"
 7 / Cache a remote
   \ "cache"
 8 / Dropbox
   \ "dropbox"
 9 / Encrypt/Decrypt a remote
   \ "crypt"
10 / FTP Connection
   \ "ftp"
11 / Google Cloud Storage (this is not Google Drive)
   \ "google cloud storage"
12 / Google Drive
   \ "drive"
13 / Google Photos
   \ "google photos"
14 / Hubic
   \ "hubic"
15 / JottaCloud
   \ "jottacloud"
16 / Koofr
   \ "koofr"
17 / Local Disk
   \ "local"
18 / Mega
   \ "mega"
19 / Microsoft Azure Blob Storage
   \ "azureblob"
20 / Microsoft OneDrive
   \ "onedrive"
21 / OpenDrive
   \ "opendrive"
22 / Openstack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
   \ "swift"
23 / Pcloud
   \ "pcloud"
24 / Put.io
   \ "putio"
25 / QingCloud Object Storage
   \ "qingstor"
26 / SSH/SFTP Connection
   \ "sftp"
27 / Union merges the contents of several remotes
   \ "union"
28 / Webdav
   \ "webdav"
29 / Yandex Disk
   \ "yandex"
30 / http Connection
   \ "http"
31 / premiumize.me
   \ "premiumizeme"
Storage> 12  #选择12，Google Drive
Google Application Client Id - leave blank normally.
client_id>  #留空 
Google Application Client Secret - leave blank normally.
client_secret>  #留空
Service Account Credentials JSON file path - needed only if you want use SA instead of interactive login.
service_account_file>  #留空
Choose a number from below, or type in your own value
 1 / Full access all files, excluding Application Data Folder.
   \ "drive"
 2 / Read-only access to file metadata and file contents.
   \ "drive.readonly"
   / Access to files created by rclone only.
 3 | These are visible in the drive website.
   | File authorization is revoked when the user deauthorizes the app.
   \ "drive.file"
   / Allows read and write access to the Application Data folder.
 4 | This is not visible in the drive website.
   \ "drive.appfolder"
   / Allows read-only access to file metadata but
 5 | does not allow any access to read or download file content.
   \ "drive.metadata.readonly"
scope> 1 
ID of the root folder
Leave blank normally.
Fill in to access "Computers" folders. (see docs).
Enter a string value. Press Enter for the default ("").
root_folder_id> 
Service Account Credentials JSON file path 
Leave blank normally.
Needed only if you want use SA instead of interactive login.
Enter a string value. Press Enter for the default ("").
service_account_file> 
Edit advanced config? (y/n)
y) Yes
n) No
y/n> n  #输入n
Remote config
Use auto config?
 * Say Y if not sure
 * Say N if you are working on a remote or headless machine
y) Yes
n) No
y/n> n  #输入n
If your browser doesn't open automatically go to the following link: https://accounts.google.com/o/  #打开该地址获取code
Log in and authorize rclone for access
Enter verification code>hjdd  #输入你获取到的code
Configure this as a team drive?
y) Yes
n) No
y/n> n  #输入n
--------------------
[Berry]
type = drive
client_id = 85042871
client_secret = D72gPc
scope = drive
token = {"access_token":"y902Z"}
--------------------
y) Yes this is OK
e) Edit this remote
d) Delete this remote
y/e/d> y  #输入y
Current remotes:

Name                 Type
====                 ====
Berry                drive

e) Edit existing remote
n) New remote
d) Delete remote
r) Rename remote
c) Copy remote
s) Set configuration password
q) Quit config
e/n/d/r/c/s/q> q  #输入q保存退出
```
如果是要连接 onedrive 的话，那么需要在本地安装 Rclone 并使用 ```rclone authorize "onedrive" ```并获取授权码。

![onedrive-authorize](https://omo.moe/usr/uploads/2019/02/1662802672.png)

之后会弹出浏览器，要求输入微软的账号和密码登录。

![onedrive-login](https://omo.moe/usr/uploads/2019/02/3146682821.png)

点击同意

![onedrive-agree](https://omo.moe/usr/uploads/2019/02/108776614.png)

返回成功页面

![onedrive-success](https://omo.moe/usr/uploads/2019/02/1787560041.png)

之后本地终端会获取到授权码

![onedrive-code](https://omo.moe/usr/uploads/2019/02/1787560041.png)

粘贴到 vps 窗口中并回车

![onedrive-confirm](https://omo.moe/usr/uploads/2019/02/1662802672.png)

之后就可以使用 rclone 命令来进行 vps 与网盘之间的文件传输了。

# VPS 与百度网盘进行数据传输

BYPY一个百度云/百度网盘的Python客户端。主要的目的就是在Linux环境下（Windows下应该也可用，但没有仔细测试过）通过命令行来使用百度云盘的2TB的巨大空间。它提供文件列表、下载、上传、比较、向上同步、向下同步，等操作。

由于百度PCS API权限限制，程序只能存取百度云端/apps/bypy目录下面的文件和目录。

据说百度PCS API最多返回目录下1000个文件（#306)，如果属实，百度云盘上若有超过1000个文件的目录，将有一部分文件无法被看到/下载。

Github地址：[Github](https://github.com/houtianze/bypy)

百度云PCS API地址：[点击进入](http://developer.baidu.com/wiki/index.php?title=docs/pcs/rest/file_data_apis_list)

## 安装
系统要求：```Python```版本要求```2.7+```，```3.3+```。可以使用命令```python -V```查看Python版本。当然建议系统越新越好，这样Python版本自然就高了。

### 1、安装pip或pip3
```pip``` 或 ```pip3``` 随便选择一个安装即可。

#### 安装pip：

```
# CentOS 6.x 32位
rpm -ivh http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
yum install -y python-pip

# CentOS 6.x 64位
rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
yum install -y python-pip

# CentOS 7.x
yum install -y epel-release
yum install -y python-pip
# 如果CentOS 7安装出现No package python-pip available，可以用以下命令进行安装
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py

#  Debian/Ubuntu系统
apt-get -y update
apt-get -y install python-pip
安装pip3：

# CentOS系统
wget https://www.moerats.com/usr/shell/Python3/CentOS_Python3.6.sh && sh CentOS_Python3.6.sh
# Debian系统
wget https://www.moerats.com/usr/shell/Python3/Debian_Python3.6.sh && sh Debian_Python3.6.sh
```

#### 查看是否安装成功
```pip3 -V```

### 2、安装bypy

#### pip安装
```pip install bypy```

### pip3安装
```pip3 install bypy```

#### 授权
执行 ```bypy info``` 命令，然后会给一个链接，用浏览器打开，将授权码复制过来即可。

![bypy-info](https://www.moerats.com/usr/picture/BYPY.png)

安装完成后可以看到，在你的百度网盘的【我的应用数据】下面已经多了一个【bypy】目录，你以后通过VPS所上传的文件都会在这个目录下面，你也只能下载这个目录里面的文件。

#### 操作命令
##### 1、显示网盘根目录(bypy)的文件列表：
```bypy list```
##### 2、比较当前目录和网站根目录文件：
```bypy compare```
##### 3、上传单个文件的命令如下：
```bypy upload 文件名```
##### 4、把当前目录上传到云盘：

```bypy syncup 目录地址```
or
```bypy upload 目录地址```
##### 5、下载单个文件的命令如下：
```bypy downfile 下载文件名```
##### 6、把云盘内容下载到本地来：

```bypy syncdown 目录地址```
or
```bypy downdir 目录地址```
更多命令和详细解释请运行```bypy```命令。
