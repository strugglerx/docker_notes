<!--
 * @Description: 
 * @Author: Moqi
 * @Date: 2019-07-24 06:46:20
 * @Email: str@li.cm
 * @Github: https://github.com/strugglerx
 * @LastEditors: Moqi
 * @LastEditTime: 2019-07-24 07:33:49
 -->
# 文件

#### .dockerignore 

>类似.gitignore 排除不要的路径

```shell
.git
node_modules
npm-debug.log
```

#### Dockerfile

>docker配置

```
FROM node:8.4
COPY . /app
WORKDIR /app
RUN npm install --registry=https://registry.npm.taobao.org
EXPOSE 3000
```

- FROM imageName:tag or imageName:digest
#imageName为镜像 tag为版本
- COPY OriginPath SourcePath
#将原始目录下的文件拷贝到目标文件夹
- WORKDIR PATH
#工作目录
- RUN COMMAND
#制作镜像时初始执行的命令，例如：
#npm install --registry=https://registry.npm.taobao.org 
#["npm","indtall"]
#当使用多条命令时可使用\
```
RUN /bin/bash -c 'source $HOME/.bashrc; \
echo $HOME'
```
- EXPOSE port
#向外暴露的端口
- CMD COMMAND
#cmd会在每次容器启动后执行，一个Dockerfile可以包含多个run命令，但是最多只能有一个cmd命令，如果Dockerfile添加了CMD命令，就不能附加指令，例如启动执行的 /bin/bash 如果附加指令，cmd命令就失效了。




