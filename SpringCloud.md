## SpringCloud

## 一、Eureka：服务的注册与发现

### 1、创建EurekaServer

#### 1.1 创建一个父工程，并且在父工程中指定SpringCloud的版本，并且将packaing修改为pom

```
 <groupId>com.whj</groupId>
    <artifactId>springcloud</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>springcloud</name>
    <description>Demo project for Spring Boot</description>
    <packaging>pom</packaging>

    <properties>
        <java.version>1.8</java.version>
        <spring.cloud-version>Hoxton.SR8</spring.cloud-version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring.cloud-version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

```

#### 1.2 创建eureka的server，创建springboot工程导入依赖，并在启动类中添加注解，编写yml文件

##### 1.2.1 导入依赖

```
<dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

##### 1.2.2 编写yml

```
server:
  port: 8080

eureka:
  instance:
    hostname: localhost
  client:
    registerWithEureka: false
    fetchRegistry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
```

##### 1.2.3 添加注解

```
@SpringBootApplication
@EnableEurekaServer
public class EurekaApplication {

    public static void main(String[] args){
        SpringApplication.run(EurekaApplication.class,args);
    }
}
```



### 2、创建EurekaClient

#### 2.1 创建maven工程，修改为springboot

#### 2.2 导入依赖

```
<dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

#### 2.3 在启动类上添加注解

```
@SpringBootApplication
@EnableEurekaClient
public class CustomerApplication {

    public static void main(String[] args){
        SpringApplication.run(CustomerApplication.class,args);
    }
}
```

#### 2.4 编写配置文件

```
#指定Eureka服务地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8080/eureka/
spring:
  application:
    name: customer
#指定服务的名称
```



## 二、Robbin：服务之间的负载均衡

## 三、Feign：服务之间的通讯

## 四、Hystrix：服务的线程隔离以及断路器

## 五、Zuul：服务网关

## 六、Stream：实现MQ的使用

## 七、Config：动态配置

## 八、Sleuth：服务追踪

