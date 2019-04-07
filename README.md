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
    
    -- 容器和虚拟机的生命周期比较相似（创建，运行，暂停，关闭等等）
    
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
        
        命令参数： -a 作者 ；  -c 为创建的镜像加如Dockerfile命令；  -m 提交信息； -p 提交时暂停容器；
         
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
          
         -- host网络模式
         
          * 容器完全共享主机的网络，网络没有隔离，宿主机的网络就是容器的网络
          
          * 容器，主机上的应用所使用的端口不能重复
          
          * 外部可以直接访问容器，不需要端口映射
          
          * 容器的IP就是宿主机的IP
          
         -- 特殊host网络模式（Container网络模式）
         
          * Container网络模式，其实就是容器共享其他容器的网络； docker run --network container:镜像名 镜像名
          
          * 相当于该容器，在网络层面上，将其他容器作为“主机”。他们之间的网络没有隔离
          
          * 这些容器字间的特性同host模式
         
         -- none 网络模式
          
          * 容器上没有网络，也无任务网络设备
          
          * 如果需要使用网络，需要用户自行安装与配置
          
          * 应用场景：该模式适合需要高度定制网络的用户使用
          
         -- overlay网络模式，也成为覆盖网络
         
          * 实现方式和方案有多种，docker自身集成了一种，基于VXLAN隧道技术实现
          
          * 应用场景：需要管理成百上千个跨主机的容器集群的网络时
          
         -- macvlan 网络模式
         
          * 最主要的特征就是他们的通信会直接基于mac地址进行转发
          
          * 这时宿主主机其实充当一个二层交换机，docker会维护着一个MAC地址表，当宿主机网络收到一个数据包后，直接根据mac地址找到相对应的容器，再把数据交给对应的容器。
          
          * 容器之间可以直接通过IP层互通，通过宿主机上内建的虚拟网络设备（创建macvlan网络时自动创建），但与主机无法直接利用IP互通
          
  
 Docker 之数据管理 
 
        -- 数据集介绍，为什么用数据卷？
        
          * 宿主机无法直接访问容器中的文件
          
          * 容器中的文件没有持久化，导致容器删除后，文件夹数据也随之消失
          
          * 容器之间也无法直接访问互相的文件
          
          数据卷机制：
           
          * 容器与主机之间，容器与容器之间共享文件
          
          * 容器中数据的持久化
          
          * 将容器中的数据备份、迁移、恢复等
        
        -- 数据卷管理
        
          * bind mounts：将宿主机上的一个文件或目录被挂载到容器上
          
          * volumes： 由Docker创建和管理，使用docker volume命令管理
          
          * tmpfs mounts：tmpfs是一种基于内存的临时文件系统，tmpfs mounts数据不会存储在磁盘上 
        
          * docker run/create -v [path]:[path]    ;  docker run --rm -dti --mount type=bind,src=[path],dst=[path]
          
          * docker run -v VOLUME NAME：path   ;  docker run --mount type=volumes,src=...,dst=path
          
          * docker run --mount type=tmpfs, dst=PATH
          
          * 共享其他容器的数据卷： docker run/create --volumes-from CONTAINER
          
        -- 数据卷注意事项
          
          * 更多的使用volumes方式
          
          * 如果挂载一个空的数据卷到容器中的一个非空目录中，那么这个目录下的文件会被复制到数据卷中
          
          * 如果挂载一个非空的数据卷到容器中的一个目录中，那么容器中的目录中会显示数据卷中的数据，如果原来容器中的目录中有数据，那么这些原始数据会被隐藏掉
          
          
Docker仓库 
    
    -- docker仓库就是存放docker镜像并有docker pull方法下载的云环境
    
    -- docker仓库分为私有仓库和共有仓库
      
      * 共有仓库指docker hub（官方）等开放给用户使用，允许用户管理镜像
      
      * 私有仓库指由用户自行搭建的存放镜像的云环境
          
搭建无认证私有仓库
    
    -- 1，在需要搭建仓库的服务器上安装docker
    
    -- 2，在服务器上，从docker hub下载registry仓库： docker pull registry
    
    -- 3，在服务器上，启动仓库： docker run -d -ti --restart always\ --name my-registry\ -p 8000:5000\ -v /my-registry/registry:/var/lib/registry\ registry
    
        * 注意：registry内部对外开放端口是5000，默认情况下，会镜像存放于容器内的/var/lib/registry（官网Dockerfile中查看）目录下，这样如果容器被删除，则存放于容器中的镜像也会丢失。
         
        * 本地利用curl 服务器IP:8000/v2/_catalog 查看当前仓库中的存放的镜像列表。（注意打开8000端口访问）
                                                  
                                          
私有仓库 -上传、下载镜像

      -- 1,利用docker tag重命名需要上传的镜像：docker tag IMAGE 服务器IP:端口/IMAGE_NAME
      
      -- 2，利用docker push上传刚刚重命名的镜像：docker push 服务器IP：端口/centos
      
      -- 3，下载：docker pull 服务器IP：端口/IMAGE_NAME
      
      * 注意：必须重命名为服务器IP：端口/IMAGE_NAME
      
      * 如果push出现了类似htps的错误那么需要往配置文件/etc/docker/daemon.json里添加："insecure-registries":["服务器IP:端口"]  ,然后重启docker
                                                  
          

搭建带认证的私有仓库 

      -- 1，删除先前创建的无认证的仓库容器： docker rm -f my-registry
      
      -- 2,创建存放认证用户名和密码的文件：mkdir /my-registry(可以替换自己路径）/auth -p
      
      -- 3,创建密码验证文件，注意将USERNAME和PASSWORD替换为设置的用户名和密码：
          
          docker run --entrypoint htpasswd registry -Bbn USERNAME PASSWORD > /my-registry/auth/htpasswd
          
      -- 4,重新启动镜像
          
          docker run -d -p:8000：5000 --restart=always --name docker-registry\ -v/my-registry/registry:/var/lib/registry\
                  
                  -v /my-registry/auth:/auth\
                  
                  -e "REGISTRY_AUTH=htpasswd"\
                  
                  -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm"\
                  
                  -e "REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd"\ registry
                
           *  登录： docker login -u username -p password 服务器IP：端口
           
           *  执行pull或者push命令
           
           *  操作完毕后，可以推出登录 docker logout 服务器IP：端口
           
Docker核心技术之dockerfile 

    -- dockerfile简介
    
      * Dockerfile其实就是根据特定的语法格式撰写出来的一个普通的文本文件
      
      *利用docker build命令依次执行在Dockerfile中定义的一系列命令，最终生成一个新的镜像（定制镜像）
      
    --dockerfile示例和使用
       
       * docker build [OPTIONS] PATH | URL | -
       
    -- Dockerfile特征
    
        * 
        
    -- Dockerfile 命令概述 
      
       *  FROM: 指定基础镜像
       
       *  RUN :  构建镜像过程中需要执行的命令，可以由多条。
       
       *  CMD ：添加启动容器时需要执行的命令，多条只有最后一条生效，可以在启动容器时被覆盖修改。
       
       *  ENTRYPOINT ：同CMD，但这个一个会被执行，不会被覆盖修改。
       
       *  LABEL ：为镜像添加对应的数据
       
       *  MAINTAINER ：表名镜像的作者，将被遗弃，被LBALE代替。
       
       *  EXPOSE ：设置对外暴露的端口。
       
       *  ENV ：设置执行命令时的环境变量，并且在构建完成后，仍然生效。
       
       *  ARG ：设置只在构建过程中使用的环境变量，构建完成后，将消失。
       
       *  ADD ：将本地文件或目录拷贝到镜像的文件系统中，能解压特定格式文件，能将URL作为要拷贝的文件。
       
       * COPY ：将本地文件或目录拷贝到镜像的文件系统中
       
       * VOLUME ：添加数据卷
       
       *  USER ： 指定以哪个用户的名义执行RUN，CMD，和ENTRYPOINT等命令。
       
       *  WORKDIR ：设置工作目录。
       
       *  ONBUILD ：如果制作的镜像被另一个Dockerfile使用，将在那里被执行Dockerfile命令
       
       *  STOPSIGNAL : 设置容器退出时发出的关闭信号。
       
       *  HEALTHCHECK ：设置容器状态检查。
       
       *  SHELL ： 更改执行shell命令的程序，linux的默认时shell是[ "/bin/sh" , "-c"] , Windows 的是 [ "cmd" , "/S", "/C"]
       

Dokcer Compose 

    -- 简介：一次性的定义和管理多个容器的启动和关闭
    
    -- Docker Compose File Top配置参数
      
        *   version ：指定Docker Compose File版本号
        
        *   services ：定义多个服务并配置启动参数
        
        *   volumes ：声明或创建在多个服务中共同使用的数据卷对象
        
        *   networks ：定义在多个服务中共同使用的网络对象
        
        *   configs ：声明将在本服务中要使用的一些配置文件
        
        *   secrets ：声明将在本服务中要使用的一些秘匙，密码文件
        
        *   x-*** ： 自定义配置，主要用于复用相同的配置 
