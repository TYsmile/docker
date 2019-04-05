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

    -- 运行：docker run  ==  docker create + docker start -a 前台模式
    
    -- docker run -d 相当于 docker create + docker start | 后台模式
    
    -- 暂停： docker pause 
    
    -- 关闭:  docker stop
    
    -- 重启： docker restart
    
    -- 查看日志信息： docker logs
    
    -- 容器重命名 docker rename old new

4，容器创建  --  docker create

    --  命令参数:   -t 运行终端； -i 即使没有连接，也要把持STDIN打开；
    
    --  作用：利用镜像创建出一个Created状态的待启动容器
     
5,容器运行时 动作：

    -- 容器连接,作用：将当前终端的STDIN，STDOUT，STDERR绑定到正在运行的容器的主进程上实现连接： docker attach 
    
    -- 容器中执行新命令，作用：在容器中运行一个命令： docker exec 
    
  
容器与镜像的关系
    
    -- 容器提交 ：  docker commit   带有历史信息  ，经常用，重点
    
        作用：根据容器生成一个新的镜像  
        
        命令格式： docker commit [options] CONTAINER [REPOSITORY[:TAG]]
        
        命令参数： -a 作者 ；  -c 为创建的镜像假如Dockerfile命令；  -m 提交信息； -p 提交时暂停容器；
         
    --  容器导出 : docker export    全新镜像
        
        作用： 将容器当前的文件系统导出成一个tar文件
        
        命令格式 docker export [options] CONTAINER
 
        参数： -o  filename 
        
     --  容器打包的导入 : docker import 
         
         作用： 从一个tar文件中导入内容创建一个镜像
         
         命令格式: docker import [options] file|URL|- [RESPOSITORTY[:TAG]]
         
         
  Docker核心技术之网络管理 
    
      1，docker网络管理
       
        容器的网络默认与宿主机，与其他容器都是相互隔离。
        
        *  容器中可以运行一些网络应用（如nginx，web应用，数据库等），如果要让外部也可以访问这些容器内运行的网络应用，那么就需要配置网络来实现。
        
        *  有可能有的需求下，容器不想让它的网络与宿主机，与其它容器隔离。
        
        *  sometimes，容器根本不需要网络 ； （比如计算型任务）
        
        *  sometimes，容器需要更高的定制化网络（如定制特殊的集群网络，定制容器间的局域网）。
        
        *  sometimes，容器数量特别多，体量很大的一系列容器的网络管理如何
        
      2，docker中有五种网络驱动模式 
      
        *  bridge network 模式（网桥）：默认的网络模式。类似虚拟机的nat模式
        
        *  host network模式（主机）：容器与宿主之间的网络无隔离，即容器直接使用宿主机网络
        
        *  None network 模式： 容器禁用所有网络
        
        *  Overlay network 模式：利用VXLAN实现的bride模式
        
        *  Macvian network模式：容器具备MAc地址，使其显示为网络上的物理设备
        
      3，docker 网络管理命令 
        
        --  查看已经建立的网络对象 docker network ls   
        
        --  创建网络： docker networkk create
        
        --  删除网络： docker network rm 
        
        --  查看网络详细信息 : docker network inspect
        
        --  使用网络： docker run --network 网络名 镜像名
        
        --  网络连接与断开： docker network connect/disconnect [-f] 网络名 镜像名
        
      4， docker 网络模式简介
      
        -- bridge 网络模式 
        
          * 宿主机上需要单独的bridge网卡，如默认docker默认创建的docker0
          
          * 容器之间，容器与主机之间的网络通信，是借助为每一个容器生成的一对veth pair虚拟网络设备对，进行通信的。一个容器上，另一个在宿主机上
          
          * 每创建一个基于bridge网络的容器，都会自动在宿主机上创建一个veth** 虚拟网络设备
          
          * 外部无法直接访问容器，需要建立端口映射才能访问
          
          * 容器借由veth虚拟设备通过如docker0这种bridge网络设备进行通信
          
          * 每一容器具有单独的IP
