# docker



登录阿里云, 镜像源https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors 左边的**镜像加速器**. 



卡到setting都打不开,绝了.重启, 关闭, 多试几次好像就变流畅了

你会发现和网上很多图不一样,因为

"网络"选项卡等各种选项在 Windows 容器模式下不可用，因为网络CPU 由 Windows 管理。不同的设置可用于配置，具体取决于您是使用 WSL 2 模式下的 Linux 容器、Hyper-V 模式下的 Linux 容器还是 Windows 容器。

## 命令

### build  

-t  

### run

```bash
-name 
-d detach模式,在后台运行
-p 80:80  把host 80 端口映射到contain 80 端口
-i  interactive 交互式
-t 终端
--rm 在容器终止运行后自动删除容器文件。
```

```bash
container
$ docker container run hello-world
$ docker container run -it ubuntu bash #进入ubuntu的bash如果要退出就：Ctrl-D 或者exit
docker container run -p 8000:3000 -it koa-demo:0.0.1 /bin/bash #容器的 3000 端口映射到本机的 8000 端口。 这里可能要开一下防火墙, Node 进程运行在 Docker 容器的虚拟环境里面，进程接触到的文件系统和网络接口都是虚拟的，与本机的文件系统和网络接口是隔离的，因此需要定义容器与物理机的端口映射（map）。
```

### container文件

```bash
docker container run #命令会从 image 文件，生成一个正在运行的容器实例container。
docker ps -a 看所有的container，包括运行中的，以及未运行的或者说是沉睡镜像
docker attach goofy_almeida   #container运行在后台，如果想进入它的终端,用attach有一个缺点，那就是每次从container中退出到前台时，container也跟着退出了。
docker exec -it  #退出container时，让container仍然在后台运行
docker container rm goofy_almeida#删除运行的容器文件，释放硬盘空间
docker container kill [containID]
```

注意，`docker container run`命令具有自动抓取 image 文件的功能。如果发现本地没有指定的 image 文件，就会从仓库自动抓取。因此，前面的`docker image pull`命令并不是必需的步骤。



### image文件

```bash
# 列出本机的所有 image 文件。
$ docker image ls

# 删除 image 文件
$ docker image rm [imageName]

$ docker image pull library/hello-world
```

**Docker 把应用程序及其依赖，打包在 image 文件里面。**只有通过这个文件，才能生成 Docker 容器。image 文件可以看作是容器的模板。Docker 根据 image 文件生成容器的实例。同一个 image 文件，可以生成多个同时运行的容器实例。

image 是二进制文件。实际开发中，一个 image 文件往往通过继承另一个 image 文件，加上一些个性化设置而生成。举例来说，你可以在 Ubuntu 的 image 基础上，往里面加入 Apache 服务器，形成你的 image。

image 文件是通用的，一台机器的 image 文件拷贝到另一台机器，照样可以使用。一般来说，为了节省时间，我们应该尽量使用别人制作好的 image 文件，而不是自己制作。即使要定制，也应该基于别人的 image 文件进行加工，而不是从零开始制作。

为了方便共享，image 文件制作完成后，可以上传到网上的仓库。此外，出售自己制作的 image 文件也是可以的。

下载到哪里呢?

windows会下载到 C:\users\Public\Documents公用文档中.

#### 自己制作 image 文件

1. 源码

2. 写dockerfile 文件 

   ```dockerfile
   FROM node:8.4
   COPY . /app #不用taobao源我用了180秒,用了后用了10秒
   WORKDIR /app
   RUN npm install --registry=https://registry.npm.taobao.org #不用淘宝源就会npm链接不上 ,用了淘宝源后用了30秒
   EXPOSE 3000
   ```

3.  dockerignore文件 路径要排除，不要打包进入 image 文件.

```bash
docker image build -t koa-demo:0.0.1 .
```

上面代码中，`-t`参数用来指定 image 文件的名字，后面还可以用冒号指定标签。如果不指定，默认的标签就是`latest`。最后的那个点表示 Dockerfile 文件所在的路径，上例是当前路径，所以是一个点。