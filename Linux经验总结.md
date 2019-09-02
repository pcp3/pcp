 

**Linux常用操作**


# 1 硬盘分区及格式化

说明：扩容需求，新添加了一块硬盘sdb。需将sdb分成一个区，然后格式化。

## 1.1 硬盘分区

- 查看已有硬盘

  ```
  fdisk –l
  ```

- 查看已有分区

  ```
  df –l
  ```

- 添加分区

  ```
  fdisk /dev/sdb
  ```


  ```
   1. 回车
   2. P 回车
   3. 1 回车
   4. 两次回车
   5. W 回车
  ```

- 查看分区

  ```
  fdisk –l
  ```

## 1.2   分区格式化

- 执行分区格式化命令

  ```
  mkfs -t ext4 -c /dev/sdb
  ```

  [^参数说明]: t 指定要把磁盘格式化成什么类型；-c 在建立文件系统之前检查坏道，可能会很费时间，新硬盘一般不需要。


## 1.3   挂载到目录

- 新建文件夹

  ```
  mkdir /opt
  ```

- 执行挂载

  ```
  mount /dev/sdb /opt
  ```

- 开机自动挂载

  ```
  echo "/dev/sdb /opt ext4 defaults 0 0">>/etc/fstaba
  ```

# 2 systemctl命令介绍

  systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。

## 2.1 启停命令

  以firewalld.service为例

- 启动服务

  ```
  systemctl start firewalld.service
  ```

- 关闭服务

  ```
  systemctl stop firewalld.service
  ```

- 重启服务

  ```
  systemctl restart firewalld.service
  ```

- 显示服务状态

  ```
  systemctl status firewalld.service
  ```

## 2.2 开机设置

- 在开机时启用一个服务

  ```
  systemctl enable firewalld.service
  ```

- 在开机时禁用一个服务

  ```
  systemctl disable firewalld.service
  ```

- 查看服务是否开机启动

  ```
  systemctl is-enabled firewalld.service
  ```

## 3.3 查看命令

- 查看开机启动的服务列表

  ```
  systemctl list-unit-files|grep enabled
  ```

- 查看失败的服务列表

  ```
  systemctl --failed
  ```

# 3 yum缓存离线rpm包

## 3.1 连通互联网

```
	vi /etc/resolv.conf 
```

[^增加]: nameserver 114.114.114.114

## 3.2 配置163的源

```
	vi /etc/yum.repos.d/CentOS7-Base-163.repo
```

   [*CentOS7-Base-163.repo下载地址*](http://mirrors.163.com/.help/CentOS7-Base-163.repo)

## 3.3 设置Yum缓存

```
vi /etc/yum.conf
```

 修改：keepcache=1

<!--开启缓存后，用yum install安装的软件包会在/var/cache/yum中保存-->

[参考网址](http://jingyan.baidu.com/album/d2b1d102b8b0825c7f37d46b.html?picindex=11)



# 4 免密钥登录

只有两步，特别简单

- 生成公钥与私钥对，一路回车执行

  ```
  ssh-keygen
  ```

- 将公钥复制到远程机器（本机也要拷贝一份）

  ```
  ssh-copy-id -i .ssh/id_rsa.pub  root@10.10.10.10
  ```

# 5 后台进程监控工具

progress-master是一款监控后台进程的工具。非常有助于实时监控后台执行的大文件的拷贝速度、大文件下载速度。

## 5.1 安装工具

- 下载

  ```
  wget https://github.com/Xfennec/progress/archive/master.zip
  ```

- 解压

  ```
  unzip unzip progress-master.zip
  ```

- 安装依赖包

  ```
  yum install -y ncurses-devel
  yum install -y gcc       
  ```

- 执行安装

  ```
  make && make install
  ```

## 5.2 常用命令

- 监控后台进程	

  ```
  watch progress -q
  ```

- 查看Web服务的进程 

  ```
  watch progress -c httpd
  ```

[官网网址]: (https://github.com/Xfennec/progress



# 6 根据内容查找文件

- 根据内容查找文件：

  ```
  find /opt/kubespray/kubespray/ | xargs grep -ri 'centos'
  ```

# 7 安装Python3环境（与python2并存）

## 7.1 安装Python3

- 创建安装目录

  ```
  mkdir -p /opt/python/ && cd /opt/python/
  ```

- 从官网下载最新稳定版python安装包（略过）

  [<u>python官网下载地址</u>](https://www.python.org/downloads/)

- 解压，进入目录

  ```
  tar -xvf Python-3.6.2.tar.xz
  ```

- 安装python依赖

- ```
  yum install -y openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel gcc
  ```

- cd Python-3.6.2

  ```
  mkdir -p /usr/local/python3
  
  ./configure --prefix=/usr/local/python3
  
  cd /opt/python/Python-3.6.2
  
  make && make install
  ```

## 7.2 设置软连接

- 移除python2软连接

  ```
  cd /usr/bin
  rm -rf python
  ```

- 建立python3的软连接

  ```
  ln -s /usr/local/python3/bin/python3.6 /usr/bin/python
  ln -s /usr/local/python3/bin/pip3 /usr/bin/pip
  ```

## 7.3 设置yum	

```
vi /usr/bin/yum
```
> 第一行改为：/usr/bin/python2.7


```
 vi /usr/libexec/urlgrabber-ext-down
```
> 第一行改为：/usr/bin/python2.7


## 7.4 测试

```
python 
```

> Python 3.6.2 (default, Mar  9 2018, 20:59:55)
>
> ​       [GCC 4.8.5 20150623 (Red Hat 4.8.5-16)] on linux
>
> ​       Type "help", "copyright", "credits" or "license" for more information.
>
> ​       \>>>
>
