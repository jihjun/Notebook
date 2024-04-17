要使用Docker运行MongoDB，您可以通过几个简单的步骤来设置和启动MongoDB容器。这里将介绍如何从Docker Hub上拉取MongoDB的官方镜像、启动MongoDB容器，并配置一些基本的设置。

### 第一步：安装Docker

确保您的机器上已安装Docker。如果未安装，您可以从[Docker官网](https://www.docker.com/products/docker-desktop)下载并安装Docker Desktop（支持Windows和MacOS）或在Linux上安装Docker Engine。

### 第二步：拉取MongoDB的Docker镜像

打开终端或命令行工具，执行以下命令来拉取最新版的MongoDB镜像：

```bash
docker pull mongo
```

### 第三步：启动MongoDB容器

使用以下命令启动MongoDB容器，这里还包括了一些常用的配置：

```bash
docker run -d --name mongodb-container -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=mongoadmin -e MONGO_INITDB_ROOT_PASSWORD=secret mongo
```

这条命令的参数说明如下：
- `-d`：后台运行容器。
- `--name mongodb-container`：为容器命名为`mongodb-container`。
- `-p 27017:27017`：将容器的27017端口映射到宿主机的27017端口，MongoDB默认使用此端口。
- `-e MONGO_INITDB_ROOT_USERNAME=mongoadmin`：设置MongoDB的管理员用户名为`mongoadmin`。
- `-e MONGO_INITDB_ROOT_PASSWORD=secret`：设置管理员密码为`secret`。
- `mongo`：指定要运行的镜像。

### 第四步：连接到MongoDB

一旦容器运行起来，您可以使用任何MongoDB客户端通过`localhost:27017`连接到数据库。使用上面设置的用户名和密码进行连接。

### 使用MongoDB Shell

如果您想直接进入MongoDB容器中使用Mongo shell，可以使用以下命令：

```bash
docker exec -it mongodb-container mongo -u mongoadmin -p secret
```

这将允许您以管理员身份登录到Mongo shell。

### 数据持久化

如果您想要数据持久化，即容器删除后数据仍然保存，您应该将MongoDB的数据目录挂载到宿主机的一个目录。可以通过添加 `-v` 参数到 `docker run` 命令来实现：

```bash
docker run -d --name mongodb-container -p 27017:27017 -e MONGO_INITDB_ROOT_USERNAME=mongoadmin -e MONGO_INITDB_ROOT_PASSWORD=secret -v /my/own/datadir:/data/db mongo
```

在这里，`/my/own/datadir` 是宿主机上的目录，用于存储MongoDB的数据。

通过以上步骤，您可以快速启动并运行一个MongoDB的Docker容器，进行开发和测试。