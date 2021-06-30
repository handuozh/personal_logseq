-
- `dockerignore`
	- `.git`
	- `node_modules`
	- `npm-debug.log`
- 例子
	-
	  ```shell
	  FROM node:8.4
	  COPY . /app
	  WORKDIR /app
	  RUN npm install --registry=https://registry.npm.taobao.org
	  EXPOSE 3000
	  ```
		- `COPY . /app`
			- 将当前目录下的所有文件（除了`.dockerignore`  排除的路径），都拷贝进入 image 文件的 `/app` 目录
		- `WORKDIR /app`
			- 指定接下来的工作路径为`/app`
		- `EXPOSE 3000`
			- 将容器 `3000` 端口暴露出来， 允许外部连接这个端口
	-