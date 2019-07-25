## Image 相关的操作

- Image的生成途径
    - Dockerfile
    - 基于容器制作镜像
        - Create a new image from a container‘s changes
        - Usage
            - [docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]](#COMMIT-Usage)
    - Docker Hub automated builds

####  COMMIT Usage

|Name,shorthand|Default|Description|
| :---: | :---: |:---|
|--author,-a||Author Info|
|--change,-c||Apply Dockerfile instruction to the created image eg:"CMD echo date"|
|--message,-m||Commit message|
|--pause,-p|true|Pause container during commit|


#### TAG

```
docker tag [OPTIONS] IMAGE[:TAG] [REGISTRYHOST/][USERNAME/]NAME[:TAG]
```
## 镜像导入和导出

#### SAVE

```
docker save [OPTIONS] IMAGE [IMAGE...]
docker save -o xxx.gz imageName1 imageName2  
docker save --output xxx.gz imageName1 imageName2
```

- Save one or more images to a tar archive (streamed to STDOUT by default)
- Usage : docker save [OPTIONS] IMAGE [IMAGE...]
    - --output ,-o Write to a file ,instead of STDOUT

#### Load

读取docker镜像
```
docker load -i file
docker load -input file
```
- Load an image from a tar archive or STDIN
- Usage: docer load [OPTIONS]
    - --input -i: Read from tar archive file ,instead of STDIN
    - --quiet -q: Suppress the load output
