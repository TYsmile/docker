# docker
docker学习总结

镜像搜索 -docker search  

作用：搜索docker HUb（镜像仓库）上的镜像

命令格式：docker search [OPTIONS] TERM

命令参数：-f, --filter filter 根据提供的格式筛选结果

             --format string 利用Go语言的format格式化输出结果 
             
             --limit int  展示最大的结果数，默认25个
             
             -- no-trunc 内容全部展示
             
例子：docker search -f is-official=true ubuntu     搜索官方的ubuntu镜像



镜像查看 --  docker images  /  docker image ls

镜像下载（可以指定版本号） --  docker pull 

镜像删除 --  docker rmi 名称:版本号 / IMAGE ID          /   docker image rm

        强制删除     --  docker rmi -f  
        
镜像保存备份 --  docker save 名称：TAG

镜像备份导入 --  docker load -i 文件名

镜像重命名  --  docker tag 旧名称 新名称:新版本号

镜像详细信息  -- docker image inspect / docker inspect

镜像历史信息  --  docker history 


docker核心技术之容器

1，容器：容器是一种轻量级，可移植，并将应用程序进行打包的技术，使应用程序可以在几乎任何地方以相同的方式运行

* Docker将镜像文件运行起来后，产生的对象就是容器。容器相当于是镜像运行起来的一个实例。

* 容器具备一定的生命周期。

* 另外，可以借助docker ps 命令查看运行的容器，如同在linux上利用ps命令查看运行着的进程。

2，容器与虚拟机

    相同点：
    
    -- 容器和虚拟机一样，都会对物理硬件资源进行共享使用。
    
    -- 容器和虚拟机的声明周期比较相似（创建，运行，暂停，关闭等等）
    
    -- 容器中或虚拟机中都可以安装各种应用，也就是说，在容器中的操作，如同在一个虚拟机中（操作系统）中操作一样
    
    -- 同虚拟机一样，容器创建后，会存储在宿主机上：Linux上位于 /var/lib/docker/containers下
    
    不同点：容器并不是虚拟
    
    -- 虚拟机的创建，启动和关闭都是基于一个完整的操作系统。一个虚拟机就是一个完整的操作系统。而容器直接运行在宿主机的内核上，其本质上以一系列进程的结合。
    
    -- 容器是轻量级的，虚拟机时重量级的。首先容器不需要额外的资源来管理（不需要Hypervisor,Guest,OS),虚拟机额外更多的性能消耗；其次创建，启动，或关闭容器，如同创建，启动或者关闭进程那么轻松，而创建，启动，关闭一个操作系统就没那么方便了。
    
    -- 也因此，意味着在给定的硬件上能运行更多数量的容器，甚至可以直接把Docker运行在虚拟机上。
    
3，容器的生命周期

    -- 

4，容器创建  --  docker create
    
    --  作用：利用镜像创建出一个Created状态的待启动容器
    
