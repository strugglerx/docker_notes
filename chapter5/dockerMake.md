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
