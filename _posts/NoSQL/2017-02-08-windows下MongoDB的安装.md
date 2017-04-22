---
layout: post
title: windows下MongoDB的安装
category: NoSQL
tags: NoSQL
description: windows下MongoDB的安装
---
### 下载MongoDB安装文件
进入MonogoDB的官网，进入下载页，选择合适自己系统版本的安装程序进行安装。

### 安装MongoDB
双击下载的MongoDB的.msi文件，这里采用默认的进行安装即可，也可点击“Custom”（自定义）安装选项来指定安装目录。

##### 这时候的MongoDB应该安装在C:\Program Files\MongoDB路径下（这里指的是64位操作系统）

### 运行MongoDB
##### 搭建MongoDB的运行环境
MongoDB需要一个数据目录来存储所有数据。MongoDB的默认数据目录路径是：C:\data\db。

在命令行中运行以下命令创建该文件夹：  

    md C:\data\db

也可以使用mongod.exe启动的时候，通过--dbpath选项来用指定数据文件的路径，例如： 
 
    C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe --dbpath d:\test\mongodb\data

如果您的路径包含空格，请将路径用双引号引用，例如：

    C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe --dbpath "d:\test\mongo db data"

也可以将数据路径（dbpath）配置到一个配置文件中。

### 启动MongoDB
运行mongod.exe来启动MongoDB，例如，在命令行提示下执行：

    C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe  

该命令会启动MongoDB数据库的主进程。命令执行后，在控制台输出连接相关的消息，表明mongod.exe进程运行成功。

或者直接进入C:\Program Files\MongoDB\Server\3.4\bin目录下双击mongod.exe

###连接MongoDB
运行mongo.exe就能连接上MongoDB，可以通过打开另外一个命令行窗口，执行如下命令：

    C:\Program Files\MongoDB\Server\3.4\bin\mongo.exe

或者直接进入C:\Program Files\MongoDB\Server\3.4\bin目录下双击mongo.exe

### 判断是否已连接MongoDB
打开浏览器，访问[http://localhost:27017/](http://localhost:27017/ "http://localhost:27017/")，如有

    It looks like you are trying to access MongoDB over HTTP on the native driver port.
显示，则MongoDB启动正常。


### 配置MongoDB服务
1.管理员模式打开命令行窗口
  
2.创建目录，执行下面的语句来创建数据库和日志文件的目录

    md c:\data\db
    md c:\data\log

3.创建配置文件
创建一个配置文件。该文件必须设置systemLog.path参数，包括一些附加的配置选项更好。

例如，创建一个配置文件位于C:\Program Files\MongoDB\mongod.cfg，其中指定systemLog.path和storage.dbPath。具体配置内容如下：

    systemLog:
        destination: file
        path: c:\data\log\mongod.log
    storage:
        dbPath: c:\data\db

4.安装MongoDB服务  
通过执行mongod.exe，使用--install选项来安装服务，使用--config选项来指定之前创建的配置文件。

    "C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe" --config "C:\Program Files\MongoDB\mongod.cfg" --install  

要使用备用dbpath，可以在配置文件（例如：C:\Program Files\MongoDB\mongod.cfg）或命令行中通过--dbpath选项指定。

如果需要，您可以安装mongod.exe或mongos.exe的多个实例的服务。只需要通过使用--serviceName和--serviceDisplayName指定不同的实例名。只有当存在足够的系统资源和系统的设计需要这么做。

5.启动MongoDB服务  

    net start MongoDB

6.关闭MongoDB服务

    net stop MongoDB

7.移除MongoDB服务

    "C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe" --remove

官方手册：[http://docs.mongoing.com/manual-zh/tutorial/install-mongodb-on-windows.html](http://docs.mongoing.com/manual-zh/tutorial/install-mongodb-on-windows.html "http://docs.mongoing.com/manual-zh/tutorial/install-mongodb-on-windows.html")
