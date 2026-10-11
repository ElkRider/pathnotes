1.关闭异常内存占用导致的cpu等待长时间的io导致的云服务器的假死

```
systemctl stop docker.socket

[root@iZuf6chlqotgoe8s0enis8Z ~]# systemctl stop docker.socket 
[root@iZuf6chlqotgoe8s0enis8Z ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           1.7G        179M        983M        108K        593M        1.4G
Swap:          1.0G         85M        938M

```

```
[root@iZuf6chlqotgoe8s0enis8Z ~]# ps aux --sort=-%mem | head -20
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root      1762  1.5 65.0 6888732 1170684 ?     Sl   May21 192:14 /usr/local/bin/v2ray run --config=/etc/v2raya/config.json
root      1674  1.7  2.3 1334580 41468 ?       Ssl  May21 215:46 v2raya
root      1675  0.4  1.8 1422924 33336 ?       Ssl  May21  53:11 /ko-app/frpp master
mongod    1146  0.3  0.9 2670452 17720 ?       Ssl  May21  44:46 /usr/bin/mongod --config /etc/mongod.conf
collie   25478  0.8  0.9 1132508 17632 ?       Sl   00:08   0:03 /opt/collie/services/collie-agent/engine daemon
root     27683  0.6  0.7 167100 13640 ?        Dsl  May26  34:08 /usr/local/aegis/aegis_client/aegis_12_93/AliYunDunMonitor
root     25572  0.7  0.5 1393872 10108 ?       Dsl  00:13   0:00 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
root       941  0.0  0.4 1496784 8992 ?        Ssl  May21   9:04 /usr/bin/containerd
collie    1147  0.0  0.2  68472  4444 ?        Dsl  May21   5:41 /opt/collie/collie daemon --runtime-dir /opt/collie
root     15506  0.1  0.1 689704  3080 ?        Ssl  May23  13:41 /usr/local/share/aliyun-assist/aliyun-service.symlink
root     25545  0.0  0.1 149344  2812 pts/0    S+   00:10   0:00 vim docker-compose.yml
root      1613  0.0  0.1 1236192 2348 ?        Sl   May21   1:43 /usr/bin/containerd-shim-runc-v2 -namespace moby -id e8addfa813317a8fd54bcffa6fc3defd7f01a374f50de33e6d7defe1f18edd96 -address /run/containerd/containerd.sock
root     27649  0.3  0.1 135564  2300 ?        Ssl  May26  17:22 /usr/local/aegis/aegis_client/aegis_12_93/AliYunDun
root      1610  0.0  0.1 1236448 2292 ?        Sl   May21   2:29 /usr/bin/containerd-shim-runc-v2 -namespace moby -id deb092b0303240299a37f5483521314b80cf694c36fba725c91df123ae69b0e9 -address /run/containerd/containerd.sock
root     25104  0.0  0.1 117112  2000 pts/0    Ss   May29   0:00 -bash
root     25254  0.0  0.1 117004  1900 pts/1    Ss   May29   0:00 -bash
root     25589  0.1  0.1 117396  1860 ?        D    00:15   0:00 sshd: [accepted]
root      1040  0.6  0.1 1438040 1840 ?        Sl   May21  72:54 /usr/local/cloudmonitor/bin/argusagent
root     25590  0.0  0.0 155588  1752 pts/1    R+   00:15   0:00 ps aux --sort=-%mem





[root@iZuf6chlqotgoe8s0enis8Z ~]# ps -ef | grep docker
root     25545 25104  0 00:10 pts/0    00:00:00 vim docker-compose.yml
root     25572     1  0 00:13 ?        00:00:00 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
root     25581 25254  0 00:14 pts/1    00:00:00 grep --color=auto docker







[root@iZuf6chlqotgoe8s0enis8Z ~]# top
e
top - 00:06:31 up 8 days, 10:15,  2 users,  load average: 39.34, 45.05, 44.07
Tasks:  98 total,   2 running,  96 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  5.0 sy,  0.0 ni,  0.0 id, 95.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  1798512 total,    66160 free,  1599928 used,   132424 buff/cache
KiB Swap:  1048572 total,   640864 free,   407708 used.    57768 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                                                       
 2308 collie    20   0    1.1g   0.0g   0.0g D   1.0  0.7  28:02.05 engine                                                                                                        
    1 root      20   0    0.0g   0.0g   0.0g D   0.0  0.1   9:13.71 systemd                                                                                                       
    2 root      20   0    0.0g   0.0g   0.0g S   0.0  0.0   0:00.08 kthreadd                                                                                                      
    4 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 kworker/0:0H                                                                                                  
    6 root      20   0    0.0g   0.0g   0.0g S   0.0  0.0  12:45.41 ksoftirqd/0                                                                                                   
    7 root      rt   0    0.0g   0.0g   0.0g S   0.0  0.0   0:05.26 migration/0                                                                                                   
    8 root      20   0    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 rcu_bh                                                                                                        
    9 root      20   0    0.0g   0.0g   0.0g S   0.0  0.0  11:28.98 rcu_sched                                                                                                     
   10 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 lru-add-drain                                                                                                 
   11 root      rt   0    0.0g   0.0g   0.0g S   0.0  0.0   0:07.87 watchdog/0                                                                                                    
   12 root      rt   0    0.0g   0.0g   0.0g S   0.0  0.0   0:02.73 watchdog/1                                                                                                    
   13 root      rt   0    0.0g   0.0g   0.0g S   0.0  0.0   0:01.65 migration/1                                                                                                   
   14 root      20   0    0.0g   0.0g   0.0g S   0.0  0.0  10:03.68 ksoftirqd/1                                                                                                   
   16 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 kworker/1:0H                                                                                                  
   18 root      20   0    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 kdevtmpfs                                                                                                     
   19 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 netns                                                                                                         
   20 root      20   0    0.0g   0.0g   0.0g S   0.0  0.0   0:00.27 khungtaskd                                                                                                    
   21 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 writeback                                                                                                     
   22 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 kintegrityd                                                                                                   
   23 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 bioset                                                                                                        
   24 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 bioset                                                                                                        
   25 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 bioset                                                                                                        
   26 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 kblockd                                                                                                       
   27 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 md                                                                                                            
   28 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 edac-poller                                                                                                   
   29 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 watchdogd                                                                                                     
   36 root      20   0    0.0g   0.0g   0.0g S   0.0  0.0  11:04.49 kswapd0                                                                                                       
   37 root      25   5    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 ksmd                                                                                                          
   38 root      39  19    0.0g   0.0g   0.0g S   0.0  0.0   0:00.68 khugepaged                                                                                                    
   39 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 crypto                                                                                                        
   47 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 kthrotld                                                                                                      
   49 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 kmpath_rdacd                                                                                                  
   50 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 kaluad                                                                                                        
   51 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 kpsmoused                                                                                                     
   52 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 ipv6_addrconf                                                                                                 
   65 root       0 -20    0.0g   0.0g   0.0g S   0.0  0.0   0:00.00 deferwq   
```

```
top - 23:50:56 up 8 days,  9:59,  1 user,  load average: 41.39, 40.59, 38.90
Tasks:  98 total,   1 running,  97 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.5 us,  1.9 sy,  0.0 ni,  0.0 id, 97.4 wa,  0.0 hi,  0.1 si,  0.0 st
KiB Mem : 97.2/1798512  [                                                                                                    ]
KiB Swap: 39.8/1048572  [                                                                                                    ]

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                                                       
   36 root      20   0    0.0m   0.0m   0.0m S   1.3  0.0  10:30.20 kswapd0                                                                                                       
  231 root       0 -20    0.0m   0.0m   0.0m S   0.9  0.0   3:51.62 kworker/0:1H                                                                                                  
 2308 collie    20   0 1109.0m  12.4m   0.4m D   0.9  0.7  27:51.37 engine                                                                                                        
27683 root      20   0  163.2m  13.8m   1.1m S   0.7  0.8  33:53.94 AliYunDunMonito                                                                                               
27649 root      20   0  132.4m   2.3m   0.0m S   0.6  0.1  17:14.08 AliYunDun                                                                                                     
  357 root      20   0  289.5m   0.5m   0.3m D   0.4  0.0   7:58.62 systemd-journal                                                                                               
 1040 root      20   0 1404.3m   1.7m   0.0m S   0.4  0.1  72:45.59 argusagent                                                                                                    
 1762 root      20   0 6727.3m   1.1g   0.0m S   0.4 65.2 192:11.15 v2ray                                                                                                         
  941 root      20   0 1461.7m   8.0m   0.0m S   0.2  0.5   8:36.68 containerd                                                                                                    
 1146 mongod    20   0 2607.9m  16.8m   0.0m S   0.2  1.0  44:42.00 mongod                                                                                                        
 1674 root      20   0 1303.3m  40.5m   0.0m S   0.2  2.3 215:43.43 v2raya                                                                                                        
15506 root      20   0  673.5m   3.0m   0.0m S   0.2  0.2  12:58.35 aliyun-service.                                                                                               
25211 root      20   0  158.3m   1.2m   0.5m R   0.2  0.1   0:00.12 top                                                                                                           
27562 root      20   0   52.0m   1.4m   0.0m S   0.2  0.1   3:53.27 AliYunDunUpdate                                                                                               
    1 root      20   0   50.8m   1.5m   0.3m D   0.0  0.1   9:13.30 systemd                                                                                                       
    2 root      20   0    0.0m   0.0m   0.0m S   0.0  0.0   0:00.08 kthreadd                                                                                                      
    4 root       0 -20    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 kworker/0:0H                                                                                                  
    6 root      20   0    0.0m   0.0m   0.0m S   0.0  0.0  12:40.99 ksoftirqd/0                                                                                                   
    7 root      rt   0    0.0m   0.0m   0.0m S   0.0  0.0   0:05.24 migration/0                                                                                                   
    8 root      20   0    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 rcu_bh                                                                                                        
    9 root      20   0    0.0m   0.0m   0.0m S   0.0  0.0  11:28.50 rcu_sched                                                                                                     
   10 root       0 -20    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 lru-add-drain                                                                                                 
   11 root      rt   0    0.0m   0.0m   0.0m S   0.0  0.0   0:07.84 watchdog/0                                                                                                    
   12 root      rt   0    0.0m   0.0m   0.0m S   0.0  0.0   0:02.71 watchdog/1                                                                                                    
   13 root      rt   0    0.0m   0.0m   0.0m S   0.0  0.0   0:01.65 migration/1                                                                                                   
   14 root      20   0    0.0m   0.0m   0.0m S   0.0  0.0   9:59.79 ksoftirqd/1                                                                                                   
   16 root       0 -20    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 kworker/1:0H                                                                                                  
   18 root      20   0    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 kdevtmpfs                                                                                                     
   19 root       0 -20    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 netns                                                                                                         
   20 root      20   0    0.0m   0.0m   0.0m S   0.0  0.0   0:00.27 khungtaskd                                                                                                    
   21 root       0 -20    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 writeback                                                                                                     
   22 root       0 -20    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 kintegrityd                                                                                                   
   23 root       0 -20    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 bioset                                                                                                        
   24 root       0 -20    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 bioset                                                                                                        
   25 root       0 -20    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 bioset                                                                                                        
   26 root       0 -20    0.0m   0.0m   0.0m S   0.0  0.0   0:00.00 kblockd 
```







```
root@iZuf6chlqotgoe8s0enis8Z ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2026-05-21 13:51:31 CST; 1 weeks 1 days ago
     Docs: https://docs.docker.com
 Main PID: 1150 (dockerd)
    Tasks: 12
   Memory: 29.9M
   CGroup: /system.slice/docker.service
           └─1150 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock


^C^C^Cdo
cker  s^C




docker stop v2raya
[root@iZuf6chlqotgoe8s0enis8Z ~]# 
[root@iZuf6chlqotgoe8s0enis8Z ~]# 
[root@iZuf6chlqotgoe8s0enis8Z ~]# 
[root@iZuf6chlqotgoe8s0enis8Z ~]# 
[root@iZuf6chlqotgoe8s0enis8Z ~]# docker stop v2raya
^C^C^C^C^C
[root@iZuf6chlqotgoe8s0enis8Z ~]# ^C

```

```
[root@iZuf6chlqotgoe8s0enis8Z ~]# docker ps
CONTAINER ID   IMAGE                     COMMAND                  CREATED         STATUS                            PORTS     NAMES
913ef21af856   mzz2017/v2raya:v2.2.7.5   "v2raya"                 4 minutes ago   Restarting (137) 19 seconds ago             v2raya
28bec5ba17d4   vaalacat/frp-panel        "/ko-app/frpp curl -…"   4 months ago    Restarting (0) 7 seconds ago                frp-panel-server

```

