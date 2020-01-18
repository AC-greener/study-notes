#### docker

##### dockerfile

| 命令    | 含义                           | 例子                     |
| ------- | ------------------------------ | ------------------------ |
| FROM    | 继承的镜像                     | FROM node                |
| COPY    | 拷贝                           | COPY ./app /app          |
| WORKDIR | 指定工作目录                   | WORKDIR  /app            |
| RUN     | 编译打包阶段运行的命令         | RUN npm install          |
| EXPOSE  | 暴露端口，允许外部连接这个端口 | EXPOSE 3000              |
| CMD     | 容器运行阶段运行的命令         | CMD ["node", "index.js"] |
|         |                                |                          |

##### .dockerignore 文件

​	表示要排除，不要打包到image中的文件路径

##### 创建镜像

```dockerfile
docker build -t express-demo ./
```

##### 从一个镜像运行容器

-it将容器的shell映射为当前的shell，在本机中执行的命令都会发送到容器中执行

/bin/bash 容器启动后执行的第一个命令

-rm在容器终止运行后自动删除容器文件

```
docker run -p 8080:3000 -it express-demo /bin/bash
```

##### 发布镜像

```
docker login
docker image tag [imageName] [username]/[repository]:[tag]
docker image build -t [username]/[repository]:[tag] .
docker push [username]/[repository]:[tag]

```

##### 创建数据盘

删除docker容器后，吧数据留在服务器上面，

```
docker run -v /mnt -it express-demo bash
```

##### compose

![1571736884383](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1571736884383.png)

![1571736465921](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1571736465921.png)![1571736651863](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1571736651863.png)