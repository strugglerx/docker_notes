<!--
 * @Description: 
 * @Author: Moqi
 * @Date: 2019-07-24 06:46:20
 * @Email: str@li.cm
 * @Github: https://github.com/strugglerx
 * @LastEditors: Moqi
 * @LastEditTime: 2019-07-24 08:35:03
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
    "description": "",
    "by": ""
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

## ADD
ADD构建环境中的文件或者目录到镜像中
```
ADD <src>... <dest>
ADD ["<src>",... "<dest>"]
```
通过ADD复制文件时，需要通过<src>指定源文件位置，并通过<dest>来指定目标位置。<src>可以是一个构建上下文中的文件或目录，也可以是一个URL，但不能访问构建上下文之外的文件或目录。

如，通过ADD复制一个网络文件：
```
ADD http://wordpress.org/latest.zip $CUSTOM_PATH
```
另外，如果使用的是本地归档文件（gzip、bzip2、xz）时，Docker会自动进行解包操作，类似使用tar -x。

## COPY
COPY构建环境中的文件或者目录到镜像中
```
COPY <src>... <dest>
COPY ["<src>",... "<dest>"]
```
COPY指令非常类似于ADD，不同点在于COPY只会复制构建目录下的文件，不能使用URL也不会进行解压操作。

## VOLUME

VOLUME用于创建挂载点，即向基于所构建镜像创始的容器添加卷：
```
VOLUME ["/data"]
```
一个卷可以存在于一个或多个容器的指定目录，该目录可以绕过联合文件系统，并具有以下功能：

- 卷可以容器间共享和重用
- 容器并不一定要和其它容器共享卷
- 修改卷后会立即生效
- 对卷的修改不会对镜像产生影响
- 卷会一直存在，直到没有任何容器在使用它
VOLUME让我们可以将源代码、数据或其它内容添加到镜像中，而又不并提交到镜像中，并使我们可以多个容器间共享这些内容。

如，通过VOLUME创建一个挂载点：
```
ENV CUSTOM_PATH /home/custom/
VOLUME [$CUSTOM_PATH]
```
构建的镜像，并指定镜像名为itbilu/test。构建镜像后，使用新构建的运行一个容器。运行容器时，需-v参将能本地目录绑定到容器的卷（挂载点）上，以使容器可以访问宿主机的数据。

```shell
$ sudo docker run -i -t -v ~/code/test:/home/custom/  imageName
root@31b0fac536c4:/# cd /home/test/
root@31b0fac536c4:/home/test# ls
README.md  app.js  bin  config.js  controller  db  demo  document  lib  minify.js  node_modules  package.json  public  routes  test  views
```
如上所示，我们已经可以容器的/home/itbilu/目录下访问到宿主机~/code/test目录下的数据了。

## USER
USER用于指定运行镜像所使用的用户：
```
USER daemon
```
使用USER指定用户时，可以使用用户名、UID或GID，或是两者的组合。以下都是合法的指定试：
```
USER user
USER user:group
USER uid
USER uid:gid
USER user:gid
USER uid:group
```
使用USER指定用户后，Dockerfile中其后的命令RUN、CMD、ENTRYPOINT都将使用该用户。镜像构建完成后，通过docker run运行容器时，可以通过-u参数来覆盖所指定的用户。

## WORKDIR
WORKDIR用于在容器内设置一个工作目录：
```
WORKDIR /path/to/workdir
```
通过WORKDIR设置工作目录后，Dockerfile中其后的命令RUN、CMD、ENTRYPOINT、ADD、COPY等命令都会在该目录下执行。

如，使用WORKDIR设置工作目录：
```
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```
在以上示例中，pwd最终将会在/a/b/c目录中执行。

在使用docker run运行容器时，可以通过-w参数覆盖构建时所设置的工作目录。



## ARG
ARG用于指定传递给构建运行时的变量：
```
ARG <name>[=<default value>]
```
如，通过ARG指定两个变量：
```
ARG site
ARG build_user=user
```
以上我们指定了site和build_user两个变量，其中build_user指定了默认值。在使用docker build构建镜像时，可以通过--build-arg <varname>=<value>参数来指定或重设置这些变量的值。

```
$ sudo docker build --build-arg site=custom.com -t custom/test .
```
这样我们构建了custom/test镜像，其中site会被设置为custom.com，由于没有指定build_user，其值将是默认值user。



## ONBUILD
ONBUILD用于设置镜像触发器：
```
ONBUILD [INSTRUCTION]
```
当所构建的镜像被用做其它镜像的基础镜像，该镜像中的触发器将会被钥触发。

如，当镜像被使用时，可能需要做一些处理：
```
[...]
ONBUILD ADD . /app/src
ONBUILD RUN /usr/local/bin/python-build --dir /app/src
[...]
```


## STOPSIGNAL
STOPSIGNAL用于设置停止容器所要发送的系统调用信号：
```
STOPSIGNAL signal
```
所使用的信号必须是内核系统调用表中的合法的值，如：9、SIGKILL。



## SHELL
SHELL用于设置执行命令（shell式）所使用的的默认shell类型：
```
SHELL ["executable", "parameters"]
```
SHELL在Windows环境下比较有用，Windows下通常会有cmd和powershell两种shell，可能还会有sh。这时就可以通过SHELL来指定所使用的shell类型：
```
FROM microsoft/windowsservercore

# Executed as cmd /S /C echo default
RUN echo default

# Executed as cmd /S /C powershell -command Write-Host default
RUN powershell -command Write-Host default

# Executed as powershell -command Write-Host hello
SHELL ["powershell", "-command"]
RUN Write-Host hello

# Executed as cmd /S /C echo hello
SHELL ["cmd", "/S"", "/C"]
RUN echo hello
```

















