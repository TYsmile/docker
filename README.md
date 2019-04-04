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
