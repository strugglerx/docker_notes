# 记录`docker`学习
> author： struggler \
> email： str@li.cm \
> start Date: 2019/07/23

记录一些笔记方便以后复习查看
***

### 常用基础命令

``` vb
# 查看版本
docker version
docker info

# 查找镜象
docker search [imageName]

# 镜象操作
## 列出所有image
docker image ls
## 删除image
docker image rm [imageName or containerID]
## or
docker rmi [imageName or containerID]

## 获取镜像
docker image pull repertory/imageName

## 运行镜象
docker container run imageName
docker contianer run -it imageName bash #交互命令
eg：
docker container run --rm -p 8000:3000 -it imageName /bin/bash
加入--rm参数，在容器终止后自动删除容器文件

## 终止容器
docker container kill [containID]

## 删除终止的容器
docker container rm [containID]

## 列出正在运行的容器
docker container ls
docker ps 

## 列出所有容器（包括终止的）
docker container ls --all
docker ps -a

## 针对已创建好的容器操作
docker container start [containerID]
docker container stop [containerID]
docker container logs [containerID]
进入容器进行操作
docker container exec -it [containerID] /bin/bash
从容器复制到本机
docker container cp [containerID]:[path/file] localPath

## 创建image文件(当前目录存在Dockerfile)
docker image build -t imageName path
docker image build -t imageName:tag path

-t 表示指定image文件的名字 tag表示指定标签，一般为版本号，如果不指定就是latest

## 发布image标注用户名和版本
1.在对应的网站注册账号hub.docker.com or cloud.docker.com 
2.命令行登陆
docker login
3.本地image标注用户名和版本
docker image tag [imageName] [username]/[repository]:[tag]
3.1 实例
docker image tag koa-demos:0.0.1 ruanyf/koa-demos:0.0.1
3.2 不标注用户名，重新构建image文件
docker image build -t [username]/[repository]:[tag] path
4 发布image文件
docker image push [username]/[repository]:[tag]

```
### 制作docker容器文件

#### .dockerignore 

>类似.gitignore 排除不要的路径

```shell
.git
node_modules
npm-debug.log
```

#### Dockerfile

>docker配置

```shell
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
```

- FROM imageName:tag
#imageName为镜像 tag为版本
- COPY OriginPath SourcePath
#将原始目录下的文件拷贝到目标文件夹
- WORKDIR PATH
#工作目录
- RUN COMMAND
#制作镜像时初始执行的命令，例如：
#npm install --registry=https://registry.npm.taobao.org 
#["npm","indtall"]
- EXPOSE port
#向外暴露的端口
- CMD COMMAND
#cmd会在每次容器启动后执行，一个Dockerfile可以包含多个run命令，但是最多只能有一个cmd命令，如果Dockerfile添加了CMD命令，就不能附加指令，例如启动执行的 /bin/bash 如果附加指令，cmd命令就失效了。




