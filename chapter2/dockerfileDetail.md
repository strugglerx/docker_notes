<!--
 * @Description: 
 * @Author: Moqi
 * @Date: 2019-07-24 06:46:20
 * @Email: str@li.cm
 * @Github: https://github.com/strugglerx
 * @LastEditors: Moqi
 * @LastEditTime: 2019-07-24 07:56:32
 -->
# dockerfile详细配置

## 所有配置项 

```
1 FROM
2 RUN
3 CMD
4 ENTRYPOINT
5 LABEL
6 EXPOSE
7 ENV
8 ADD
9 COPY
10 VOLUME
11 USER
12 WORKDIR
13 ARG
14 ONBUILD
15 STOPSIGNAL
16 SHELL
```
## `ENTRYPOINT`

ENTRYPOINT用于给容器配置一个可执行程序。也就是说，每次使用镜像创建容器时，通过ENTRYPOINT指定的程序都会被设置为默认程序。ENTRYPOINT有以下两种形式

```
ENTRYPOINT ["executable", "param1", "param2"]
ENTRYPOINT command param1 param2
```
ENTRYPOINT与CMD非常类似，不同的是通过docker run执行的命令不会覆盖ENTRYPOINT，而docker run命令中指定的任何参数，都会被当做参数再次传递给ENTRYPOINT。Dockerfile中只允许有一个ENTRYPOINT命令，多指定时会覆盖前面的设置，而只执行最后的ENTRYPOINT指令。

docker run运行容器时指定的参数都会被传递给ENTRYPOINT，且会覆盖CMD命令指定的参数。如，执行docker run <image> -d时，-d参数将被传递给入口点。

也可以通过docker run --entrypoint重写ENTRYPOINT入口点。

如：可以像下面这样指定一个容器执行程序：

```
ENTRYPOINT ["/usr/bin/nginx"]
```

示例

```
FROM ubuntu:16.04
MAINTAINER s
RUN apt-get update
RUN apt-get install -y nginx
RUN echo 'Hello World, 我是个容器' \ 
   > /var/www/html/index.html
ENTRYPOINT ["/usr/sbin/nginx"]
EXPOSE 80
```

构建完成后，启动一个容器：
```
docker run -i -t  itbilu/test -g "daemon off;"
```
在运行容器时，我们使用了-g "daemon off;"，这个参数将会被传递给ENTRYPOINT，最终在容器中执行的命令为/usr/sbin/nginx -g "daemon off;"。

## LABEL
LABEL用于为镜像添加内容，以key value指定：
```
LABEL <key>=<value> <key>=<value> <key>=<value> ...
```
使用LABEL指定元数据时，一条LABEL指定可以指定一或多条元数据，指定多条元数据时不同元数据之间通过空格分隔。推荐将所有的元数据通过一条LABEL指令指定，以免生成过多的中间镜像。

如，通过LABEL指定一些元数据
```
LABEL version="1.0" description="这是一个Web服务器" by="IT笔录"
```

指定后可以通过docker inspect查看：

```
docker inspect imageName
"Labels": {
    "version": "1.0",
    "description": "这是一个Web服务器",
    "by": "IT笔录"
},
```

注意；Dockerfile中还有个MAINTAINER命令，该命令用于指定镜像作者。但MAINTAINER并不推荐使用，更推荐使用LABEL来指定镜像作者。如：

```
LABEL maintainer="itbilu.com"
```

## ENV
ENV配置环境变量
```
ENV <key> <value>
ENV <key>=<value> ...
```
eg:设置一个环境变量
```
ENV CUSTOM_PATH /home/test/
WORKERDIR $CUSTOM_PATH
```
这些环境变量不仅可以构建镜像过程使用，使用该镜像创建的容器中也可以使用
```
$ docker run -it imageName 
root@196ca123c0c3:/# cd $CUSTOM_PATH
root@196ca123c0c3:/home/test# 
```













