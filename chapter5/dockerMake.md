## Image 相关的操作

- Image的生成途径
    - Dockerfile
    - 基于容器制作镜像
        - Create a new image from a container‘s changes
        - Usage
            - [docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]](#Usage)
    - Docker Hub automated builds


#### Usage

|Name,shorthand|Default|Description|
| :---: | :---: |:---|
|--author,-a||Author Info|
|--change,-c||Apply Dockerfile instruction to the created image|
|--message,-m||Commit message|
|--pause,-p|true|Pause container during commit|
