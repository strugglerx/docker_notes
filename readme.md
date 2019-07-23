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

```