隔离是很多计算模式，资源管理策略的一个核心概念

#### 1，docker简介

docker是什么：

docker是一个工具，它包括一个命令行程序，一个后台守护进程，以及一组远程服务，解决了常见的软件问题，简化了软件的安装，运行 发布和删除

不用再专注于软件的安装等复杂的细节

虚拟化：

在没有docker的时代，通常使用虚拟机来提供隔离，虚拟机提供虚拟的硬件，可安装一个操作系统和其他程序，docker是不虚拟化技术，运行在docker容器中的程序接口直接和主机中的Linux内核直接打交道

![image-20200801141055051](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801141055051.png)

docker构建的容器隔离包括8个方面：

1. 进程标识符和能力
2. 主机名和域名
3. 文件系统访问和结构
4. 通过共享内存的进程间通信
5. 网络访问和结构
6. 用户名和标识
7. 控制文件系统根目录的位置
8. 资源保护

docker镜像：

是一个容器中运行程序的所有文件的快照

![image-20200801150813361](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801150813361.png)

启动容器：

```dockerfile
docker run helloworld
```

docker命令行帮助：

```dockerfile
docker help
docker help cp
```

容器的四种状态：

1. 运行
2. 已暂停
3. 重新启动
4. 已退出

#### 2，Docerk基本命令

安装nginx容器:

```dockerfile
docker run --detach --name web nginx:latest
```

守护式容器非常适合那些在后台静静运行的程序，一般使用--detach来运行



检查哪些容器正在运行：

```
docker -ps
```

```dockerfile
docker ps -a  //查看所有的容器包括在退出状态的
```

该命令会列出以下信息：

1. 容器id
2. 使用镜像
3. 容器中执行的命令
4. 容器运行的时长
5. 容器暴露的网络端口
6. 容器名



xxx表示容器的名字

重启容器：

```dockerfile
docker restart  xxx
```

查看容器的日志：

```dockerfile
docker logs xxx
```

停止容器：

```dockerfile
docker stop xxx

```

可以在正在运行的容器中运行一个额外的进程：

```shell
docker exec xxx echo just a test
```

查看容器中那些进程正在运行着：

```shell
docker top xxx
```

容器重启策略，容器出现故障时自动重启：

```shell
docker run -d --name xxx --restart always busybox date
```

将挂载的容器文件系统设置为只读，防止容器被修改

```
docker run -d --name xxx --read-only nginx
```

删除容器：

```shell
docker rm xxx
docker run --rm //运行容器，并在容器退出时将其删除
```

PID命名空间：

每个运行的程序，在linux上都有一个唯一的编号，叫做进程标识符PID，在linux中可以创建多个PID明明空间，每个命名空间拥有一套完整的PID。每个docker容器也会创建一个PID命名空间

![image-20200801155953560](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801155953560.png)

通过检查容器元数据判断容器是否运行：

docker inspect --format "{{.State.Running}}" xxx

#### 3，使用Docker安装软件

docker通过镜像来创建容器，镜像包含了创建容器所需的文件和镜像元数据（暴漏的端口，卷的定义等等）



docker仓库：

![image-20200801165455047](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801165455047.png)

 标签：标签可以指定唯一的镜像

在docker hub查找docker镜像

```
docker search postgress
```

吧镜像导出成文件：

![image-20200801170651295](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801170651295.png)

使用dockerfile安装软件：

git clone  htpps://github.com/ch3_dockerfile.git

docker build -t 

**镜像的分层：**

镜像实际上是镜像层的集合。镜像和其他镜像有着父子依赖关系，这些关系构成分层

<img src="C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801172250497.png" alt="image-20200801172250497" style="zoom: 80%;" />

分层文件系统的优点：

1. 公共层仅需要安装一次，如果箱安装任何数目的镜像，他们都依赖于公共层，公共层只需要下载一次
2. 分层提供了依赖管理和隔离的工具

删除指定的镜像：

```shell
docker rm imageName
```

#### 4，持久化存储和卷间状态共享

在容器中运行一个数据库，当启动容器时，他将会初始化一个空数据库，当其他程序介入该数据库并存入数据，这些数据保存在哪里呢？是以容器中一个文件的形式吗？

一组不同的web程序运行在不同的容器中，为了让它们的日志文件保存在容器之外，应该把日志写到哪儿去呢？

##### 存储卷简介

<img src="C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801175255529.png" alt="image-20200801175255529" style="zoom:80%;" />

存储卷是一个数据分割和共享的工具，有一个与容器无关的生命周期；

存储卷可分离关注点，并为架构组件创建模块化

存储卷可以隔离应用程序和主机的关系

<img src="C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200801180121944.png" alt="image-20200801180121944"  />

##### 存储卷的类型

1，绑定挂载卷，可挂载在主机文件系统任意位置

2，管理存储卷，docker守护程序会在主机文件系统中创建存储卷，并由docker管理

![image-20200802110406787](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200802110406787.png)

##### 容器间共享存储卷

##### 管理券的生命周期

存储卷的生命周期独立于任何容器，但是一个用户只能通过容器句柄引用docker管理卷

##### 常见的存储卷模式

1. 卷容器模式。是一个容器，只是提供卷的句柄

2. 数据打包的存储卷容器。经常用于给其他容器分发静态内容

   ![image-20200802121541941](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200802121541941.png)

3. 多态容器模式，是一种组成最小功能组件并最大化重用的方法

#### 5，网络访问



![image-20200802125858071](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200802125858071.png)

docker创建的时一种虚拟网络（也成为网桥），可以让正在运行的容器能够连接到网络当中

![image-20200802130501250](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200802130501250.png)

四种网络容器原型：

1. closed容器
2. joined容器
3. bridged容器
4. open容器

![image-20200802130827747](C:\Users\zhutongtong\AppData\Roaming\Typora\typora-user-images\image-20200802130827747.png)

##### Closed容器

运行在这种容器中的进程只能够访问本地回环接口

##### Bridged容器

该容器开放了网络的隔离程度，是最常见的网络容器原型

创建bridged容器：

```
docker run --rm --net bridge alpine:latest ip addr
```

##### Joined容器

Joined容器共享一个网络栈，容器之间没有任何的隔离。

使用场景：

1. 想要不同容器上的程序通过本地回环接口进行通信时

##### Open容器

open容器，没有网络容器，并且对主机网络有完全的访问权，open容器没有提供任何隔离

