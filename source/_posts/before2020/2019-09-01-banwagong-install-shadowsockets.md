---
title: 'Banwagong 自己配置shadowsockets'
date: 2019-09-01 17:51:05
tags: centos;banwagong;shadowsockets
---

这个是转载的文章，但是我在配置的过程中遇到了几个问题，都在此记录一下，防止之后自己又记不住了。(以前总以为自己能记住几个特殊的步骤，回头发现之后俄都是自欺欺人啊)

[原文地址](https://baijifeilong.github.io/2018/12/10/bandwagon/)
原文内容也贴过来，备用啊。

## 原文对我有用的
三、服务器机房选择
洛杉矶机房二选一（DC2 QNET、DC4 MCOM）。DC4为搬瓦工新加机房。
实际体验上半斤八两。
加拿大、荷兰机房最好不要选择。
根据概率论，美国与赵国的网络连接基数大，IP被封杀的机率低

四、服务器购买
支付宝购买即可。美刀自动换算为人民币

有6.25趴优惠码(BWH26FXH3HIQ)可用。优惠码来自网络，有效期不详。可省1.25美刀，最终需支付18.74美刀

五、服务器系统选择
默认系统是 Centos 6 x86 bbr。

BBR是谷歌出品的拥塞控制算法，据说优化网速有奇效

建议换为Centos 7 x86_64 bbr。Centos 6 太老，官方包的python只支持到2.6。没有Systemd服务管理工具

六、Shadowsocks服务安装
1. 安装Shadowsocks
yum install -y epel-release 安装Centos社区仓库，pip与sodium在里头
yum install -y python2-pip libsodium git pip和git用来安装Shadowsocks libsodium用于支持chacha20加密算法
pip install git+https://github.com/shadowsocks/shadowsocks.git@master 从Shadowsocks官方Git仓库的主分支下载Shadowsocks源码并安装
2. 添加Shadowsocks为Systemd服务
创建并填充 /usr/lib/systemd/system/myss.service
```
    [Unit]
    Description=My shadowsocks server

    [Service]
    ExecStart=/usr/bin/ssserver -k password -m chacha20 -p 33333
```
3. 启动Shadowsocks服务
systemctl start myss 启动服务
systemctl status myss 查看服务运行状态
stop 停止服务 restart 重启服务
七、Shadowsocks的使用
1. 运行本地代理服务
sslocal -s <SERVER-HOST> -k password -m chacha20 -p 33333 -l 44444

Shadowsocks会开启一个SOCKS5本地代理。

端口最好更改一下，减小被封杀机率

加密方法建议选择chacha20，CPU负载低，给搬瓦工公司节省几分钱电费

2. 测试代理是否工作
curl --socks5-host localhost:44444 www.google.


## 补充1

使用mac的termanil可以直接进行ssh的链接。
命令：
``` bash
    ssh root@**.**.**.** -p 26556
```
接下来输入密码即可
这个比网页上自带的bash页面好很多，也能直接编辑文件什么的

## 补充2
执行 安装Shadowsocks 中的安装源和pip的时候，会提示已经安装，但是安装pip的时候依然提示找不到包，原因是默认禁用了epel，所以需要我们开启这个源。
搜到大多数的方法都是编辑文件，当时不会用term 进行ssh链接，所以找到了这个命令行修改的方式
``` bash
    yum-config-manager --enable remi
```
## 补充3

创建的命令 
    usr/lib/systemd/system/myss.service

vim 内的保存推出是 wq。

## 补充4
第一次设置完成后不好用，不知道端口不对还是其他参数有问题，所以使用了之前的配置可以正常使用了。下回配置的时候注意