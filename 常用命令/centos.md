

# 一、JDK

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





# 二、Docker

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





# 三、iptable

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



# 四、Firewall

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



# 五、MySQL

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



# 六、部署

## 1、jar方式运行

```
nohup java -jar k12_classroom-0.0.1-SNAPSHOT.jar --server.port=9001 &
```





# 七、IDEA配置Docker插件

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



# 八、Jenkins

## 一、Docker安装Jenkins

### 1、拉取镜像

```
 docker pull jenkins/jenkins:lts
```



### 2、创建目录

方便后期Jenkins中配置文件的修改，同时，防止jenkins中重要文件因为容器损毁或删除导致文件丢失，因此需要将/var/jenkins_home目录对外挂载

```
mkdir -p /var/jenkins

chmod 777 /var/jenkins
```



### 3、启动容器

```
docker run -itd -p 9003:8080 -p 9004:50000  --restart always -v /var/jenkins:/var/jenkins_home 
-v /lib/java/jdk1.8.0_261/bin/java:/lib/java/jdk1.8.0_261/bin/java --name jenkins  jenkins/jenkins:lts

docker run -itd -p 9003:8080 -p 9004:50000 --restart always \
-v /lib/java/jdk1.8.0_261/bin/java:/lib/java/jdk1.8.0_261/bin/java \
-v /lib/java/jdk1.8.0_261:/lib/java/jdk1.8.0_261 \ 
-v /var/jenkins:/var/jenkins_home \
-v /lib/maven3.6.3:/lib/maven3.6.3 \
-v /usr/bin/git:/usr/bin/git \
--name jenkins  jenkins/jenkins:lts
```

**-d 后台运行镜像**

**-p 9003:8080 将镜像的8080端口映射到服务器的9003端口。**

**-p 9004:50000 将镜像的50000端口映射到服务器的9004端口**

**-v /var/jenkins_\**mount\**:/var/jenkins_mount /var/jenkins_home目录为容器jenkins工作目录，我们将硬盘上的一个目录挂载到这个位置，方便后续更新镜像后继续使用原来的工作目录。这里我们设置的就是上面我们创建的 /var/jenkins_mount目录**

**-v /etc/localtime:/etc/localtime让容器使用和服务器同样的时间设置。**

**--name jenkins 给容器起一个别名**



### 二、yum安装Jenkins

### 1、下载依赖

```
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```



### 2、导入秘钥

```
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```



### 3、安装

```
yum install jenkins
```



### 4、查看安装目录

```
rpm -ql jenkins

jenkins相关目录释义：
1. /usr/lib/jenkins/：jenkins安装目录，war包会放在这里。
2.  /etc/sysconfig/jenkins：jenkins配置文件，“端口”，“JENKINS_HOME”等都可以在这里配置。
3. /var/lib/jenkins/：默认的JENKINS_HOME。
4. /var/log/jenkins/jenkins.log：jenkins日志文件。
```

![image-20200921092557391](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200921092557391.png)



### 5、修改默认端口（默认是8080）

```
vim /etc/sysconfig/jenkins

#修改为9002，记住要打开防火墙端口 
JENKINS_PORT="9002"

firewall-cmd --zone=public --add-port=9002/tcp --permanent
firewall-cmd --reload
```



### 6、设置开机启动

```
chkconfig jenkins on
```



### 7、启动Jenkins

```
service jenkins start
```

如果启动报错如下则说明你的jdk路径不对，需要配置：

```
#查看错误日志
journalctl -xe
```

![image-20200921093222796](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200921093222796.png)

解决方法：修改Jenkins启动配置文件，指定jdk路径（which java命令查看）

```
vim /etc/init.d/jenkins
```

找到candidates，增加java路径，如下：

```
candidates="
/lib/java/jdk1.8.0_261/bin/java
/etc/alternatives/java
/usr/lib/jvm/java-1.8.0/bin/java
/usr/lib/jvm/jre-1.8.0/bin/java
/usr/lib/jvm/java-1.7.0/bin/java
/usr/lib/jvm/jre-1.7.0/bin/java
/usr/lib/jvm/java-11.0/bin/java
/usr/lib/jvm/jre-11.0/bin/java
/usr/lib/jvm/java-11-openjdk-amd64
/usr/bin/java
```

再次启动：

```
systemctl start jenkins
```

如果有报这个warning，执行一个命令即可去除:

Warning: The unit file, source configuration file or drop-ins of jenkins.service changed on disk. Run 'systemctl daemon-reload' to reload units.

```
systemctl daemon-reload
```

### 8、查看运行状态

```
systemctl status jenkins
```

![image-20200921093854635](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200921093854635.png)

初始密码在/var/lib/jenkins/secrets/initialAdminPassword

![image-20200921093933158](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200921093933158.png)



# 九、Maven

## 1、下载

```
wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
```



## 2、指定文件目录安装

```
mkdir -p /lib/maven3.6.3
```



## 3、解压

```
tar -zxvf apache-maven-3.6.3-bin.tar.gz 
```



## 4、copy到自定目录

```
[root@dev-server apache-maven-3.6.3]# cp  -r  * /lib/maven3.6.3
```



## 5、配置环境变量

```
vim /etc/profile

#maven
export MAVEN_HOME=/lib/maven3.6.3
export PATH=$PATH:$JAVA_HOME/bin:$MAVEN_HOME/bin

source /etc/profile
```



# 十、Gitlab配置SSH key

## 1、生成ssh公钥和私钥

```
ssh-keygen -t rsa -C '727356424@qq.com'
```

然后一路回车



## 2、复制~/.ssh/id_rsa.pub内容

```
 cat ~/.ssh/id_rsa.pub
```

![image-20200918160558334](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200918160558334.png)



## 3、在gitlab上添加ssh key

![image-20200918160659095](/Users/weihuajian/Library/Application Support/typora-user-images/image-20200918160659095.png)