---
title: 回顾IM服务器配置
date: 2018-07-10 10:25:24
tags:
---
大体分为4个部分

#### 一.配置collider信令服务器，基于golang：
1.请先安装golang,以Ubuntu为例

``` 
$ sudo apt-get update
$ sudo apt-get install golang
```
2.创建文件夹，并将该文件夹路径设置为$GOPATH
``` 
$ cd
$ mkdir go
$ echo "export GOPATH=~/go" >> ~/.bashrc
$ echo "export PATH=$PATH:~/go/bin" >> ~/.bashrc
$ source ~/.bashrc
$ mkdir -p ~/go/bin ~/go/src ~/go/pkg
```
<!--more-->
3.配置和部署
``` 
$ cd ~/go/src
$ git clone https://github.com/changvvb/collidersrc.git
$ mv collidersrc/collider* ./
$ go get github.com/go-sql-driver/mysql
$ go get golang.org/x/net/websocket
$ go install collidermain
$ collidermain -room-server=http://IP:6060
```
#### 二.配置基于google appengine的python服务
这个是IM视频对讲的python服务

1.配置google_appengine,然后配置环境变量(以下命令仅供参考)
到https://cloud.google.com/sdk/docs/ 下载对应的sdk，解压，然后将解压后的文件夹中的bin目录路径配置到PATH环境变量中
``` 
$ cd
$ wget https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-sdk-131.0.0-linux-x86_64.tar.gz
$ tar zxvf google-cloud-sdk-131.0.0-linux-x86_64.tar.gz
$ echo "export PATH=$PATH:~/google-cloud-sdk/bin" >> ~/.bashrc
$ source ~/.bashrc
```
2.下载与部署
``` 
$ git clone https://github.com/changvvb/apprtc_python.git
$ cd apprtc_python
```
3.配置IP地址
打开apprtc_startup.sh，修改IP地址 
打开source/constant.py修改SERVER_IP变量

4.启动服务
``` 
$ ./apprtc_startup.sh
```
####三.配置一个api服务，基于golang
turn_ice 一个api服务
如果你已经把collider服务装好，golang应该已经配置好

下载与部署
``` 
$ go get github.com/changvvb/turn_ice
$ go install github.com/changvvb/turn_ice
$ turn_ice -server=IP
```
#### 四、配置coturn
1.安装coturn
参考 https://github.com/coturn/coturn
``` 
$ git clone https://github.com/coturn/coturn.git
$ cd coturn
$ ./configure
$ make
$ sudo make install
```
2.下载与部署
``` 
$ git clone https://github.com/changvvb/coturnconfig.git
$ cd coturnconfig
```
3.配置IP地址
打开 turnserver_start 更改IP地址

4.部署相关配置配置文件
``` 
$ sudo ./deploy
```
5.启动服务
``` 
$ ./turnserver_start
```
