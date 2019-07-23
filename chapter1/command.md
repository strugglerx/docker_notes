<!--
 * @Description: 
 * @Author: Moqi
 * @Date: 2019-07-24 06:42:24
 * @Email: str@li.cm
 * @Github: https://github.com/strugglerx
 * @LastEditors: Moqi
 * @LastEditTime: 2019-07-24 06:57:28
 -->
# 常用基础命令


## 查看版本
``` vb
docker version
docker info
```

## 查找镜象
``` vb
docker search [imageName]
```
## 镜象操作
### 列出所有image
```vb
docker image ls
```
### 删除image
```vb
docker image rm [imageName or containerID]
## or
docker rmi [imageName or containerID]
```
## 获取镜像
```vb
docker image pull repertory/imageName
```
## 运行镜象
```vb
docker container run imageName
docker contianer run -it imageName bash #交互命令
eg：
docker container run --rm -p 8000:3000 -it imageName /bin/bash
加入--rm参数，在容器终止后自动删除容器文件
```
## 终止容器
```vb
docker container kill [containID]
```
## 删除终止的容器
```vb
docker container rm [containID]
```
## 列出正在运行的容器
```vb
docker container ls
docker ps 
```
## 列出所有容器（包括终止的）
```vb
docker container ls --all
docker ps -a
```
## 针对已创建好的容器操作
```vb
docker container start [containerID]
docker container stop [containerID]
docker container logs [containerID]
进入容器进行操作
docker container exec -it [containerID] /bin/bash
从容器复制到本机
docker container cp [containerID]:[path/file] localPath
```
## 创建image文件(当前目录存在Dockerfile)
```vb
docker image build -t imageName path
docker image build -t imageName:tag path

-t 表示指定image文件的名字 tag表示指定标签，一般为版本号，如果不指定就是latest
```
## 发布image标注用户名和版本
```vb
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




