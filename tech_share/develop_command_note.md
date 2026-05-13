# 开发过程常用命令记录

## 一、linux运维常见的命令

### 1、系统常用命令

#### 1.1 linux 快捷链接

ln -s  /mnt/bigdisk/dev_workspace/dev_running_data/1panel/  1panel

```shell
root@lckfb:~# ll

total 172

dr-xr-x--- 11 root root  4096 May 11 23:19 ./

drwxr-xr-x 25 root root  4096 Jan 15 23:03 ../

lrwxrwxrwx  1 root root    51 May 11 12:02 1panel -> /mnt/bigdisk/dev_workspace/dev_running_data/1panel/
```



#### 1.2、系统注册服务查看

```shell
systemctl status
systemctl start docker
systemctl restart docker
systemctl stop docker
```

### 1.3、screen命令实现命令行的多命令行操作

```shell
 1749  screen -h
 1750  screen -ls
 1751  screen -S wol
 1764  screen -rD wol
```

### 1.4、系统命令手册查看

```shell
man
help
```

### 1.5、常用的操作命令

```shell
tar 
zip
rpm
lsblk
free
top
ssh
watch
tail
cat
less
vim
nano
fdisk
mount
ln
```



### 1.6、配合调试其他系统的命令

```shell
adb
telnet
curl
ping 
tracert
```





## 二、docker

#### 2.1、show doker active server status

```bash
[root@192 ~]# docker system df
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              13                  5                   10.67 GB            8.544 GB (80%)
Containers          10                  2                   131.6 kB            131.6 kB (99%)
Local Volumes       2                   0                   132 kB              132 kB (100%)

查看特定容器的资源使用情况
docker stats <container_id_or_name>


查看容器的进程
使用 `docker top` 命令可以查看容器内的进程：

查看容器的事件
使用 `docker events` 命令可以实时查看 Docker 守护进程的事件：
```

#### 2.2、export  all docker images

```bash
bash
复制代码运行
docker save -o all_images.tar $(docker images -q)
   
这个命令会将所有镜像打包成一个名为all_images.tar的文件。其中，docker images -q用于获取所有镜像的ID，然后通过docker save命令将这些镜像保存到指定的文件中。
```

#### 2.3、docker compose 的升级

```shell
curl -SL https://github.com/docker/compose/releases/download/v2.29.4/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

#### 2.4、查看容器的日志

```shell
使用 `docker logs` 命令可以查看容器的日志输出：

查看容器的实时日志
使用 `docker logs -f` 命令可以实时查看容器的日志输出：
```

#### 2.5、查看 `my_container` 的详细信息：

~~~shell
```sh
docker inspect my_container
```
~~~

#### 2.6、清理现有 Docker 网络

~~~shell
如果存在多个 Docker 网络或者有损坏的网络，它们可能会导致冲突。你可以列出所有的 Docker 网络并移除不必要的网络。
```bash
docker network ls
docker network rm <network_id_or_name>
```
~~~

#### 2.7、docker设置内存运行

~~~shell
示例：为 RocketMQ 指定 200M 内存

```bash
docker run -d \
  --name rocketmq-broker \
  --memory="200m" \
  rocketmqinc/rocketmq:latest
```


```bash
docker run -d \
  --name nacos-server \
  --memory="512m" \
  nacos/nacos-server:latest
```

除了硬性的内存限制外，还可以设置一个软性限制 (`--memory-reservation`)，它指定了当系统内存不足时优先分配给该容器的最小内存量。这有助于在多租户环境中更好地管理资源竞争。

```bash
docker run -d \
  --name my-app \
  --memory="256m" \
  --memory-reservation="128m" \
  my-app-image:tag
```


配置交换分区 (Swap)

你还可以控制容器是否可以使用 swap 分区，以及如果使用的话，swap 的大小相对于物理内存的比例。例如，禁止使用 swap 或者设置一个合理的比例：

```bash
docker run -d \
  --name my-app \
  --memory="256m" \
  --memory-swap="512m" \  # 允许最多 256M 物理内存 + 256M swap
  my-app-image:tag
```

4. 使用 Docker Compose

如果你使用 Docker Compose 来管理多个服务，可以在 `docker-compose.yml` 文件中定义每个服务的资源限制。

```yaml
version: '3'
services:
  rocketmq-broker:
    image: rocketmqinc/rocketmq:latest
    container_name: rocketmq-broker
    deploy:
      resources:
        limits:
          memory: 200M
    # ...其他配置项...
  
  nacos-server:
    image: nacos/nacos-server:latest
    container_name: nacos-server
    deploy:
      resources:
        limits:
          memory: 512M
   -- # ...其他配置项...
    
```
然后使用 `docker-compose up -d` 启动这些服务。
~~~

