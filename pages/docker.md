public:: true

- #related [[container]] [[virtual machine]]
-
- 概念
  collapsed:: true
	- 容器 container
	- 镜像 image
	- 网络 network
- 服务管理
  collapsed:: true
	- service docker start       # 启动 docker 服务，守护进程
	- service docker stop        # 停止 docker 服务
	- service docker status      # 查看 docker 服务状态
	- chkconfig docker on        # 设置为开机启动
- 镜像管理
  collapsed:: true
	- 删除所有镜像
		-
		  ```shell
		  docker rmi $(docker images | grep none | awk '{print $3}' | sort -r)
		  ```
	- 连接进行进入命令行模式，exit命令退出
		- `docker run -t -i nginx:latest /bin/bash`
- 命令
  heading:: true
	- `docker attach`
		- 介入到一个正在运行的容器
		- 前提是这个容器必须是由`docker run -d`启动的
	- `docker build`
		- 根据 [[Dockerfile]] 构建一个镜像
			-
			  ```shell
			  touch Dockerfile .dockerignore
			  ```
			- `docker image build -t han-demo:0.0.1 .`
				- 如果不指定标签就是默认的`latest`
	- `docker commit`
		- 根据容器的更改创建一个新的镜像
		- docker commit -m="First Docker" -a="handuo" a6b0a6cfdacf handuo/nginx:v1.2.1
			- `-m` 提交的描述信息
			- `-a` 镜像作者
			- `a6b0a6cfdacf ` 这个是容器id，不是镜像id
			- `handuo/nginx:v1.2.1` 创建的目标镜像名
	- `docker exec`
		- 在容器中执行一条命令
	- `docker run`
		- 在一个新的容器中执行一条命令
		- `-it` iteractivley
		- `-rm` causes Docker to automatically remove the container when it exits
	- `docker images`
	- `docker kill`
		- 杀死一个或多个正在运行的容器
	- `docker logs`
	- `docker pause` / `docker unpause`
	- `docker ps`
		- 列出所有容器
	- `docker restart`
		- 重新启动一个或多个容器
	- `docker rm`
		- 删除一个或多个容器
	- `docker rmi`
		- 删除一个或多个镜像
	- `docker tag`
		- 为镜像创建一个新的标签
	- `docker top`
		- 显示一个容器内的所有进程
- 容器管理
  heading:: true
	- `docker container ls`
		- 列出本机正在运行的容器
	- `docker container ls --all`
		- 列出本机所有容器，包括终止运行的容器
	- `docker exec -it [containerID/Names] /bin/bash`
		- 进入容器
	- `docker container cp [containID]:[/path/to/file] .`
		- 从正在运行的 Docker 容器里面，将文件拷贝到本机
		- 注意后面有个`.`拷贝到当前目录
	- `docker ps` and `docker ps -a`
	-
- 容器服务管理
  heading:: true
	- `docker run -itd --name my-nginx2 nginx`
		- 通过nginx镜像，【创建】容器名为 my-nginx2 的容器
		- `-d`/ `--detach`
			- 后台模式
			- 表示让Docker容器在终端的后台运行，它不接收输入或显示输出
	- `docker start my-nginx --restart=always`
		- 启动策略: 一个已经存在的容器启动添加策略
		- no - 容器不重启
		- on-failure - 容器推出状态非0时重启
		- always - 始终重启
	- `docker start my-nginx`
		- 【启动】一个已经存在的容器
	- `docker restart my-nginx`
		- 【重启】容器
	- `docker stop my-nginx`
		- 【停止运行】一个容器
	- `docker kill my-nginx`
		- 【杀死】一个运行中的容器
	- `docker rename my-nginx new-nginx`
		- 【重命名】容器
	- `docker rm new-nginx`
		- 【删除】容器