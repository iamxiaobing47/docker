# docker命令

```shell
systemctl enable docker
systemctl start docker
systemctl stop docker
systemctl status docker
systemctl restart docker
docker info 
docker --help
docker <command> --help
```

## inpsect
```shell
docker inspect --type=network mynetwork
docker inspect --format='{{range .NetworkSettings.Networks}}{{println .IPAddress}}{{end}}' <ContianerID>
```

## events

一个实时监控命令，它可以实时显示 Docker 守护进程生成的事件流。这些事件包括但不限于容器的创建、启动、停止、删除，以及镜像的拉取、推送、删除等

### 选项说明：
- `--since=<timestamp>`：从指定的时间戳开始获取事件。
- `--until=<timestamp>`：获取直到指定时间戳的事件。
- `--filter=<filter>`：使用过滤器来限制事件的输出。

### 用法示例：

1. **显示所有 Docker 事件**:
   ```bash
   docker events
   ```

2. **显示自特定时间以来的事件**:
   ```bash
   docker events --since="2024-06-09T10:00:00"
   ```

3. **显示直到特定时间的事件**:
   ```bash
   docker events --until="2024-06-09T11:00:00"
   ```

4. **过滤特定类型的事件**:
   ```bash
   docker events --filter 'event=container:start'
   ```

5. **过滤特定容器的事件**:
   ```bash
   docker events --filter 'container=my_container_name'
   ```

6. **过滤特定镜像的事件**:
   ```bash
   docker events --filter 'image=my_image_name'
   ```

## history

用于显示一个 Docker 镜像的构建历史和元数据。这个命令可以列出镜像构建过程中的每一层，并显示每一层的指令、大小以及创建的时间戳。

### 用法示例：

1. **查看镜像的构建历史**:
   ```bash
   docker history my_image
   ```

2. **以人类可读的格式显示大小**:
   ```bash
   docker history --human my_image
   ```

3. **仅显示镜像 ID**:
   ```bash
   docker history -q my_image
   ```

### 特点和用途：
- `docker history` 可以帮助用户了解镜像是如何构建的，以及每一层都做了什么操作
- 通过查看历史记录，用户可以优化 Dockerfile，减少镜像大小
