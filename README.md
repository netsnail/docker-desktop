# Hack Day 参赛作品 docker desktop 基于浏览器的图形操作系统

##1、团队介绍

团队    ： 北京光音网络（我很冷）

负责人： 赵文来

成员    ： 赵文来

##2、作品介绍

为容器提供基于WEB的GUI操作界面，UI使用windows10风格，用户可以像操作本地电脑一样操作容器。

用户可以上传文件到容器中，打开编辑预览文件，当网盘来使用。

启动特定的语言容器，进行可视化程序开发。

启动已经打好包Image，在可视化界面里直观的测试。

只要有浏览器用户就可以拥有一台高配置的桌面操作系统，并在里面工作。

因为是使用windows的界面风格，就算用户对linux不熟悉也可以很容易的使用

##3、实现方式

1、首先我使用 typescript语言 以及 angualr2框架 编写了一套UI组件 web desktop ，这套组件尽可能的模仿了windows10系统，完成了基本的操作。

2、然后使用swarm搭建跨区的集群

3、后端语言为nodejs，socket与前端建立tty 并与UI组件进行整合

##4、如何使用

使用起来非常简单 首先将docker daemon 启动tcp方式，然后运行我已经制作好的镜像，将docker地址通过环境变量传给容器
```
docker run -d -p 8088:8088 --env DOCKER_HOST=192.168.100.100:8080 junjun16818/docker-desktop:0.1
```
项目地址

https://github.com/junjun16818/docker-desktop

在线演示地址

http://101.251.243.38

具体操作方法在实现功能里有描述

##5、实现功能

在首页中有创建机器的按钮，点击会弹出表单

     名称（name）： 容器的名称，会通过 docker run参数 - - name 传入
     标签（label）：我创建的swarm集群可识别 japan 和 china标签，将容器启动到中国节点或者日本节点，通过docker run参数 - -label 传入
     镜像（image）：会根据这个镜像启动容器，默认会使用 junjun16818/node:5.4.0-wheezy (这个镜像被服务器缓存过，可以快速创建)
     
创建成功后会列出所有容器。点击容器可以进入

进入后是一个windows10风格的界面，在里面有 我的电脑、我的文档、terminal、浏览器。

双击我的电脑会列出容器根目录下的所有文件夹以及文件

双击我的文档会列出容器root目录下的所有文件夹以及文件

用户可以拖拽本地文件到窗口实现上传

支持的文件类型：

     inode/directory （文件夹类型 双击进入文件夹内）
     text/plain、inode/x-empty（文本类型，可打开编辑保存，保存的快捷键是 control ＋ s）
     image/png、image/jpeg （图片类型 双击预览图片）
     application/zip （双击自动解压缩到当前目录，需要容器安装unzip工具）
     application/pdf （pdf文件 双击预览）

我的电脑和我的文档都可以点击右键来创建文件以及文件夹

文件类型可以右键删除、重命名以及复制粘贴

双击terminal会弹出终端管理窗口，在这里可以通过命令来操作当前容器

双击IE浏览器会弹出浏览器窗口，默认会指向当前当前容器的80端口，用户可以在容器内运行80端口的web程序在这里测试（在创建容器时默认暴露了80端口）

模仿windows风格将窗口拖拽到顶端会最大化，拖拽到左右两端实现分屏，再次拖拽窗口可以变回原来的大小

所有窗口可以最小化、最大化、以及关闭，最小化后的窗口可以在任务栏中找回。

鼠标移到任务栏会浮出小窗体（windows风格）

##6、重点难点

特点是浏览器只是提供图形化界面来帮助用户执行通用的linux接口。用户在页面上的大部分操作，消耗的都是容器资源。

解决的难点是当容器在国外时延迟的问题，解决的方法是当用户在访问页面时通过socket连接一个tty终端，用户在页面上的所有操作都模拟用户在终端输入命令并返回结果，所以就算容器是在国外，操作起来也和国内的容器一样流畅。


##7、项目截图


![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/1.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/2.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/3.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/4.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/5.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/6.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/7.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/8.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/9.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/10.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/11.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/12.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/13.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/14.jpeg "github")  
![github](https://github.com/junjun16818/docker-desktop/blob/master/resource/demo/15.jpeg "github")  

