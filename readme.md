# 华中科技大学硕士毕业论文latex模板 
此项目主要为记录如何从0开始，利用docker部署私人overleaf latex服务

以华中科技大学硕士毕业论文模板为例子

## 前言
早在本科时本人就被本地的latex环境折腾的够呛，~~但是用latex写出的论文与试验报告还是能获得更高的分数~~

之后发现了sharelatex、overleaf这个在线编辑latex的网站，从此不用再麻烦的配置本地环境了，但仍然有几个问题
* 在线版本的编译速度特慢（时间久了还要付费）
* 服务器在国外，国内访问速度巨慢

因此萌发了尝试使用docker部署本地overleaf的想法,这么做的好处有
* 本地服务器，访问速度快
* 可与小伙伴分享
* 编译速度快，无时间限制

于是开了这个仓库记录一下部署流程

## 文件说明
* HUST-master.zip
    > 华科的硕士毕业论文模板 
    [这位老哥提供的，默默感谢](https://github.com/zhaohyy/hust-master-latex)
    
* Songti SC-Regular.ttf
    > 华科的硕士毕业论文模板里面用到的字体，需要通过docker映射到容器内部使用

## 笔者服务器硬件参考
操作系统：manjaro
硬件配置：e3 1tssd 24gdd3

---------------
---------------
## 安装教程
接下来就是具体的安装流程

### docker安装
1. 各位参考[菜鸟教程](https://www.runoob.com/docker/docker-tutorial.html)进行安装

2. docker换源：编辑`/etc/docker/daemon.json`
```json
{
    "registry-mirrors":[
         "http://docker.mirrors.ustc.edu.cn",
         "http://hub-mirror.c.163.com",
         "http://registry.docker-cn.com"
    ] ,
    "insecure-registries":[
       "docker.mirrors.ustc.edu.cn",
         "registry.docker-cn.com"
    ]
}    
```
然后重启docker: `systemctl restart docker`

3. 开始下载overleaf镜像
```shell
docker pull kingsleyluoxin/sharelatex:full
```
这个镜像有点大，小水管可能要下很久


### 镜像启动
将本仓库的那个字体文件下载到你的服务器上,并修改下权限
```
chmod 755 ./Songti SC-Regular.ttf
```
接下来通过docker映射进容器并启动(字体文件路径请根据情况修改)
```
docker run -p 8085:80 -d -v /你的保存目录/Songti\ SC-Regular.ttf:/usr/share/fonts/chinese/Songti\ SC-Regular.ttf kingsleyluoxin/sharelatex:full
```
此时去访问服务器ip+8085端口就能看到效果了

比如服务器ip是192.168.3.105

那么就访问http://192.168.3.105:8085/launchpad 开始设置管理员账号密码

设置完成之后，将zip文件加载进去，再在menu处设置compiler 为 Xelatex 就能正常编译了！

