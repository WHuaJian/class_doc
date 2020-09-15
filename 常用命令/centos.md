#   一、centos

# 1、JDK

## 1、检查当前系统中是否存在默认的jdk

```
rpm -qa | grep java
```



## 2、如果出现java-版本号的，可以删除

```
rpm -e --nodeps java-1.7.0-openjdk-headless-1.7.0.111-2.6.7.8.el7.x86_64
rpm -e --nodeps java-1.7.0-openjdk-1.7.0.111-2.6.7.8.el7.x86_64
```



## 3、搜索jdk安装包

```
yum search java|grep jdk
```



## 4、下载jdk

```
yum search java|grep jdk
```



## 5、解压

```bash
tar -xvfz  tar -xzvf /lib/java/jdk-8u261-linux-x64.tar.gz 
```

如果不指定解压路径，解压文件会放在/root/目录下



## 6、配置环境变量

```
vi /etc/profile 

# jdk 环境变量
export JAVA_HOME=/lib/java/jdk1.8.0_261
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin

source /etc/profile
```

## 7、检验

```
java -version

java version "1.8.0_261"
Java(TM) SE Runtime Environment (build 1.8.0_261-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.261-b12, mixed mode)

```





# 2、Docker

## 1、查看所有仓库中所有docker版本

```
yum list docker-ce --showduplicates | sort -r
```



## 2、下载docker0-ce的repo

```
curl https://download.docker.com/linux/centos/docker-ce.repo -o /etc/yum.repos.d/docker-ce.repo
```



## 3、安装依赖

```
yum install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.6-3.3.fc30.x86_64.rpm
```

centos8.0的关键步骤



## 4、安装docker-ce

```
yum install docker-ce
```



## 5、启动docker

```java
systemctl start docker
```



## 6、重启docker

```
systemctl restart docker
```



## 7、设置开机启动

```
systemctl enable docke
```



## 8、验证安装是否成功

(有client和service两部分表示docker安装启动都成功了)

```
docker version
```





# 3、iptable

## 1、 查看防火墙状态

```
service iptables status 
```

## 2、停止防火墙

```
service iptables stop 
```

## 3、 启动防火墙

```
service iptables start 
```

## 4、重启防火墙

```
service iptables restart 
```

## 5、永久关闭防火墙

```
chkconfig iptables off 
```

## 6、 永久关闭后重启

```
chkconfig iptables on　　
```

## 7、开启80端口

```
vim /etc/sysconfig/iptables
```

\# 加入如下代码

```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
```

## 8、保存退出后重启防火墙

```
service iptables restart
```



# 4、Firewall

## 1、查看firewall服务状态

```
systemctl status firewalld

出现Active: active (running)切高亮显示则表示是启动状态。
出现 Active: inactive (dead)灰色表示停止，看单词也行。
```



## 2、查看firewall的状态

```
firewall-cmd --state
```



## 3、开启、重启、关闭、firewalld.service服务

```
# 开启
service firewalld start

# 重启
service firewalld restart

# 关闭
service firewalld stop
```



## 4、查看防火墙规则

```
firewall-cmd --list-all
```



## 5、查询、开放、关闭端口

```
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp

# 临时开放80端口
firewall-cmd --zone=public --add-port=8080/tcp
# 永久开放80端口
firewall-cmd --permanent --zone=public --add-port=80/tcp

# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp

#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload
```



## 6、参数解释

```
1、firwall-cmd：是Linux提供的操作firewall的一个工具；
2、--permanent：表示设置为持久；
3、--add-port：标识添加的端口；
```



# 5、MySQL

## 1、查询MySQL版本

```
docker search mysql
```



## 2、拉取指定版本镜像

```
docker pull NAME
```



## 3、启动MySQL

```
docker run -p 3306:3306 --name mysql01 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.5
```



## 4、设置开机启动

```
docker run --restart=always --name mysql01 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.5
```



## 5、修改配置文件

```
docker run -d -e MYSQL_ROOT_PASSWORD='password' -v $HOME/my:/etc/mysql/conf.d/ --name mysql mysql57:latest
---------------------
$HOME/my这个文件夹是存放my.cnf配置文件
```



# 6、部署

## 1、jar方式运行

```
nohup java -jar k12_classroom-0.0.1-SNAPSHOT.jar --server.port=9001 &
```





# 7、IDEA配置Docker插件

## 1、修改docker.service,开启2375端口

```
vim /usr/lib/systemd/system/docker.service
```



## 2、在ExecStart末尾增加 ：按"i"插入，按"Esc"退出，输入":wq" 完成文件写入

```
# 允许所有客户端连接
-H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
```

![image-20200915142014343](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200915142014343.png)



## 3、重新加载配置文件和重启Docker

```
systemctl daemon-reload 
systemctl restart docker 
```



## 4、查看进程

```
netstat -tulp
```

![image-20200915141919627](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200915141919627.png)

```
netstat -nplt |grep 2375
```

![image-20200915141929116](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200915141929116.png)



## 5、防火墙开放2375端口/重启防火墙

```
firewall-cmd --zone=public --add-port=2375/tcp --permanent
firewall-cmd --reload
```

<!--*注意：虚拟机或与服务器的安全组也要开放2375端口，否者依然无法访问！*-->



## 6、IDEA安装docker插件

![image-20200915142625759](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200915142625759.png)



## 7、配置连接ip和port

![image-20200915142824418](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200915142824418.png)

