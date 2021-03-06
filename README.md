Docker Official
====

docker 官方镜像使用及配置

# 初始化环境 #

## 安装Docker ##
```sh
curl -sSL https://get.docker.com | sh
```

# 操作命令 #

## 获取 FULL CONTAINER ID
```sh
docker inspect -f '{{.Id}}' SHORT_CONTAINER_ID || CONTAINER_NAME
```

## 复制 ##

### 1. 从 container 到 主机（host）
使用 docker cp 命令
```sh
　　docker cp <SHORT_CONTAINER_ID || CONTAINER_NAME>:/PATH/TO/FILE/WITHIN/CONTAINER /PATH/TO/FILE
```
 
### 2. 从 主机（host） 到 container
```sh
# 获取 FULL CONTAINER ID
# 1. Aufs
sudo cp PATH/TO/FILE /var/lib/docker/aufs/mnt/${FULL_CONTAINER_ID}/PATH/TO/FILE

# 2. Btrfs
sudo cp PATH/TO/FILE /var/lib/docker/btrfs/subvolumes/${FULL_CONTAINER_ID}/PATH/TO/FILE
```


# 高级配置 #

## 限制CPU ##
```sh
--cpu-shares=512   # CPU配额，默认最大值1024。简称 -c
--cpuset-cpus=0,1  # 指定使用的CPU核心编号，1.6及以下版本是--cpuset
```

## 限制内存 ##
限制内存后超出使用限制会OOMKilled进程
```sh
--memory=100m      # 限制内存(mem+swap) -m
--memory-swap=-1   # 限制swap+内存，关闭swap为-1
```

## 关闭OOMKilled ##
```sh
--oom-kill-disable=true
```

## 自动重启 ##
```sh
#   使用在 Docker run 的时候使用 --restart 参数来设置。
#   
#   no          - container不重启
#   on-failure  - container推出状态非0时重启
#   always      - 始终重启
#
# https://docs.docker.com/reference/commandline/cli/#restart-policies

--restart=always
```

## 网络模式 ##
```sh
--net=bridge、host、container、none
```

## 设置DNS ##
```sh
--dns=8.8.8.8
--dns=4.4.4.4
```



# 疑难问题 #

## 内存限制无效 ##
```sh
#   On systems using GRUB (which is the default for Ubuntu), 
#   you can add those parameters by editing /etc/default/grub 
#   and extending GRUB_CMDLINE_LINUX. Look for the following line:
#   
#   $ GRUB_CMDLINE_LINUX=""
#   And replace it by the following one:
#   
#   $ GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
#   Then run sudo update-grub, and reboot.
#   
#   These parameters will help you get rid of the following warnings:
#   
#   WARNING: Your kernel does not support cgroup swap limit.
#   WARNING: Your kernel does not support swap limit capabilities. Limitation discarded.


vim /etc/default/grub
# GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"
sudo update-grub
reboot
```
