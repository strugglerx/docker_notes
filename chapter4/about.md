## 关于 Docker Images

- pid为1 的问题
    - docker容器启动的是欧，默认会把容器的第一个进程，也就是pid为1的程序，作为docker容器正在运行的标志，如果docker容器中的pid为1的进程关闭，docker容器就会推出。
    - 当执行自定义cmd的时候，bash或者sh的pid就变成了1，一旦执信完自定义cmd，容器也就推出了。

- 采用分层构建机制，最底层为bootfs，其次是rootfs
    - bootfs ： 用于系统引导的文件系统，包括bootloader 和 kernel，容器启动完成后会被释放以节约资源
    - rootfs ： 位于bootfs之上，表现为docker容器的根目录
        - 传统模式中，系统启动时，内核挂载rootfs时会首先将其挂载为“只读”模式，完整自检完成后将其重新挂载为读写模式
        - docker中，rootfs有内核挂载为“只读”模式，而后通过“联合挂载”技术额外挂载一个“可写”层