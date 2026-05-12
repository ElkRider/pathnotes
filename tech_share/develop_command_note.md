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