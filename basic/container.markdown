# docker run

**基本用法**：

```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

**OPTION**：

- `-d`：后台运行容器，并打印容器ID
- `-i` ：让容器保持交互模式，即使没有附加到容器的标准输入。这允许容器继续运行，即使没有用户输入。通常与 -t 一起使用，以确保容器不会在没有交互时停止。
- `-t`：这个选项分配一个伪终端或终端给容器
- `--name="容器名"`：为容器指定一个名称
- `--restart="策略"`：设置容器的重启策略。no 或者 always
- `-p`：端口映射，格式为 `<宿主机端口>:<容器端口>`
- `-e`：设置环境变量。
- `-v`：挂载卷 `<宿主机目录>:<容器目录>`。默认是rw,也可以改成ro，容器只有可读权限
- `--network`：指定容器的网络连接。
- `-w` ：指定容器内的工作目录

3. **COMMAND**：覆盖镜像中的默认命令。

4. **ARG**：传递给命令的参数。


  ```bash
  docker run ubuntu:latest echo "Hello, Docker!"
  ```

  ```bash
  docker run -d -p 8080:80 nginx
  ```

  ```bash
  docker run -e "ENV_VAR=value" ubuntu:latest
  ```
只在当前会话有效，重起docker后，环境变量失效

  ```bash
  docker run -v /path/on/host:/path/in/container ubuntu:latest
  ```

  ```bash
  docker run -w /path/to/work ubuntu:latest
  ```

## exec 和 attch

```shell
docker exec -it <Container> /bin/bash
docker attach <Container> /bin/bash
```

1. **docker attach**:

   - 使用 `docker attach` 进入容器后，你实际上是连接到了容器启动时指定的初始进程的终端。
   - 如果使用 `exit` 命令或者关闭连接终端，这将导致容器的初始进程接收到 SIGHUP 信号，这通常会导致进程退出。如果这个进程是容器的 PID 1 进程（即容器的根进程），那么容器将停止运行
2. **docker exec**:

   - 这个命令允许你在容器内启动新的进程，并且可以与这个新进程进行交互。
   - 使用 `docker exec` 启动的进程是容器内的一个新进程，而不是初始进程。
   - 如果你使用 `exit` 命令退出这个新进程，它不会影响容器的运行，因为**容器的运行状态是由初始进程决定的**

# container 命令

```shell
docker start <Container>
docker restart <Container>
docker stop <Container>
docker kill <Container>
docker rm <Container>
docker rm -f $(docker ps -aq)
docker logs <Container>
docker top <Container>
```

## copy

```bash
docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH
docker cp [OPTIONS] SRC_PATH DEST_PATH
```

1. **从容器复制文件到主机**:

   ```bash
   docker cp my_container:/path/to/container/file /path/to/destination/on/host
   ```
2. **从主机复制文件到容器**:

   ```bash
   docker cp /path/to/source/on/host my_container:/path/to/destination/in/container
   ```
3. **使用通配符复制多个文件**:

   ```bash
   docker cp my_container:/path/to/container/* /path/to/destination/on/host
   ```

   这将把容器内 `/path/to/container/` 目录下的所有文件**不包括目录** 复制到主机的 `/path/to/destination/on/host` 路径下

### 注意事项：

- 如果目标路径不存在，`docker cp` 会尝试创建必要的目录结构
- `docker cp` 命令在复制文件时不会保留文件的权限和所有权，除非你使用 `--chmod` 选项来指定 `--chmod=755`

## diff

- `A` (Added)：文件或目录被添加。
- `D` (Deleted)：文件或目录被删除。
- `C` (Changed)：文件内容被修改。
- `M` (Modified)：文件权限或元数据被修改。


**查看特定容器的文件系统更改**:

```bash
docker diff my_container
```

- 如果容器正在运行，`docker diff` 将显示自容器启动以来的所有更改

## wait

用于等待一个或多个容器停止运行。当容器停止时，docker wait 命令会返回容器的退出状态码

```shell
docker wait my_app_container
exit_status=$(docker wait my_app_container)

if [ "$exit_status" -eq 0 ]; then
   echo "容器正常退出，状态码：$exit_status"
   # 这里可以添加一些清理工作的命令
else
   echo "容器异常退出，状态码：$exit_status"
   # 这里可以添加一些错误处理的命令
fi
```

## pause

```shell
docker pause <Container>
docker unpause <Container>
```

## port

列出容器的端口映射情况

- `--no-trunc`：显示完整的端口映射列表，不截断

1. **查看指定容器的所有端口映射**:

   ```bash
   docker port my_container
   ```
2. **查看指定容器特定端口的映射**:

   ```bash
   docker port my_container 80
   ```
3. **以完整形式查看端口映射**:

   ```bash
   docker port --no-trunc my_container
   ```

## export and import

```shell
docker export my_container > my_container_fs.tar
docker import my_container_fs.tar my_new_image
```
- 数据来源：docker load 加载的是镜像的归档，而 docker import 导入的是容器的文件系统
- 元数据保留：docker load 会保留镜像的原有标签和元数据，docker import 则不会，需要指定新的标签