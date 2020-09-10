# Spring Boot 

## [简介](https://note.clboy.cn/#/backend/springboot/helloworld?id=简介)

[百度百科](http://t.cn/AirXJZuO)

**优点**

> - 快速创建独立运行的Spring项目以及与主流框架集成
> - 使用嵌入式的Servlet容器，应用无需打成WAR包
> - starters自动依赖与版本控制
> - 大量的自动配置，简化开发，也可修改默认值
> - 无需配置XML，无代码生成，开箱即用
> - 准生产环境的运行时应用监控
> - 与云计算的天然集成

## [微服务](https://note.clboy.cn/#/backend/springboot/helloworld?id=微服务)

> 2014，martin fowler
>
> 微服务：架构风格（服务微化）
>
> 一个应用应该是一组小型服务；可以通过HTTP的方式进行互通；
>
> 单体应用：ALL IN ONE
>
> 微服务：每一个功能元素最终都是一个可独立替换和独立升级的软件单元；
>
> [详细参照微服务文档](https://martinfowler.com/articles/microservices.html#MicroservicesAndSoa)
>
> [集群、分布式、微服务概念和区别](https://blog.csdn.net/qq_37788067/article/details/79250623)

## [环境约束](https://note.clboy.cn/#/backend/springboot/helloworld?id=环境约束)

–jdk1.8：Spring Boot 推荐jdk1.7及以上；

–maven3.x：maven 3.3以上版本；

## [Maven设置](https://note.clboy.cn/#/backend/springboot/helloworld?id=maven设置)

给maven 的settings.xml配置文件的profiles标签添加：（设置使用的jdk版本）

开发工具中的maven设置为自己配置的maven

```xml
<profile>
  <id>jdk-1.8</id>
  <activation>
    <activeByDefault>true</activeByDefault>
    <jdk>1.8</jdk>
  </activation>
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
  </properties>
</profile>Copy to clipboardErrorCopied
```

## [创建一个maven工程](https://note.clboy.cn/#/backend/springboot/helloworld?id=创建一个maven工程)

1. 导入spring boot相关的依赖

   ```xml
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.2.1.RELEASE</version>
           <relativePath/>
       </parent>
   
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
       </dependencies>Copy to clipboardErrorCopied
   ```

2. 编写一个主程序；启动Spring Boot应用

   ```java
   package cn.clboy.springboot;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   /**
    * @Author cloudlandboy
    * @Date 2019/11/13 下午2:58
    * @Since 1.0.0
    * springBootApplication：标注一个主程序类，表示这个是一个Springboot应用
    */
   
   @SpringBootApplication
   public class HelloWorldMainApplication {
   
       public static void main(String[] args) {
           //启动
           SpringApplication.run(HelloWorldMainApplication.class, args);
       }
   }Copy to clipboardErrorCopied
   ```

1. 编写一个Controller

   ```java
   package cn.clboy.springboot.controller;
   
   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   /**
    * @Author cloudlandboy
    * @Date 2019/11/13 下午3:05
    * @Since 1.0.0
    * RestController：是spring4里的新注解，是@ResponseBody和@Controller的缩写。
    */
   
   @RestController
   public class HelloController {
   
       @RequestMapping("/hello")
       public String hello(){
           return "hello SpringBoot,this is my first Application";
       }
   }Copy to clipboardErrorCopied
   ```

2. 运行主程序Main方法测试

3. 访问 [localhost:8080/hello](http://localhost:8080/hello)

## [简化部署](https://note.clboy.cn/#/backend/springboot/helloworld?id=简化部署)

1. 添加maven插件

   ```xml
    <!-- 这个插件，可以将应用打包成一个可执行的jar包；-->
       <build>
           <plugins>
               <plugin>
                   <groupId>org.springframework.boot</groupId>
                   <artifactId>spring-boot-maven-plugin</artifactId>
               </plugin>
           </plugins>
       </build>Copy to clipboardErrorCopied
   ```

2. 使用mvn package进行打包

3. 进入打包好的jar包所在目录

4. 使用 `java -jar jar包名称` 运行

## [Hello World探究](https://note.clboy.cn/#/backend/springboot/helloworld?id=hello-world探究)

### [依赖](https://note.clboy.cn/#/backend/springboot/helloworld?id=依赖)

```
    <!--Hello World项目的父工程是org.springframework.boot-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.1.RELEASE</version>
        <relativePath/>
    </parent>

    <!--
        org.springframework.boot他的父项目是spring-boot-dependencies
        他来真正管理Spring Boot应用里面的所有依赖版本；
        Spring Boot的版本仲裁中心；
        以后我们导入依赖默认是不需要写版本；（没有在dependencies里面管理的依赖自然需要声明版本号）
    -->
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.2.1.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
  </parent>Copy to clipboardErrorCopied
```

### [启动器](https://note.clboy.cn/#/backend/springboot/helloworld?id=启动器)

```xml
       <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>Copy to clipboardErrorCopied
```

**spring-boot-starter**-`web`：

 spring-boot-starter：spring-boot场景启动器；帮我们导入了web模块正常运行所依赖的组件；

Spring Boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来。要用什么功能就导入什么场景的启动器

### [主程序类，主入口类](https://note.clboy.cn/#/backend/springboot/helloworld?id=主程序类，主入口类)

```java
@SpringBootApplication
public class HelloWorldMainApplication {

    public static void main(String[] args) {
        //启动
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }
}Copy to clipboardErrorCopied
```

`@SpringBootApplication`: Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

看一下`@SpringBootApplication`这个注解类的源码

```java
@Target({ElementType.TYPE})    //可以给一个类型进行注解，比如类、接口、枚举
@Retention(RetentionPolicy.RUNTIME)    //可以保留到程序运行的时候，它会被加载进入到 JVM 中
@Documented    //将注解中的元素包含到 Javadoc 中去。
@Inherited    //继承，比如A类上有该注解，B类继承A类，B类就也拥有该注解

@SpringBootConfiguration

@EnableAutoConfiguration

/*
*创建一个配置类，在配置类上添加 @ComponentScan 注解。
*该注解默认会扫描该类所在的包下所有的配置类，相当于之前的 <context:component-scan>。
*/
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplicationCopy to clipboardErrorCopied
```

- `@SpringBootConfiguration`：Spring Boot的配置类；标注在某个类上，表示这是一个Spring Boot的配置类；

  ```java
  @Target({ElementType.TYPE})
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @Configuration
  public @interface SpringBootConfigurationCopy to clipboardErrorCopied
  ```

  - `@Configuration`：配置类上来标注这个注解；

    配置类 ----- 配置文件；配置类也是容器中的一个组件；@Component

    ```java
    @Target({ElementType.TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    @Component
    public @interface Configuration Copy to clipboardErrorCopied
    ```

- `@EnableAutoConfiguration`：开启自动配置功能；

  以前我们需要配置的东西，Spring Boot帮我们自动配置；@**EnableAutoConfiguration**告诉SpringBoot开启自动配置功能；这样自动配置才能生效；

  ```
  @Target({ElementType.TYPE})
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @Inherited
  @AutoConfigurationPackage
  @Import({AutoConfigurationImportSelector.class})
  public @interface EnableAutoConfigurationCopy to clipboardErrorCopied
  ```

  - `@AutoConfigurationPackage`：自动配置包

    ```java
    @Target({ElementType.TYPE})
    @Retention(RetentionPolicy.RUNTIME)
    @Documented
    @Inherited
    @Import({Registrar.class})
    public @interface AutoConfigurationPackageCopy to clipboardErrorCopied
    ```

    - `@Import`：Spring的底层注解@Import，给容器中导入一个组件

      导入的组件由`org.springframework.boot.autoconfigure.AutoConfigurationPackages.Registrar`将主配置类（@SpringBootApplication标注的类）的所在包及下面所有子包里面的所有组件扫描到Spring容器；

      ![DEBUG](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573637120233.png)

      这里controller包是在主程序所在的包下，所以会被扫描到，我们在springboot包下创建一个test包，把主程序放在test包下，这样启动就只会去扫描test包下的内容而controller包就不会被扫描到，再访问开始的hello就是404

      ![DEBUG](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573637728857.png)

  - `@Import({AutoConfigurationImportSelector.class})`

    `AutoConfigurationImportSelector.class`将所有需要导入的组件以全类名的方式返回；这些组件就会被添加到容器中；会给容器中导入非常多的自动配置类（xxxAutoConfiguration）；就是给容器中导入这个场景需要的所有组件，并配置好这些组件；

    有了自动配置类，免去了我们手动编写配置注入功能组件等的工作；

    ![Configuration](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573638685562.png)

Spring Boot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值，将这些值作为自动配置类导入到容器中，自动配置类就生效，帮我们进行自动配置工作；以前我们需要自己配置的东西，自动配置类都帮我们完成了；

# [使用Spring Initializer快速创建Spring Boot项目](https://note.clboy.cn/#/backend/springboot/springInitializer?id=使用spring-initializer快速创建spring-boot项目)

IDE都支持使用Spring的项目创建向导快速创建一个Spring Boot项目；

选择我们需要的模块；向导会联网创建Spring Boot项目；

需要联网

## [IDEA](https://note.clboy.cn/#/backend/springboot/springInitializer?id=idea)

1. 创建项目时选择Spring Initializr

   ![idea01](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573639771931.png)

1. 完善项目信息

   ![idea02](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573639923504.png)

   出现 **Artifact contains illegal characters** 是因为`Artifact`中使用了大写，只能是全小写，单词之间用`-`分隔

2. 选择需要的starter

   ![idea03](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573640773365.png)

3. 创建完成后 不要的文件可以删除

   ![idea04](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573641023432.png)

## [Eclipse](https://note.clboy.cn/#/backend/springboot/springInitializer?id=eclipse)

1. 需要安装插件，或者使用STS版本

2. 创建项目时选择Spring Starter Project

   ![eclipse01](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573643167357.png)

3. 完善信息

   ![eclipse02](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573643396117.png)

4. 选择需要的选择需要的starter

   ![eclipse03](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573643505863.png)

**默认生成的Spring Boot项目**

- 主程序已经生成好了，我们只需要完成我们自己的逻辑

- ```
  resources
  ```

  文件夹中目录结构

  - `static`：保存所有的静态资源； js、css、images；
  - `templates`：保存所有的模板页面；（Spring Boot默认jar包使用嵌入式的Tomcat，`默认`不支持JSP页面）；可以使用模板引擎（freemarker、thymeleaf）；
  - `application.properties`：Spring Boot应用的配置文件；可以修改一些默认设置；





# [配置文件](https://note.clboy.cn/#/backend/springboot/properties?id=配置文件)

SpringBoot使用一个全局的配置文件，配置文件名`application`是固定的；

- application.properties
- application.yml
- application.yaml

配置文件的作用：修改SpringBoot自动配置的默认值；SpringBoot在底层都给我们自动配置好；

## [YAML](https://note.clboy.cn/#/backend/springboot/properties?id=yaml)

YAML（YAML Ain't Markup Language）

 YAML A Markup Language：是一个标记语言

 YAML isn't Markup Language：不是一个标记语言；

标记语言：

 以前的配置文件；大多都使用的是 **xxxx.xml**文件；

 YAML：**以数据为中心**，比json、xml等更适合做配置文件；

### [YAML语法：](https://note.clboy.cn/#/backend/springboot/properties?id=yaml语法：)

以`空格`的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的

次等级的前面是空格，不能使用制表符(tab)

冒号之后如果有值，那么冒号和值之间至少有一个空格，不能紧贴着

### [字面量：普通的值（数字，字符串，布尔）](https://note.clboy.cn/#/backend/springboot/properties?id=字面量：普通的值（数字，字符串，布尔）)

```
k: v
```

字符串默认不用加上单引号或者双引号；

`""`：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

*eg：* name: "zhangsan \n lisi"：输出；zhangsan 换行 lisi

`''`：单引号；会转义特殊字符，特殊字符最终只是一个普通的字符串数据

*eg：* name: ‘zhangsan \n lisi’：输出；zhangsan \n lisi

### [对象、Map（属性和值）：](https://note.clboy.cn/#/backend/springboot/properties?id=对象、map（属性和值）：)

`k: v`在下一行来写对象的属性和值的关系；注意缩进

1. ```yaml
   person:
     name: 张三
     gender: 男
     age: 22Copy to clipboardErrorCopied
   ```

2. 行内写法

   ```yaml
   person: {name: 张三,gender: 男,age: 22}Copy to clipboardErrorCopied
   ```

### [数组（List、Set）](https://note.clboy.cn/#/backend/springboot/properties?id=数组（list、set）)

1. ```
   fruits: 
     - 苹果
     - 桃子
     - 香蕉Copy to clipboardErrorCopied
   ```

2. 行内写法

   ```
   fruits: [苹果,桃子,香蕉]Copy to clipboardErrorCopied
   ```

## [配置文件值注入](https://note.clboy.cn/#/backend/springboot/properties?id=配置文件值注入)

<details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box; font-size: 15px; color: rgb(52, 73, 94); font-family: &quot;Source Sans Pro&quot;, &quot;Helvetica Neue&quot;, Arial, sans-serif; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-style: initial; text-decoration-color: initial;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box; font-weight: bold; color: green;">JavaBean：</summary></details>

```xml
        <!--导入配置文件处理器，配置文件进行绑定就会有提示-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>Copy to clipboardErrorCopied
```

**配置文件：**

```yaml
person:
  name: 张三
  gender: 男
  age: 36
  boss: true
  birth: 1982/10/1
  maps: {k1: v1,k2: v2}
  lists:
    - apple
    - peach
    - banana
  pet:
    name: 小狗
    age: 12
Copy to clipboardErrorCopied
```

**测试**

```java
package cn.clboy.helloworldquickstart;

import cn.clboy.helloworldquickstart.model.Person;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class HelloworldquickstartApplicationTests {

    @Autowired
    private Person person;

    @Test
    void contextLoads() {
        System.out.println(person);
    }

}
Copy to clipboardErrorCopied
```

## [properties](https://note.clboy.cn/#/backend/springboot/properties?id=properties)

上面yaml对应的properties配置文件写法

```properties
person.name=李四
person.age=34
person.birth=1986/09/12
person.boss=true
person.gender=女
person.lists=cat,dog,pig
person.maps.k1=v1
person.maps.k2=v2
person.pet.name="小黑"
person.pet.age=10Copy to clipboardErrorCopied
```

测试，发现中文会乱码，而且char类型还会抛出Failed to bind properties under 'person.gender' to java.lang.Character异常

### [中文乱码解决方法：](https://note.clboy.cn/#/backend/springboot/properties?id=中文乱码解决方法：)

在设置中找到`File Encodings`，将配置文件字符集改为`UTF-8`，并勾选：

-  `Transparent native-to-ascii conversion`

![乱码解决](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573695616496.png)

yaml和properties配置文件同时存在，properties配置文件的内容会覆盖yaml配置文件的内容

## [配置文件值注入两种方式对比](https://note.clboy.cn/#/backend/springboot/properties?id=配置文件值注入两种方式对比)

配置文件值注入有两种方式，一个是Spring Boot的`@ConfigurationProperties`注解，另一个是spring原先的`@value`注解

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

**松散绑定**：例如Person中有`lastName`属性，在配置文件中可以写成

`lastName`或`lastname`或`last-name`或`last_name`等等

**SpEL**：

```
##　properties配置文件
persion.age=#{2019-1986+1}

# Person类
#--------------------使用@ConfigurationProperties注解，会抛出异常--------------------
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private Integer age;


#--------------------使用@value注解 OK--------------------
@Component
public class Person {
    @Value("${person.age}")
    private Integer age;Copy to clipboardErrorCopied
```

**JSR303数据校验**

`@ConfigurationProperties`支持校验，如果校验不通过，会抛出异常

![数据校验](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573716216690.png)

`@value`注解不支持数据校验

![数据校验](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573716427494.png)

**复杂类型封装**

```
@value`注解无法注入map等对象的复杂类型，但`list、数组可以
```

![1573716770263](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573716770263.png)

## [@PropertySource](https://note.clboy.cn/#/backend/springboot/properties?id=propertysource)

`@PropertySource`注解的作用是加载指定的配置文件，值可以是数组，也就是可以加载多个配置文件

springboot默认加载的配置文件名是`application`，如果配置文件名不是这个是不会被容器加载的，所以这里Person并没有被注入任何属性值

![1573718577827](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573718577827.png)

使用`@PropertySource({"classpath:person.properties"})`指定加载`person.properties`配置文件

![1573718679208](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573718679208.png)

使用这个注解加载配置文件就需要配置类使用@component等注解而不是等待@EnableConfigurationProperties激活m，而且不支持yaml，只能是properties

## [@ImportResource](https://note.clboy.cn/#/backend/springboot/properties?id=importresource)

`@ImportResource`注解用于导入Spring的配置文件，让配置文件里面的内容生效；(就是以前写的springmvc.xml、applicationContext.xml)

Spring Boot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别；

![1573719440710](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573719440710.png)

想让Spring的配置文件生效，加载进来；@**ImportResource**标注在一个配置类上

![1573720006428](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573720006428.png)

注意！这个注解是放在主入口函数的类上，而不是测试类上

## [@Configuration](https://note.clboy.cn/#/backend/springboot/properties?id=configuration)

SpringBoot推荐给容器中添加组件的方式；推荐使用全注解的方式

配置类**@Configuration** ---equals---> Spring配置文件

### [@Bean](https://note.clboy.cn/#/backend/springboot/properties?id=bean)

使用**@Bean**给容器中添加组件

```java
package cn.clboy.helloworldquickstart.config;

import cn.clboy.helloworldquickstart.model.Pet;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Author cloudlandboy
 * @Date 2019/11/14 下午4:33
 * @Since 1.0.0
 *
 * Configuration：指明当前类是一个配置类；就是来替代之前的Spring配置文件
 */


@Configuration
public class BeanConfiguration {

    /**
     *相当于在配置文件中用<bean><bean/>标签添加组件
     */
    @Bean
    public Pet myPet() {
        Pet pet = new Pet();
        pet.setName("嘟嘟");
        pet.setAge(3);
        return pet;
    }
}Copy to clipboardErrorCopied
```

## [配置文件占位符](https://note.clboy.cn/#/backend/springboot/properties?id=配置文件占位符)

**随机**

```
${random.value}
${random.int}
${random.long}
${random.int(10)}
${random.int[1024,65536]}Copy to clipboardErrorCopied
```

![1573721695426](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573721695426.png)

可以引用在配置文件中配置的其他属性的值，如果使用一个没有在配置文件中的属性，则会原样输出

![1573722018302](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573722018302.png)

可以使用`:`指定默认值

![1573722098119](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573722098119.png)

## [Profile](https://note.clboy.cn/#/backend/springboot/properties?id=profile)

Profile是Spring对不同环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境

### [多profile文件形式](https://note.clboy.cn/#/backend/springboot/properties?id=多profile文件形式)

文件名格式：application-{profile}.properties/yml，例如：

- application-dev.properties
- application-prod.properties

![1573723830627](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573723830627.png)

程序启动时会默认加载`application.properties`，启动的端口就是8080

可以在主配置文件中指定激活哪个配置文件

![1573724084979](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573724084979.png)

### [yml支持多文档块方式](https://note.clboy.cn/#/backend/springboot/properties?id=yml支持多文档块方式)

每个文档块使用`---`分割

```yaml
server:
  port: 8080
spring:
  profiles:
    active: prod
---
server:
  port: 8081
spring:
  profiles: dev
---
server:
  port: 8082
spring:
  profiles: prodCopy to clipboardErrorCopied
```

![1573724588671](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573724588671.png)

### [激活指定profile的三种方式](https://note.clboy.cn/#/backend/springboot/properties?id=激活指定profile的三种方式)

1. 在配置文件中指定 spring.profiles.active=dev（如上）

2. 项目打包后在命令行启动

   ```shell
   java -jar xxx.jar --spring.profiles.active=dev；Copy to clipboardErrorCopied
   ```

   ![1573724952868](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573724952868.png)

3. 虚拟机参数

   ```
   -Dspring.profiles.active=devCopy to clipboardErrorCopied
   ```

   ![1573725631649](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573725631649.png)

## [配置文件加载位置](https://note.clboy.cn/#/backend/springboot/properties?id=配置文件加载位置)

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件

```
file: ./config/

    file: ./

        classpath: /config/

            classpath: /        -->first load ↑
```

优先级由高到底，高优先级的配置会覆盖低优先级的配置（优先级低的先加载）；

SpringBoot会从这四个位置全部加载主配置文件；**互补配置**；

![1573728449451](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573728449451.png)

这里项目根路径下的配置文件maven编译时不会打包过去，需要修改pom

```xml
        <resources>
            <resource>
                <directory>.</directory>
                <filtering>true</filtering>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.yaml</include>
                </includes>
            </resource>
        </resources>Copy to clipboardErrorCopied
```

> 我们还可以通过`spring.config.location`来改变默认的配置文件位置
>
> **项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置；**
>
> ```
> java -jar xxx.jar --spring.config.location=/home/cloudlandboy/application.yamlCopy to clipboardErrorCopied
> ```

## [外部配置加载顺序](https://note.clboy.cn/#/backend/springboot/properties?id=外部配置加载顺序)

**SpringBoot也可以从以下位置加载配置； 优先级从高到低；高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置**

1. **命令行参数** ![point_up_2](https://github.githubassets.com/images/icons/emoji/unicode/1f446.png)

   所有的配置都可以在命令行上进行指定

   ```
   java -jar xxx.jar --server.port=8087  --server.context-path=/abcCopy to clipboardErrorCopied
   ```

   多个配置用空格分开； --配置项=值

2. 来自java:comp/env的JNDI属性 ⤴️

3. Java系统属性（System.getProperties()） ⤴️

4. 操作系统环境变量 ⤴️

5. RandomValuePropertySource配置的random.*属性值 ⤴️

**由jar包外向jar包内进行寻找；**

**再来加载不带profile**

1. **jar包外部的`application.properties`或`application.yml`(不带spring.profile)配置文件** ⤴️
2. **jar包内部的`application.properties`或`application.yml`(不带spring.profile)配置文件 ⤴️

**优先加载带profile**

1. **jar包外部的`application-{profile}.properties`或`application.yml`(带spring.profile)配置文件 ⤴️
2. **jar包内部的`application-{profile}.properties`或`application.yml`(带spring.profile)配置文件 ⤴️

1. @Configuration注解类上的@PropertySource ⤴️
2. 通过SpringApplication.setDefaultProperties指定的默认属性 ⤴️

所有支持的配置加载来源：

[参考官方文档](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/htmlsingle/#boot-features-external-config)

![1573735371567](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573735371567.png)



# [自动配置原理](https://note.clboy.cn/#/backend/springboot/autoconfig?id=自动配置原理)

配置文件到底能写什么？怎么写？自动配置原理；

[配置文件能配置的属性参照](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/htmlsingle/#common-application-properties)

SpringBoot启动的时候加载主配置类，开启了自动配置功能

```
@SpringBootApplication
    @EnableAutoConfigurationCopy to clipboardErrorCopied
```

## [@EnableAutoConfiguration 作用](https://note.clboy.cn/#/backend/springboot/autoconfig?id=enableautoconfiguration-作用)

- 利用EnableAutoConfigurationImportSelector给容器中导入一些组件

- getAutoConfigurationEntry方法中

  ```
  //获取候选的配置
  List<String> configurations = getCandidateConfigurations(annotationMetadata,attributes);Copy to clipboardErrorCopied
  ```

- getCandidateConfigurations方法中，SpringFactoriesLoader.loadFactoryNames()，扫描所有jar包类路径下 `META-INF/spring.factories`，把扫描到的这些文件的内容包装成properties对象，从properties中获取到EnableAutoConfiguration.class（类名）对应的值，然后把它们添加在容器中

  ![1573805094643](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573805094643.png)

- 每一个这样的 `xxxAutoConfiguration`类都是容器中的一个组件，都加入到容器中；用他们来做自动配置；

- 每一个自动配置类进行自动配置功能；

## [以HttpEncodingAutoConfiguration（Http编码自动配置）为例解释自动配置原理](https://note.clboy.cn/#/backend/springboot/autoconfig?id=以httpencodingautoconfiguration（http编码自动配置）为例解释自动配置原理)

```java
package org.springframework.boot.autoconfigure.web.servlet;

......

//表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
@Configuration(
    proxyBeanMethods = false
)

/**
 * 启动指定类的ConfigurationProperties功能；
 * 将配置文件中对应的值和HttpProperties绑定起来；
 * 并把HttpProperties加入到ioc容器中
 */
@EnableConfigurationProperties({HttpProperties.class})

/**
 * Spring底层@Conditional注解
 * 根据不同的条件，如果满足指定的条件，整个配置类里面的配置就会生效；
 * 判断当前应用是否是web应用，如果是，当前配置类生效
 */
@ConditionalOnWebApplication(
    type = Type.SERVLET
)

//判断当前项目有没有这个类
@ConditionalOnClass({CharacterEncodingFilter.class})

/**
 * 判断配置文件中是否存在某个配置  spring.http.encoding.enabled；如果不存在，判断也是成立的
 * 即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
 */
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)
public class HttpEncodingAutoConfiguration {

    //它已经和SpringBoot的配置文件映射了
    private final Encoding properties;

    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }

    @Bean     //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @ConditionalOnMissingBean    //判断容器有没有这个组件？（容器中没有才会添加这个组件）
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }

    ......Copy to clipboardErrorCopied
```

1. 根据当前不同的条件判断，决定这个配置类是否生效
2. 一但这个配置类生效；这个配置类就会给容器中添加各种组件；这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；

**所有在配置文件中能配置的属性都是在`xxxxProperties`类中封装着；配置文件能配置什么就可以参照某个功能对应的这个属性类**

```java
@ConfigurationProperties(
    prefix = "spring.http"
)
public class HttpProperties {
    private boolean logRequestDetails;
    private final HttpProperties.Encoding encoding = new HttpProperties.Encoding();Copy to clipboardErrorCopied
```

## [总结](https://note.clboy.cn/#/backend/springboot/autoconfig?id=总结)

- SpringBoot启动会加载大量的自动配置类
- 我们看我们需要的功能有没有SpringBoot默认写好的自动配置类
- 再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件有，我们就不需要再来配置了）
- 给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们就可以在配置文件中指定这些属性的值

`xxxxAutoConfigurartion`：自动配置类；

`xxxxProperties`:封装配置文件中相关属性；

## [@Conditional派生注解](https://note.clboy.cn/#/backend/springboot/autoconfig?id=conditional派生注解)

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

| @Conditional扩展注解            | 作用（判断是否满足当前指定条件）                 |
| ------------------------------- | ------------------------------------------------ |
| @ConditionalOnJava              | 系统的java版本是否符合要求                       |
| @ConditionalOnBean              | 容器中存在指定Bean；                             |
| @ConditionalOnMissingBean       | 容器中不存在指定Bean；                           |
| @ConditionalOnExpression        | 满足SpEL表达式指定                               |
| @ConditionalOnClass             | 系统中有指定的类                                 |
| @ConditionalOnMissingClass      | 系统中没有指定的类                               |
| @ConditionalOnSingleCandidate   | 容器中只有一个指定的Bean，或者这个Bean是首选Bean |
| @ConditionalOnProperty          | 系统中指定的属性是否有指定的值                   |
| @ConditionalOnResource          | 类路径下是否存在指定资源文件                     |
| @ConditionalOnWebApplication    | 当前是web环境                                    |
| @ConditionalOnNotWebApplication | 当前不是web环境                                  |
| @ConditionalOnJndi              | JNDI存在指定项                                   |

## [查看那些自动配置类生效了](https://note.clboy.cn/#/backend/springboot/autoconfig?id=查看那些自动配置类生效了)

自动配置类必须在一定的条件下才能生效；

我们怎么知道哪些自动配置类生效了；

我们可以通过配置文件启用 `debug=true`属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；

- `Positive matches ` ：（自动配置类启用的）
- `Negative matches`：（没有启动，没有匹配成功的自动配置类）



# [springboot日志配置](https://note.clboy.cn/#/backend/springboot/log?id=springboot日志配置)

## [市面上的日志框架](https://note.clboy.cn/#/backend/springboot/log?id=市面上的日志框架)

`JUL`、`JCL`、`Jboss-logging`、`logback`、`log4j`、`log4j2`、`slf4j`....

| 日志门面 （日志的抽象层）                                    | 日志门面 （日志的抽象层）                         |
| ------------------------------------------------------------ | ------------------------------------------------- |
| JCL（Jakarta Commons Logging **SLF4j（Simple Logging Facade for Java）** jboss-loggi | JUL（java.util.logging） Log4j Log4j2 **Logback** |

左边选一个门面（抽象层）、右边来选一个实现；

例：SLF4j-->Logback

SpringBoot选用 `SLF4j`和`logback`

## [SLF4j使用](https://note.clboy.cn/#/backend/springboot/log?id=slf4j使用)

如何在系统中使用SLF4j ：[https://www.slf4j.org](https://www.slf4j.org/)

以后开发的时候，日志记录方法的调用，不应该来直接调用日志的实现类，而是调用日志抽象层里面的方法；

给系统里面导入slf4j的jar和 logback的实现jar

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}Copy to clipboardErrorCopied
```

![日志](http://www.slf4j.org/images/concrete-bindings.png)

每一个日志的实现框架都有自己的配置文件。使用slf4j以后，配置文件还是做成日志实现框架自己本身的配置文件；

## [遗留问题](https://note.clboy.cn/#/backend/springboot/log?id=遗留问题)

项目中依赖的框架可能使用不同的日志：

Spring（commons-logging）、Hibernate（jboss-logging）、MyBatis、xxxx

当项目是使用多种日志API时，可以统一适配到SLF4J，中间使用SLF4J或者第三方提供的日志适配器适配到SLF4J，SLF4J在底层用开发者想用的一个日志框架来进行日志系统的实现，从而达到了多种日志的统一实现。

![统一日志](http://www.slf4j.org/images/legacy.png)

### [如何让系统中所有的日志都统一到slf4j](https://note.clboy.cn/#/backend/springboot/log?id=如何让系统中所有的日志都统一到slf4j)

1. 将系统中其他日志框架先排除出去；
2. 用中间包来替换原有的日志框架（适配器的类名和包名与替换的被日志框架一致）；
3. 我们导入slf4j其他的实现

## [SpringBoot日志关系](https://note.clboy.cn/#/backend/springboot/log?id=springboot日志关系)

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>Copy to clipboardErrorCopied
```

SpringBoot使用它来做日志功能；

```xml
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </dependency>Copy to clipboardErrorCopied
```

底层依赖关系

![img](https://note.clboy.cn/backend/springboot/media/cloudlandboy/%E8%B5%84%E6%96%99/%E6%95%99%E7%A8%8B/SpringBoot/%E6%BA%90%E7%A0%81%E3%80%81%E8%B5%84%E6%96%99%E3%80%81%E8%AF%BE%E4%BB%B6/%E6%96%87%E6%A1%A3/Spring%20Boot%20%E7%AC%94%E8%AE%B0/images/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180131220946.png)

总结：

1. SpringBoot底层也是使用slf4j+logback的方式进行日志记录
2. SpringBoot也把其他的日志都替换成了slf4j；
3. 中间替换包？

```java
@SuppressWarnings("rawtypes")
public abstract class LogFactory {

    static String UNSUPPORTED_OPERATION_IN_JCL_OVER_SLF4J = "http://www.slf4j.org/codes.html#unsupported_operation_in_jcl_over_slf4j";

    static LogFactory logFactory = new SLF4JLogFactory();Copy to clipboardErrorCopied
```

![img](https://note.clboy.cn/backend/springboot/media/cloudlandboy/%E8%B5%84%E6%96%99/%E6%95%99%E7%A8%8B/SpringBoot/%E6%BA%90%E7%A0%81%E3%80%81%E8%B5%84%E6%96%99%E3%80%81%E8%AF%BE%E4%BB%B6/%E6%96%87%E6%A1%A3/Spring%20Boot%20%E7%AC%94%E8%AE%B0/images/%E6%90%9C%E7%8B%97%E6%88%AA%E5%9B%BE20180131221411.png)

如果我们要引入其他框架？一定要把这个框架的默认日志依赖移除掉？

Spring框架用的是commons-logging；

```xml
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>commons-logging</groupId>
                    <artifactId>commons-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>Copy to clipboardErrorCopied
```

**SpringBoot能自动适配所有的日志，而且底层使用slf4j+logback的方式记录日志，引入其他框架的时候，只需要把这个框架依赖的日志框架排除掉即可；**

## [日志使用](https://note.clboy.cn/#/backend/springboot/log?id=日志使用)

### [默认配置](https://note.clboy.cn/#/backend/springboot/log?id=默认配置)

SpringBoot默认帮我们配置好了日志；

```java
    //记录器
    Logger logger = LoggerFactory.getLogger(getClass());
    @Test
    public void contextLoads() {
        //System.out.println();

        //日志的级别；
        //由低到高   trace<debug<info<warn<error
        //可以调整输出的日志级别；日志就只会在这个级别以以后的高级别生效
        logger.trace("这是trace日志...");
        logger.debug("这是debug日志...");
        //SpringBoot默认给我们使用的是info级别的，没有指定级别的就用SpringBoot默认规定的级别；root级别
        logger.info("这是info日志...");
        logger.warn("这是warn日志...");
        logger.error("这是error日志...");


    }Copy to clipboardErrorCopied
    日志输出格式：
        %d表示日期时间，
        %thread表示线程名，
        %-5level：级别从左显示5个字符宽度
        %logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
        %msg：日志消息，
        %n是换行符
    -->
    %d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n
Copy to clipboardErrorCopied
```

### [SpringBoot修改日志的默认配置](https://note.clboy.cn/#/backend/springboot/log?id=springboot修改日志的默认配置)

```properties
# 也可以指定一个包路径 logging.level.com.xxx=error
logging.level.root=error


#logging.path=
# 不指定路径在当前项目下生成springboot.log日志
# 可以指定完整的路径；
#logging.file=G:/springboot.log

# 在当前磁盘的根路径下创建spring文件夹和里面的log文件夹；使用 spring.log 作为默认文件
logging.path=/spring/log

#  在控制台输出的日志的格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
# 指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} ==== %msg%nCopy to clipboardErrorCopied
```

| logging.file | logging.path | Example  | Description                        |
| ------------ | ------------ | -------- | ---------------------------------- |
| (none)       | (none)       |          | 只在控制台输出                     |
| 指定文件名   | (none)       | my.log   | 输出日志到my.log文件               |
| (none)       | 指定目录     | /var/log | 输出到指定目录的 spring.log 文件中 |

### [指定配置](https://note.clboy.cn/#/backend/springboot/log?id=指定配置)

给类路径下放上每个日志框架自己的配置文件即可；SpringBoot就不使用他默认配置的了

| Logging System          | Customization                                                |
| ----------------------- | ------------------------------------------------------------ |
| Logback                 | `logback-spring.xml`, `logback-spring.groovy`, `logback.xml` or `logback.groovy` |
| Log4j2                  | `log4j2-spring.xml` or `log4j2.xml`                          |
| JDK (Java Util Logging) | `logging.properties`                                         |

logback.xml：直接就被日志框架识别了；

**logback-spring.xml**：日志框架就不直接加载日志的配置项，由SpringBoot解析日志配置，可以使用SpringBoot的高级Profile功能

```xml
<springProfile name="staging">
    <!-- configuration to be enabled when the "staging" profile is active -->
      可以指定某段配置只在某个环境下生效
</springProfile>
Copy to clipboardErrorCopied
```

如：

```xml
<appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
        <!--
        日志输出格式：
            %d表示日期时间，
            %thread表示线程名，
            %-5level：级别从左显示5个字符宽度
            %logger{50} 表示logger名字最长50个字符，否则按照句点分割。 
            %msg：日志消息，
            %n是换行符
        -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <springProfile name="dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ----> [%thread] ---> %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
            <springProfile name="!dev">
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} ==== [%thread] ==== %-5level %logger{50} - %msg%n</pattern>
            </springProfile>
        </layout>
    </appender>Copy to clipboardErrorCopied
```

如果使用logback.xml作为日志配置文件，还要使用profile功能，会有以下错误

```
no applicable action for [springProfile]
```

## [切换日志框架](https://note.clboy.cn/#/backend/springboot/log?id=切换日志框架)

可以按照slf4j的日志适配图，进行相关的切换；

slf4j+log4j的方式；

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <artifactId>logback-classic</artifactId>
      <groupId>ch.qos.logback</groupId>
    </exclusion>
    <exclusion>
      <artifactId>log4j-over-slf4j</artifactId>
      <groupId>org.slf4j</groupId>
    </exclusion>
  </exclusions>
</dependency>

<dependency>
  <groupId>org.slf4j</groupId>
  <artifactId>slf4j-log4j12</artifactId>
</dependency>
Copy to clipboardErrorCopied
```

切换为log4j2

```xml
   <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>spring-boot-starter-logging</artifactId>
                    <groupId>org.springframework.boot</groupId>
                </exclusion>
            </exclusions>
        </dependency>

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```



# [SpringBoot Web开发](https://note.clboy.cn/#/backend/springboot/helloweb?id=springboot-web开发)

1. 创建SpringBoot应用，选中我们需要的模块
2. SpringBoot已经默认将这些场景配置好了，只需要在配置文件中指定少量配置就可以运行起来
3. 自己编写业务代码

## [web自动配置规则](https://note.clboy.cn/#/backend/springboot/helloweb?id=web自动配置规则)

1. WebMvcAutoConfiguration
2. WebMvcProperties
3. ViewResolver自动配置
4. 静态资源自动映射
5. Formatter与Converter自动配置
6. HttpMessageConverter自动配置
7. 静态首页
8. favicon
9. 错误处理

## [SpringBoot对静态资源的映射规则](https://note.clboy.cn/#/backend/springboot/helloweb?id=springboot对静态资源的映射规则)

`WebMvcAutoConfiguration`类的`addResourceHandlers`方法：（添加资源映射）

```java
        public void addResourceHandlers(ResourceHandlerRegistry registry) {
            if (!this.resourceProperties.isAddMappings()) {
                logger.debug("Default resource handling disabled");
            } else {
                Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
                CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
                if (!registry.hasMappingForPattern("/webjars/**")) {
                    this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{"/webjars/**"}).addResourceLocations(new String[]{"classpath:/META-INF/resources/webjars/"}).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
                }

                String staticPathPattern = this.mvcProperties.getStaticPathPattern();
                if (!registry.hasMappingForPattern(staticPathPattern)) {
                    this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{staticPathPattern}).addResourceLocations(WebMvcAutoConfiguration.getResourceLocations(this.resourceProperties.getStaticLocations())).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
                }

            }
        }Copy to clipboardErrorCopied
```

所有 `/webjars/**` ，都去 `classpath:/META-INF/resources/webjars/` 找资源

`webjars`：以jar包的方式引入静态资源；

[webjars官网](https://www.webjars.org/)

![1573815091111](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573815091111.png)

例如：添加jquery的webjars

```xml
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.4.1</version>
        </dependency>Copy to clipboardErrorCopied
```

![1573815506777](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573815506777.png)

访问地址对应就是：http://localhost:8080/webjars/jquery/3.4.1/jquery.js

## [非webjars，自己的静态资源怎么访问](https://note.clboy.cn/#/backend/springboot/helloweb?id=非webjars，自己的静态资源怎么访问)

**资源配置类：**

```java
@ConfigurationProperties(    //说明可以在配置文件中配置相关参数
    prefix = "spring.resources",
    ignoreUnknownFields = false
)
public class ResourceProperties {
    private static final String[] CLASSPATH_RESOURCE_LOCATIONS = new String[]{"classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/"};
    private String[] staticLocations;
    private boolean addMappings;
    private final ResourceProperties.Chain chain;
    private final ResourceProperties.Cache cache;

    public ResourceProperties() {
        this.staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
        this.addMappings = true;
        this.chain = new ResourceProperties.Chain();
        this.cache = new ResourceProperties.Cache();
    }Copy to clipboardErrorCopied
```

![1573817274649](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573817274649.png)

上图中添加的映射访问路径`staticPathPattern`值是`/**`，对应的资源文件夹就是上面配置类`ResourceProperties`中的`CLASSPATH_RESOURCE_LOCATIONS`数组中的文件夹：

| 数组中的值                     | 在项目中的位置                         |
| ------------------------------ | -------------------------------------- |
| classpath:/META-INF/resources/ | src/main/resources/META-INF/resources/ |
| classpath:/resources/          | src/main/resources/resources/          |
| classpath:/static/             | src/main/resources/static/             |
| classpath:/public/             | src/main/resources/public/             |

localhost:8080/abc ---> 去静态资源文件夹里面找abc

## [欢迎页映射](https://note.clboy.cn/#/backend/springboot/helloweb?id=欢迎页映射)

![1573819949494](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573819949494.png)

`location`就是静态资源路径，所以欢迎页的页面就是上面静态资源下的`index.html`，被`/**`映射，因此直接访问项目就是访问欢迎页

## [网站图标映射（favicon.ico）](https://note.clboy.cn/#/backend/springboot/helloweb?id=网站图标映射（faviconico）)

所有的 favicon.ico 都是在静态资源文件下找；



# [模板引擎](https://note.clboy.cn/#/backend/springboot/templateengine?id=模板引擎)

常见的模板引擎有`JSP`、`Velocity`、`Freemarker`、`Thymeleaf`

SpringBoot推荐使用Thymeleaf；

## [引入thymeleaf](https://note.clboy.cn/#/backend/springboot/templateengine?id=引入thymeleaf)

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>Copy to clipboardErrorCopied
```

**如需切换thymeleaf版本：**

```xml
<properties>

        <thymeleaf.version>X.X.X.RELEASE</thymeleaf.version>

        <!-- 布局功能的支持程序  thymeleaf3主程序  layout2以上版本 -->
        <!-- thymeleaf2   layout1-->
        <thymeleaf-layout-dialect.version>2.2.2</thymeleaf-layout-dialect.version>

  </properties>Copy to clipboardErrorCopied
```

## [Thymeleaf使用](https://note.clboy.cn/#/backend/springboot/templateengine?id=thymeleaf使用)

```java
package org.springframework.boot.autoconfigure.thymeleaf;

......

@ConfigurationProperties(
    prefix = "spring.thymeleaf"
)
public class ThymeleafProperties {
    private static final Charset DEFAULT_ENCODING;
    public static final String DEFAULT_PREFIX = "classpath:/templates/";
    public static final String DEFAULT_SUFFIX = ".html";
    private boolean checkTemplate = true;
    private boolean checkTemplateLocation = true;
    private String prefix = "classpath:/templates/";
    private String suffix = ".html";
    private String mode = "HTML";Copy to clipboardErrorCopied
```

默认只要我们把HTML页面放在`classpath:/templates/`，thymeleaf就能自动渲染；

1. 创建模板文件`t1.html`，并导入thymeleaf的名称空间

   ```html
   <html lang="en" xmlns:th="http://www.thymeleaf.org">Copy to clipboardErrorCopied
   ```

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
   
   </body>
   </html>Copy to clipboardErrorCopied
   ```

2. 使用模板

   ```html
   <!DOCTYPE html>
   <html lang="en" xmlns:th="http://www.thymeleaf.org">
   <head>
       <meta charset="UTF-8">
       <title>[[${title}]]</title>
   </head>
   <body>
   <h1 th:text="${title}"></h1>
   <div th:text="${info}">这里的文本之后将会被覆盖</div>
   </body>
   </html>Copy to clipboardErrorCopied
   ```

3. 在controller中准备数据

   ```java
   @Controller
   public class HelloT {
   
       @RequestMapping("/ht")
       public String ht(Model model) {
           model.addAttribute("title","hello Thymeleaf")
                .addAttribute("info","this is first thymeleaf test");
           return "t1";
       }
   }Copy to clipboardErrorCopied
   ```

## [语法规则](https://note.clboy.cn/#/backend/springboot/templateengine?id=语法规则)

`th:text` --> 改变当前元素里面的文本内容；

`th：任意html属性 ` --> 来替换原生属性的值

![thymeleaf](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/2018-02-04_123955.png)

更多配置参考官方文档：https://www.thymeleaf.org/documentation.html

中文参考书册：https://www.lanzous.com/i7dzr2j



# [SpringMVC自动配置](https://note.clboy.cn/#/backend/springboot/springmvcconfig?id=springmvc自动配置)

Spring Boot为Spring MVC提供了自动配置，可与大多数应用程序完美配合。

以下是SpringBoot对SpringMVC的默认配置

**`org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration`**

自动配置在Spring的默认值之上添加了以下功能：

- 包含`ContentNegotiatingViewResolver`和`BeanNameViewResolver`。--> 视图解析器
- 支持服务静态资源，包括对WebJars的支持（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content)）。--> 静态资源文件夹路径
- 自动注册`Converter`，`GenericConverter`和`Formatter `beans。--> 转换器，格式化器
- 支持`HttpMessageConverters`（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-message-converters)）。--> SpringMVC用来转换Http请求和响应的；User---Json；
- 自动注册`MessageCodesResolver`（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-message-codes)）。--> 定义错误代码生成规则
- 静态`index.html`支持。--> 静态首页访问
- 定制`Favicon`支持（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-favicon)）。--> 网站图标
- 自动使用`ConfigurableWebBindingInitializer`bean（[官方文档中有介绍](https://docs.spring.io/spring-boot/docs/2.2.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-web-binding-initializer)）。

如果您想保留 Spring Boot MVC 的功能，并且需要添加其他 [MVC 配置](https://docs.spring.io/spring/docs/5.1.3.RELEASE/spring-framework-reference/web.html#mvc)（拦截器，格式化程序和视图控制器等），可以添加自己的 `WebMvcConfigurer` 类型的 `@Configuration` 类，但**不能**带 `@EnableWebMvc` 注解。如果您想自定义 `RequestMappingHandlerMapping`、`RequestMappingHandlerAdapter` 或者 `ExceptionHandlerExceptionResolver` 实例，可以声明一个 `WebMvcRegistrationsAdapter` 实例来提供这些组件。

如果您想完全掌控 Spring MVC，可以添加自定义注解了 `@EnableWebMvc` 的 @Configuration 配置类。

## [视图解析器](https://note.clboy.cn/#/backend/springboot/springmvcconfig?id=视图解析器)

视图解析器：根据方法的返回值得到视图对象（View），视图对象决定如何渲染（转发？重定向？）

- 自动配置了ViewResolver
- ContentNegotiatingViewResolver：组合所有的视图解析器的；

![1573873741438](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573873741438.png)

视图解析器从哪里来的？

![1573874365778](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573874365778.png)

**所以我们可以自己给容器中添加一个视图解析器；自动的将其组合进来**

```java
@Component
public class MyViewResolver implements ViewResolver {

    @Override
    public View resolveViewName(String s, Locale locale) throws Exception {
        return null;
    }
}Copy to clipboardErrorCopied
```

![1573875409759](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573875409759.png)

## [转换器、格式化器](https://note.clboy.cn/#/backend/springboot/springmvcconfig?id=转换器、格式化器)

- `Converter`：转换器； public String hello(User user)：类型转换使用Converter（表单数据转为user）
- `Formatter` 格式化器； 2017.12.17===Date；

```java
        @Bean
        //在配置文件中配置日期格式化的规则
        @ConditionalOnProperty(prefix = "spring.mvc", name = "date-format")
        public Formatter<Date> dateFormatter() {
            return new DateFormatter(this.mvcProperties.getDateFormat());//日期格式化组件
        }Copy to clipboardErrorCopied
```

**自己添加的格式化器转换器，我们只需要放在容器中即可**

## [HttpMessageConverters](https://note.clboy.cn/#/backend/springboot/springmvcconfig?id=httpmessageconverters)

- `HttpMessageConverter`：SpringMVC用来转换Http请求和响应的；User---Json；
- `HttpMessageConverters` 是从容器中确定；获取所有的HttpMessageConverter；

**自己给容器中添加HttpMessageConverter，只需要将自己的组件注册容器中（@Bean,@Component）**

## [MessageCodesResolver](https://note.clboy.cn/#/backend/springboot/springmvcconfig?id=messagecodesresolver)

**我们可以配置一个ConfigurableWebBindingInitializer来替换默认的；（添加到容器）**

## [扩展SpringMVC](https://note.clboy.cn/#/backend/springboot/springmvcconfig?id=扩展springmvc)

以前的配置文件中的配置

```xml
<mvc:view-controller path="/hello" view-name="success"/>Copy to clipboardErrorCopied
```

**现在，编写一个配置类（@Configuration），是WebMvcConfigurer类型；不能标注@EnableWebMvc**

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/hi").setViewName("success");
    }
}Copy to clipboardErrorCopied
```

访问：http://localhost:8080/hi

**原理：**

我们知道`WebMvcAutoConfiguration`是SpringMVC的自动配置类

下面这个类是`WebMvcAutoConfiguration`中的一个内部类

![1573891167026](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573891167026.png)

看一下`@Import({WebMvcAutoConfiguration.EnableWebMvcConfiguration.class})`中的这个类，

这个类依旧是`WebMvcAutoConfiguration`中的一个内部类

![1573891478014](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573891478014.png)

重点看一下这个类继承的父类`DelegatingWebMvcConfiguration`

```java
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
    private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();

    public DelegatingWebMvcConfiguration() {
    }

    //自动注入，从容器中获取所有的WebMvcConfigurer
    @Autowired(
        required = false
    )
    public void setConfigurers(List<WebMvcConfigurer> configurers) {
        if (!CollectionUtils.isEmpty(configurers)) {
            this.configurers.addWebMvcConfigurers(configurers);
        }

    }

    ......

    /**
     * 查看其中一个方法
      * this.configurers：也是WebMvcConfigurer接口的一个实现类
      * 看一下调用的configureViewResolvers方法 ↓
      */
    protected void configureViewResolvers(ViewResolverRegistry registry) {
        this.configurers.configureViewResolvers(registry);
    }Copy to clipboardErrorCopied
    public void configureViewResolvers(ViewResolverRegistry registry) {
        Iterator var2 = this.delegates.iterator();

        while(var2.hasNext()) {
            WebMvcConfigurer delegate = (WebMvcConfigurer)var2.next();
            //将所有的WebMvcConfigurer相关配置都来一起调用；  
            delegate.configureViewResolvers(registry);
        }

    }Copy to clipboardErrorCopied
```

容器中所有的WebMvcConfigurer都会一起起作用；

我们的配置类也会被调用；

效果：SpringMVC的自动配置和我们的扩展配置都会起作用；

![1573892805539](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573892805539.png)

## [全面接管SpringMVC](https://note.clboy.cn/#/backend/springboot/springmvcconfig?id=全面接管springmvc)

SpringBoot对SpringMVC的自动配置不需要了，所有都是由我们自己来配置；所有的SpringMVC的自动配置都失效了

**我们只需要在配置类中添加`@EnableWebMvc`即可；**

```java
@Configuration
@EnableWebMvc
public class MyMvcConfig implements WebMvcConfigurerCopy to clipboardErrorCopied
```

![1573892899452](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573892899452.png)

原理：

为什么@EnableWebMvc自动配置就失效了；

我们看一下EnableWebMvc注解类

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
@Documented
@Import({DelegatingWebMvcConfiguration.class})
public @interface EnableWebMvc {
}Copy to clipboardErrorCopied
```

重点在于`@Import({DelegatingWebMvcConfiguration.class})`

`DelegatingWebMvcConfiguration`是`WebMvcConfigurationSupport`的子类

我们再来看一下springmvc的自动配置类`WebMvcAutoConfiguration`

```java
@Configuration(
    proxyBeanMethods = false
)
@ConditionalOnWebApplication(
    type = Type.SERVLET
)
@ConditionalOnClass({Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class})

//重点是这个注解，只有当容器中没有这个类型组件的时候该配置类才会生效
@ConditionalOnMissingBean({WebMvcConfigurationSupport.class})

@AutoConfigureOrder(-2147483638)
@AutoConfigureAfter({DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class, ValidationAutoConfiguration.class})
public class WebMvcAutoConfiguration Copy to clipboardErrorCopied
```

1. @EnableWebMvc将WebMvcConfigurationSupport组件导入进来；
2. 导入的WebMvcConfigurationSupport只是SpringMVC最基本的功能；

## [如何修改SpringBoot的默认配置](https://note.clboy.cn/#/backend/springboot/springmvcconfig?id=如何修改springboot的默认配置)

SpringBoot在自动配置很多组件的时候，先看容器中有没有用户自己配置的（@Bean、@Component）如果有就用用户配置的，如果没有，才自动配置；如果有些组件可以有多个（ViewResolver）将用户配置的和自己默认的组合起来；

- 在SpringBoot中会有非常多的xxxConfigurer帮助我们进行扩展配置
- 在SpringBoot中会有很多的xxxCustomizer帮助我们进行定制配置



## [CRUD-员工列表](https://note.clboy.cn/#/backend/springboot/restfulcrud?id=crud-员工列表)

使用rest风格

| 实验功能                             | 请求URI | 请求方式 |
| ------------------------------------ | ------- | -------- |
| 查询所有员工                         | emps    | GET      |
| 查询某个员工(来到修改页面)           | emp/1   | GET      |
| 来到添加页面                         | emp     | GET      |
| 添加员工                             | emp     | POST     |
| 来到修改页面（查出员工进行信息回显） | emp/1   | GET      |
| 修改员工                             | emp     | PUT      |
| 删除员工                             | emp/1   | DELETE   |

1. 为了页面结构清晰，在template文件夹下新建emp文件夹，将list.html移动到emp文件夹下

2. 将dao层和实体层[java代码](https://www.lanzous.com/i7eenib)复制到项目中`dao`，`entities`

3. 添加员工controller，实现查询员工列表的方法

   ```java
   @Controller
   public class EmpController {
   
       @Autowired
       private EmployeeDao employeeDao;
   
       @GetMapping("/emps")
       public String emps(Model model) {
           Collection<Employee> empList = employeeDao.getAll();
           model.addAttribute("emps", empList);
           return "emp/list";
       }
   
   }Copy to clipboardErrorCopied
   ```

4. 修改后台页面，更改左侧侧边栏，将`customer`改为`员工列表`，并修改请求路径

   ```html
   <li class="nav-item">
       <a class="nav-link" th:href="@{/emps}">
           <svg .....>
               ......
           </svg>
           员工列表
       </a>
   </li>Copy to clipboardErrorCopied
   ```

5. 同样emp/list页面的左边侧边栏是和后台页面一模一样的，每个都要修改很麻烦，接下来，抽取公共片段

## [thymeleaf公共页面元素抽取](https://note.clboy.cn/#/backend/springboot/restfulcrud?id=thymeleaf公共页面元素抽取)

### [语法](https://note.clboy.cn/#/backend/springboot/restfulcrud?id=语法)

~{templatename::selector}：模板名::选择器

~{templatename::fragmentname}:模板名::片段名

```html
/*公共代码片段*/
<footer th:fragment="copy">
    &copy; 2011 The Good Thymes Virtual Grocery
</footer>

/*引用代码片段*/
<div th:insert="~{footer :: copy}"></di

/*（〜{...}包围是完全可选的，所以上⾯的代码 将等价于：*/
<div th:insert="footer :: copy"></diCopy to clipboardErrorCopied
```

具体参考官方文档：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#including-template-fragments

三种引入公共片段的th属性：

- `th:insert`：将公共片段整个插入到声明引入的元素中
- `th:replace`：将声明引入的元素替换为公共片段
- `th:include`：将被引入的片段的内容包含进这个标签中

```html
/*公共片段*/
<footer th:fragment="copy">
&copy; 2011 The Good Thymes Virtual Grocery
</footer>

/*引入方式*/
<div th:insert="footer :: copy"></div>
<div th:replace="footer :: copy"></div>
<div th:include="footer :: copy"></div>


/*效果*/
<div>
    <footer>
    &copy; 2011 The Good Thymes Virtual Grocery
    </footer>
</div>

<footer>
&copy; 2011 The Good Thymes Virtual Grocery
</footer>

<div>
&copy; 2011 The Good Thymes Virtual Grocery
</div>Copy to clipboardErrorCopied
```

### [后台页面抽取](https://note.clboy.cn/#/backend/springboot/restfulcrud?id=后台页面抽取)

1. 将后台主页中的顶部导航栏作为片段，在list页面引入

   **dashboard.html：**

   ```html
           <nav th:fragment="topbar" class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0">
               <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">Company name</a>
               <input class="form-control form-control-dark w-100" type="text" placeholder="Search" aria-label="Search">
               <ul class="navbar-nav px-3">
                   <li class="nav-item text-nowrap">
                       <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">Sign out</a>
                   </li>
               </ul>
           </nav>Copy to clipboardErrorCopied
   ```

   **list.html：**

   ```html
   <body>
   
   <div th:replace="dashboard::topbar"></div>
   
   ......Copy to clipboardErrorCopied
   ```

2. 使用选择器的方式 抽取左侧边栏代码

   **dashboard.html：**

   ```html
   <div class="container-fluid">
       <div class="row">
           <nav id="sidebar" class="col-md-2 d-none d-md-block bg-light sidebar" ......Copy to clipboardErrorCopied
   ```

   **list.html：**

   ```html
   <div class="container-fluid">
       <div class="row">
           <div th:replace="dashboard::#sidebar"></div>
           ......Copy to clipboardErrorCopied
   ```

### [引入片段传递参数](https://note.clboy.cn/#/backend/springboot/restfulcrud?id=引入片段传递参数)

实现点击当前项高亮

将`dashboard.html`中的公共代码块抽出为单独的html文件，放到commos文件夹下

在引入代码片段的时候可以传递参数，然后在sidebar代码片段模板中判断当前点击的链接

语法：

```
~{templatename::selector(变量名=值)}

/*或者在定义代码片段时，定义参数*/
<nav th:fragment="topbar(A,B)"
/*引入时直接传递参数*/
~{templatename::fragmentname(A值,B值)}Copy to clipboardErrorCopied
```

**topbar.html**

```html
<!doctype html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<body>
<nav th:fragment="topbar" class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0">
    <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">Company
        name</a>
    <input class="form-control form-control-dark w-100" type="text" placeholder="Search" aria-label="Search">
    <ul class="navbar-nav px-3">
        <li class="nav-item text-nowrap">
            <a class="nav-link" href="http://getbootstrap.com/docs/4.0/examples/dashboard/#">Sign out</a>
        </li>
    </ul>
</nav>
</body>
</html>Copy to clipboardErrorCopied
```

**sidebar.html**

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<nav id="sidebar" class="col-md-2 d-none d-md-block bg-light sidebar">
    <div class="sidebar-sticky">
        <ul class="nav flex-column">
            <li class="nav-item">
                <a class="nav-link active" th:class="${currentURI}=='main.html'?'nav-link active':'nav-link'" th:href="@{/main.html}">
      .....
</body>
</html>Copy to clipboardErrorCopied
```

然后在`dashboard.html`和`list.html`中引入

```html
<body>
<div th:replace="commons/topbar::topbar"></div>
<div class="container-fluid">
    <div class="row">
        <div th:replace="commons/sidebar::#sidebar(currentURI='main.html')"></div>
        ......Copy to clipboardErrorCopied
<body>
<div th:replace="commons/topbar::topbar"></div>

<div class="container-fluid">
    <div class="row">
        <div th:replace="commons/sidebar::#sidebar(currentURI='emps')"></div>
        ......Copy to clipboardErrorCopied
```

1. 显示员工数据，添加增删改按钮

   ```html
           <main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
               <h2>
                   <button class="btn btn-sm btn-success">添加员工</button>
               </h2>
               <div class="table-responsive">
                   <table class="table table-striped table-sm">
                       <thead>
                       <tr>
                           <th>员工号</th>
                           <th>姓名</th>
                           <th>邮箱</th>
                           <th>性别</th>
                           <th>部门</th>
                           <th>生日</th>
                           <th>操作</th>
                       </tr>
                       </thead>
                       <tbody>
                       <tr th:each="emp:${emps}">
                           <td th:text="${emp.id}"></td>
                           <td th:text="${emp.lastName}"></td>
                           <td th:text="${emp.email}"></td>
                           <td th:text="${emp.gender}==1?'男':'女'"></td>
                           <td th:text="${emp.department.departmentName}"></td>
                           <td th:text="${#dates.format(emp.birth,'yyyy-MM-dd')}"></td>
                           <td>
                               <button class="btn btn-sm btn-primary">修改</button>
                               <button class="btn btn-sm btn-danger">删除</button>
                           </td>
                       </tr>
                       </tbody>
                   </table>
               </div>
           </main>Copy to clipboardErrorCopied
   ```

### [员工添加](https://note.clboy.cn/#/backend/springboot/restfulcrud?id=员工添加)

1. 创建员工添加页面`add.html`

   ```html
   ......
   <body>
   <div th:replace="commons/topbar::topbar"></div>
   
   <div class="container-fluid">
       <div class="row">
           <div th:replace="commons/sidebar::#sidebar(currentURI='emps')"></div>
           <main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
               <form>
                   <div class="form-group">
                       <label>LastName</label>
                       <input name="lastName" type="text" class="form-control" placeholder="zhangsan">
                   </div>
                   <div class="form-group">
                       <label>Email</label>
                       <input  name="email" type="email" class="form-control" placeholder="zhangsan@atguigu.com">
                   </div>
                   <div class="form-group">
                       <label>Gender</label><br/>
                       <div class="form-check form-check-inline">
                           <input class="form-check-input" type="radio" name="gender" value="1">
                           <label class="form-check-label">男</label>
                       </div>
                       <div class="form-check form-check-inline">
                           <input class="form-check-input" type="radio" name="gender" value="0">
                           <label class="form-check-label">女</label>
                       </div>
                   </div>
                   <div class="form-group">
                       <label>department</label>
                       <select name="department.id" class="form-control">
                           <option th:each="dept:${departments}" th:text="${dept.departmentName}" th:value="${dept.id}"></option>
                       </select>
                   </div>
                   <div class="form-group">
                       <label>Birth</label>
                       <input name="birth" type="text" class="form-control" placeholder="zhangsan">
                   </div>
                   <button type="submit" class="btn btn-primary">添加</button>
               </form>
           </main>
       </div>
   </div>
   ......Copy to clipboardErrorCopied
   ```

2. 点击链接跳转到添加页面

   ```html
   <a href="/emp" th:href="@{/emp}" class="btn btn-sm btn-success">添加员工</a>Copy to clipboardErrorCopied
   ```

3. `EmpController`添加映射方法

   ```java
       @Autowired
       private DepartmentDao departmentDao;
   
       @GetMapping("/emp")
       public String toAddPage(Model model) {
           //准备部门下拉框数据
           Collection<Department> departments = departmentDao.getDepartments();
           model.addAttribute("departments",departments);
           return "emp/add";
       }Copy to clipboardErrorCopied
   ```

1. 修改页面遍历添加下拉选项

   ```html
   <select class="form-control">
       <option th:each="dept:${departments}" th:text="${dept.departmentName}"></option>
   </select>Copy to clipboardErrorCopied
   ```

2. 表单提交，添加员工

   ```html
   <form th:action="@{/emp}" method="post">Copy to clipboardErrorCopied
   ```

   ```java
       @PostMapping("/emp")
       public String add(Employee employee) {
           System.out.println(employee);
           //模拟添加到数据库
           employeeDao.save(employee);
           //添加成功重定向到列表页面
           return "redirect:/emps";
       }Copy to clipboardErrorCopied
   ```

### [日期格式修改](https://note.clboy.cn/#/backend/springboot/restfulcrud?id=日期格式修改)

表单提交的日期格式必须是`yyyy/MM/dd`的格式，可以在配置文件中修改格式

```properties
spring.mvc.date-format=yyyy-MM-ddCopy to clipboardErrorCopied
```

### [员工修改](https://note.clboy.cn/#/backend/springboot/restfulcrud?id=员工修改)

1. 点击按钮跳转到编辑页面

   ```html
    <a th:href="@{/emp/}+${emp.id}" class="btn btn-sm btn-primary">修改</a>Copy to clipboardErrorCopied
   ```

2. 添加编辑页面，表单的提交要为post方式，提供`_method`参数

   ```html
   <body>
   <div th:replace="commons/topbar::topbar"></div>
   
   <div class="container-fluid">
       <div class="row">
           <div th:replace="commons/sidebar::#sidebar(currentURI='emps')"></div>
           <main role="main" class="col-md-9 ml-sm-auto col-lg-10 pt-3 px-4">
               <form th:action="@{/emp}" method="post">
                   <!--员工id-->
                   <input type="hidden" name="id" th:value="${emp.id}">
                   <!--http请求方式-->
                   <input type="hidden" name="_method" value="put">
                   <div class="form-group">
                       <label>LastName</label>
                       <input name="lastName" th:value="${emp.lastName}" type="text" class="form-control" placeholder="zhangsan">
                   </div>
                   <div class="form-group">
                       <label>Email</label>
                       <input  name="email" th:value="${emp.email}" type="email" class="form-control" placeholder="zhangsan@atguigu.com">
                   </div>
                   <div class="form-group">
                       <label>Gender</label><br/>
                       <div class="form-check form-check-inline">
                           <input class="form-check-input" type="radio" name="gender" value="1" th:checked="${emp.gender==1}">
                           <label class="form-check-label">男</label>
                       </div>
                       <div class="form-check form-check-inline">
                           <input class="form-check-input" type="radio" name="gender" value="0" th:checked="${emp.gender==0}">
                           <label class="form-check-label">女</label>
                       </div>
                   </div>
                   <div class="form-group">
                       <label>department</label>
                       <select name="department.id" class="form-control">
                           <option th:each="dept:${departments}" th:value="${dept.id}" th:selected="${dept.id}==${emp.department.id}" th:text="${dept.departmentName}"></option>
                       </select>
                   </div>
                   <div class="form-group">
                       <label>Birth</label>
                       <input name="birth" type="text" class="form-control" placeholder="zhangsan" th:value="${#dates.format(emp.birth,'yyyy-MM-dd')}">
                   </div>
                   <button type="submit" class="btn btn-primary">添加</button>
               </form>
           </main>
       </div>
   </div>
   
   ......Copy to clipboardErrorCopied
   ```

3. Controller转发到编辑页面，回显员工信息

   ```java
       @GetMapping("/emp/{id}")
       public String toEditPage(@PathVariable Integer id, Model model) {
           Employee employee = employeeDao.get(id);
           //准备部门下拉框数据
           Collection<Department> departments = departmentDao.getDepartments();
           model.addAttribute("emp", employee).addAttribute("departments", departments);
           return "emp/edit";
       }Copy to clipboardErrorCopied
   ```

4. 提交表单修改员工信息

   ```java
       @PutMapping("/emp")
       public String update(Employee employee) {
           employeeDao.save(employee);
           return "redirect:/emps";
       }Copy to clipboardErrorCopied
   ```

### [员工删除](https://note.clboy.cn/#/backend/springboot/restfulcrud?id=员工删除)

1. 点击删除提交发出delete请求

   ```html
       @DeleteMapping("/emp/{id}")
       public String delete(@PathVariable String id){
           employeeDao.delete(id);
           return "redirect:/emps";
       }Copy to clipboardErrorCopied
   ```

   如果提示不支持POST请求，在确保代码无误的情况下查看是否配置启动`HiddenHttpMethodFilter`

   ```properties
   spring.mvc.hiddenmethod.filter.enabled=trueCopy to clipboardErrorCopied
   ```

   ![1573987255217](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573987255217.png)

   这个好像是2.0版本以后修改的

   ![1573988088629](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573988088629.png)

   如果删除不掉，请修改`EmployeeDao`，把String转为Integer类型

   ```java
       public void delete(String id) {
           employees.remove(Integer.parseInt(id));
       }
   ```



# [SpringBoot默认的错误处理机制](https://note.clboy.cn/#/backend/springboot/errorhandler?id=springboot默认的错误处理机制)

当访问一个不存在的页面，或者程序抛出异常时

**默认效果：**

- 浏览器返回一个默认的错误页面， 注意看浏览器发送请求的`请求头`：

  ![1573993746920](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573993746920.png)

- 其他客户端返回json数据，注意看`请求头`

  ![1573996455049](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573996455049.png)

查看`org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration`源码，

这里是springboot错误处理的自动配置信息

**主要给日容器中注册了以下组件：**

- ErrorPageCustomizer 系统出现错误以后来到error请求进行处理；相当于（web.xml注册的错误页面规则）
- BasicErrorController 处理/error请求
- DefaultErrorViewResolver 默认的错误视图解析器
- DefaultErrorAttributes 错误信息
- defaultErrorView 默认错误视图

## [ErrorPageCustomizer](https://note.clboy.cn/#/backend/springboot/errorhandler?id=errorpagecustomizer)

```java
    @Bean
    public ErrorMvcAutoConfiguration.ErrorPageCustomizer errorPageCustomizer(DispatcherServletPath dispatcherServletPath) {
        return new ErrorMvcAutoConfiguration.ErrorPageCustomizer(this.serverProperties, dispatcherServletPath);
    }Copy to clipboardErrorCopied
    private static class ErrorPageCustomizer implements ErrorPageRegistrar, Ordered {
        private final ServerProperties properties;
        private final DispatcherServletPath dispatcherServletPath;

        protected ErrorPageCustomizer(ServerProperties properties, DispatcherServletPath dispatcherServletPath) {
            this.properties = properties;
            this.dispatcherServletPath = dispatcherServletPath;
        }

        //注册错误页面
        public void registerErrorPages(ErrorPageRegistry errorPageRegistry) {
            //getPath()获取到的是"/error"，见下图
            ErrorPage errorPage = new ErrorPage(this.dispatcherServletPath.getRelativePath(this.properties.getError().getPath()));
            errorPageRegistry.addErrorPages(new ErrorPage[]{errorPage});
        }

        public int getOrder() {
            return 0;
        }
    }Copy to clipboardErrorCopied
```

![1573994730632](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573994730632.png)

当请求出现错误后就会转发到`/error`

然后这个error请求就会被BasicErrorController处理；

## [BasicErrorController](https://note.clboy.cn/#/backend/springboot/errorhandler?id=basicerrorcontroller)

```java
    @Bean
    @ConditionalOnMissingBean(
        value = {ErrorController.class},
        search = SearchStrategy.CURRENT
    )
    public BasicErrorController basicErrorController(ErrorAttributes errorAttributes, ObjectProvider<ErrorViewResolver> errorViewResolvers) {
        return new BasicErrorController(errorAttributes, this.serverProperties.getError(), (List)errorViewResolvers.orderedStream().collect(Collectors.toList()));
    }Copy to clipboardErrorCopied
```

处理`/error`请求

```java
@Controller
/**
  * 使用配置文件中server.error.path配置
  * 如果server.error.path没有配置使用error.path
  * 如果error.path也没有配置就使用/error
  */
@RequestMapping({"${server.error.path:${error.path:/error}}"})
public class BasicErrorController extends AbstractErrorControllerCopy to clipboardErrorCopied
```

![1573996271708](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1573996271708.png)

这两个方法一个用于浏览器请求响应html页面，一个用于其他客户端请求响应json数据

处理浏览器请求的方法 中，modelAndView存储到哪个页面的页面地址和页面内容数据

看一下调用的resolveErrorView方法

```java
    protected ModelAndView resolveErrorView(HttpServletRequest request, HttpServletResponse response, HttpStatus status, Map<String, Object> model) {
        Iterator var5 = this.errorViewResolvers.iterator();

        ModelAndView modelAndView;
        do {
            if (!var5.hasNext()) {
                return null;
            }

            ErrorViewResolver resolver = (ErrorViewResolver)var5.next();
            //从所有的ErrorViewResolver得到ModelAndView
            modelAndView = resolver.resolveErrorView(request, status, model);
        } while(modelAndView == null);

        return modelAndView;
    }Copy to clipboardErrorCopied
```

ErrorViewResolver从哪里来的呢？

已经在容器中注册了一个DefaultErrorViewResolver

## [DefaultErrorViewResolver](https://note.clboy.cn/#/backend/springboot/errorhandler?id=defaulterrorviewresolver)

```java
    @Configuration(
        proxyBeanMethods = false
    )
    static class DefaultErrorViewResolverConfiguration {
        private final ApplicationContext applicationContext;
        private final ResourceProperties resourceProperties;

        DefaultErrorViewResolverConfiguration(ApplicationContext applicationContext, ResourceProperties resourceProperties) {
            this.applicationContext = applicationContext;
            this.resourceProperties = resourceProperties;
        }

        //注册默认错误视图解析器
        @Bean
        @ConditionalOnBean({DispatcherServlet.class})
        @ConditionalOnMissingBean({ErrorViewResolver.class})
        DefaultErrorViewResolver conventionErrorViewResolver() {
            return new DefaultErrorViewResolver(this.applicationContext, this.resourceProperties);
        }
    }Copy to clipboardErrorCopied
```

然后调用ErrorViewResolver的`resolveErrorView()`方法

```java
    public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status, Map<String, Object> model) {
        //把状态码和model传过去获取视图
        ModelAndView modelAndView = this.resolve(String.valueOf(status.value()), model);

        //上面没有获取到视图就使用把状态吗替换再再找，以4开头的替换为4xx，5开头替换为5xx，见下文（如果定制错误响应）
        if (modelAndView == null && SERIES_VIEWS.containsKey(status.series())) {
            modelAndView = this.resolve((String)SERIES_VIEWS.get(status.series()), model);
        }

        return modelAndView;
    }

    private ModelAndView resolve(String viewName, Map<String, Object> model) {
        //viewName传过来的是状态码，例：/error/404
        String errorViewName = "error/" + viewName;
        TemplateAvailabilityProvider provider = this.templateAvailabilityProviders.getProvider(errorViewName, this.applicationContext);
        //模板引擎可以解析这个页面地址就用模板引擎解析
        return provider != null ? new ModelAndView(errorViewName, model) : this.resolveResource(errorViewName, model);
    }Copy to clipboardErrorCopied
```

如果模板引擎不可用，就调用resolveResource方法获取视图

```java
    private ModelAndView resolveResource(String viewName, Map<String, Object> model) {
        //获取的是静态资源文件夹
        String[] var3 = this.resourceProperties.getStaticLocations();
        int var4 = var3.length;

        for(int var5 = 0; var5 < var4; ++var5) {
            String location = var3[var5];

            try {
                Resource resource = this.applicationContext.getResource(location);
                //例：static/error.html
                resource = resource.createRelative(viewName + ".html");
                //存在则返回视图
                if (resource.exists()) {
                    return new ModelAndView(new DefaultErrorViewResolver.HtmlResourceView(resource), model);
                }
            } catch (Exception var8) {
            }
        }

        return null;
    }Copy to clipboardErrorCopied
```

**所以：**

## [如何定制错误响应页面](https://note.clboy.cn/#/backend/springboot/errorhandler?id=如何定制错误响应页面)

- 有模板引擎的情况下；将错误页面命名为 `错误状态码.html` 放在模板引擎文件夹里面的 error文件夹下发生此状态码的错误就会来到这里找对应的页面；

  比如我们在template文件夹下创建error/404.html当浏览器请求是404错误，就会使用我们创建的404.html页面响应，如果是其他状态码错误，还是使用默认的视图，但是如果404.html没有找到就会替换成4XX.html再查找一次，看`DefaultErrorViewResolver`中的静态代码块

  ```java
      static {
          Map<Series, String> views = new EnumMap(Series.class);
          views.put(Series.CLIENT_ERROR, "4xx");
          views.put(Series.SERVER_ERROR, "5xx");
          SERIES_VIEWS = Collections.unmodifiableMap(views);
      }
  
  .....
   //再看解析方法
              //把状态码和model传过去过去视图
          ModelAndView modelAndView = this.resolve(String.valueOf(status.value()), model);
  
          //上面没有获取到视图就把状态吗替换再找，以4开头的替换为4xx，5开头替换为5xx，见下文（如果定制错误响应）
          if (modelAndView == null && SERIES_VIEWS.containsKey(status.series())) {
              modelAndView = this.resolve((String)SERIES_VIEWS.get(status.series()), model);
          }Copy to clipboardErrorCopied
  ```

  **页面可以获取哪些数据**

## [DefaultErrorAttributes](https://note.clboy.cn/#/backend/springboot/errorhandler?id=defaulterrorattributes)

再看一下BasicErrorController的errorHtml方法

```java
    public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
        HttpStatus status = this.getStatus(request);

        //model的数据
        Map<String, Object> model = Collections.unmodifiableMap(this.getErrorAttributes(request, this.isIncludeStackTrace(request, MediaType.TEXT_HTML)));
        response.setStatus(status.value());
        ModelAndView modelAndView = this.resolveErrorView(request, response, status, model);
        return modelAndView != null ? modelAndView : new ModelAndView("error", model);
    }Copy to clipboardErrorCopied
```

看一下调用的this.getErrorAttributes()方法

```java
    protected Map<String, Object> getErrorAttributes(HttpServletRequest request, boolean includeStackTrace) {
        WebRequest webRequest = new ServletWebRequest(request);
        return this.errorAttributes.getErrorAttributes(webRequest, includeStackTrace);
    }Copy to clipboardErrorCopied
```

再看 this.errorAttributes.getErrorAttributes()方法， this.errorAttributes是接口类型ErrorAttributes，实现类就一个`DefaultErrorAttributes`，看一下`DefaultErrorAttributes`的 getErrorAttributes()方法

```java
    public Map<String, Object> getErrorAttributes(WebRequest webRequest, boolean includeStackTrace) {
        Map<String, Object> errorAttributes = new LinkedHashMap();
        errorAttributes.put("timestamp", new Date());
        this.addStatus(errorAttributes, webRequest);
        this.addErrorDetails(errorAttributes, webRequest, includeStackTrace);
        this.addPath(errorAttributes, webRequest);
        return errorAttributes;
    }Copy to clipboardErrorCopied
```

- timestamp：时间戳
- status：状态码
- error：错误提示
- exception：异常对象
- message：异常消息
- errors：JSR303数据校验的错误都在这里

2.0以后默认是不显示exception的，需要在配置文件中开启

```java
server.error.include-exception=trueCopy to clipboardErrorCopied
```

原因：

![1574001983704](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574001983704.png)

在注册时

![1574002101183](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574002101183.png)

- 没有模板引擎（模板引擎找不到这个错误页面），就会在静态资源文件夹下找；
- 如果以上都没有找到错误页面，就是默认来到SpringBoot默认的错误提示页面；

## [defaultErrorView](https://note.clboy.cn/#/backend/springboot/errorhandler?id=defaulterrorview)

![1574146899180](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574146899180.png)

![1574146843810](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574146843810.png)

## [如何定制JSON数据](https://note.clboy.cn/#/backend/springboot/errorhandler?id=如何定制json数据)

springboot做了自适应效果，浏览器访问响应错误页面。客户端访问响应错误信息的json数据

1. 第一种方法，定义全局异常处理器类注入到容器中，捕获到异常返回json格式的数据

   ```java
   @ControllerAdvice
   public class MyExceptionHandler {
   
       @ResponseBody
       @ExceptionHandler(Exception.class)
       public Map<String, Object> handleException(Exception e) {
           Map<String, Object> map = new HashMap(2);
           map.put("code", "100011");
           map.put("msg", e.getMessage());
           return map;
       }
   }Copy to clipboardErrorCopied
   ```

   访问[localhost:8080/hello?str=hi](http://localhost:8080/hello?str=hi)

   ```java
   @RestController
   public class Hello {
   
       @RequestMapping("/hello")
       public String hello(String str) {
           if ("hi".equals(str)) {
               int i = 10 / 0;
           }
           return "hello world";
       }
   }Copy to clipboardErrorCopied
   ```

   这样的话，不管是浏览器访问还是客户端访问都是响应json数据，就没有了自适应效果

   1. 第二种方法，捕获到异常后转发到/error

      ```java
      @ControllerAdvice
      public class MyExceptionHandler {
      
          @ExceptionHandler(Exception.class)
          public String handleException(Exception e) {
              Map<String, Object> map = new HashMap(2);
              map.put("code", "100011");
              map.put("msg", e.getMessage());
              return "forward:/error";
          }
      }Copy to clipboardErrorCopied
      ```

      访问[localhost:8080/hello?str=hi](http://localhost:8080/hello?str=hi)，但这样异常被我们捕获然后转发，显示的状态码就是200，所以在转发之前还要设置一下状态码

      ```java
          @ExceptionHandler(Exception.class)
          public String handleException(Exception e, HttpServletRequest request) {
              Map<String, Object> map = new HashMap(2);
              map.put("code", "100011");
              map.put("msg", e.getMessage());
      
              //设置状态码
              request.setAttribute("javax.servlet.error.status_code", 500);
              return "forward:/error";
          }Copy to clipboardErrorCopied
      ```

      但是设置的数据就没有用了，只能使用默认的

      由上面我们已经知道数据的来源是调用DefaultErrorAttributes的getErrorAttributes方法得到的，而这个DefaultErrorAttributes是在ErrorMvcAutoConfiguration配置类中注册的，并且注册之前会检查容器中是否已经拥有

      ```java
          @Bean
          @ConditionalOnMissingBean(
              value = {ErrorAttributes.class},
              search = SearchStrategy.CURRENT
          )
          public DefaultErrorAttributes errorAttributes() {
              return new DefaultErrorAttributes(this.serverProperties.getError().isIncludeException());
          }Copy to clipboardErrorCopied
      ```

      所以我们可以只要实现ErrorAttributes接口或者继承DefaultErrorAttributes类，然后注册到容器中就行了

      ```java
      @ControllerAdvice
      public class MyExceptionHandler {
      
          @ExceptionHandler(Exception.class)
          public String handleException(Exception e, HttpServletRequest request) {
              Map<String, Object> map = new HashMap(2);
              map.put("name", "hello");
              map.put("password", "123456");
      
              //设置状态码
              request.setAttribute("javax.servlet.error.status_code", 500);
      
              //把数据放到request域中
              request.setAttribute("ext", map);
              return "forward:/error";
          }
      }Copy to clipboardErrorCopied
      ```

      ```java
      @Configuration
      public class MyMvcConfig implements WebMvcConfigurer {
      
          @Bean
          public DefaultErrorAttributes errorAttributes() {
              return new MyErrorAttributes();
          }
      
          class MyErrorAttributes extends DefaultErrorAttributes {
              @Override
              public Map<String, Object> getErrorAttributes(WebRequest webRequest, boolean includeStackTrace) {
                  //调用父类的方法获取默认的数据
                  Map<String, Object> map = new HashMap<>(super.getErrorAttributes(webRequest, includeStackTrace));
                  //从request域从获取到自定义数据
                  Map<String, Object> ext = (Map<String, Object>) webRequest.getAttribute("ext", RequestAttributes.SCOPE_REQUEST);
                  map.putAll(ext);
                  return map;
              }
          }
      
          ......
      ```





# [配置嵌入式Servlet容器](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=配置嵌入式servlet容器)

## [如何定制和修改Servlet容器的相关配置](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=如何定制和修改servlet容器的相关配置)

1. 修改和server有关的配置

   ```properties
   server.port=8081
   server.context-path=/crud
   
   server.tomcat.uri-encoding=UTF-8
   
   //通用的Servlet容器设置
   server.xxx
   //Tomcat的设置
   server.tomcat.xxxCopy to clipboardErrorCopied
   ```

2. 编写一个~~EmbeddedServletContainerCustomizer~~，2.0以后改为`WebServerFactoryCustomizer`：嵌入式的Servlet容器的定制器；来修改Servlet容器的配置

   ```java
   @Configuration
   public class MyMvcConfig implements WebMvcConfigurer {
       @Bean
       public WebServerFactoryCustomizer webServerFactoryCustomizer() {
           return new WebServerFactoryCustomizer<ConfigurableWebServerFactory>() {
               @Override
               public void customize(ConfigurableWebServerFactory factory) {
                   factory.setPort(8088);
               }
           };
       }
   ......Copy to clipboardErrorCopied
   ```

代码方式的配置会覆盖配置文件的配置

*小Tips：* 如果使用的是360极速浏览器就不要用8082端口了

## [注册Servlet三大组件](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=注册servlet三大组件)

由于SpringBoot默认是以jar包的方式启动嵌入式的Servlet容器来启动SpringBoot的web应用，没有web.xml文件。

### [Servlet](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=servlet)

向容器中添加ServletRegistrationBean

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Bean
    public ServletRegistrationBean myServlet() {
        ServletRegistrationBean register = new ServletRegistrationBean(new MyServlet(), "/myServlet");
        register.setLoadOnStartup(1);
        return register;
    }
    ......Copy to clipboardErrorCopied
```

<details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box; font-size: 15px; color: rgb(52, 73, 94); font-family: &quot;Source Sans Pro&quot;, &quot;Helvetica Neue&quot;, Arial, sans-serif; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-style: initial; text-decoration-color: initial;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">MyServlet</summary></details>

### [Filter](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=filter)

向容器中添加FilterRegistrationBean

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {


    @Bean
    public FilterRegistrationBean myFilter() {
        FilterRegistrationBean register = new FilterRegistrationBean(new MyFilter());
        register.setUrlPatterns(Arrays.asList("/myServlet","/"));
        return register;
    }

    ......Copy to clipboardErrorCopied
```

<details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box; font-size: 15px; color: rgb(52, 73, 94); font-family: &quot;Source Sans Pro&quot;, &quot;Helvetica Neue&quot;, Arial, sans-serif; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-style: initial; text-decoration-color: initial;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">MyFilter</summary></details>

### [Listener](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=listener)

向容器中注入ServletListenerRegistrationBean

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

    @Bean
    public ServletListenerRegistrationBean myServletContextListener(){
        return new ServletListenerRegistrationBean(new MyServletContextListener());
    }

    ......
Copy to clipboardErrorCopied
```

<details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box; font-size: 15px; color: rgb(52, 73, 94); font-family: &quot;Source Sans Pro&quot;, &quot;Helvetica Neue&quot;, Arial, sans-serif; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(255, 255, 255); text-decoration-style: initial; text-decoration-color: initial;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">MyListener</summary></details>

## [替换为其他嵌入式web服务器](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=替换为其他嵌入式web服务器)

SpringBoot默认使用的是Tomcat

![1574170320042](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574170320042.png)

如果要换成其他的就把Tomcat的依赖排除掉，然后引入其他嵌入式Servlet容器的以来，如`Jetty`，`Undertow`

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                    <groupId>org.springframework.boot</groupId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <artifactId>spring-boot-starter-jetty</artifactId>
            <groupId>org.springframework.boot</groupId>
        </dependency>Copy to clipboardErrorCopied
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                    <groupId>org.springframework.boot</groupId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <artifactId>spring-boot-starter-undertow</artifactId>
            <groupId>org.springframework.boot</groupId>
        </dependency>Copy to clipboardErrorCopied
```

### [原理](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=原理)

**查看web容器自动配置类**

2.0以下是：~~*EmbeddedServletContainerAutoConfiguration*~~

`ServletWebServerFactoryAutoConfiguration`：嵌入式的web服务器自动配置

```java
@Configuration(
    proxyBeanMethods = false
)
@AutoConfigureOrder(-2147483648)
@ConditionalOnClass({ServletRequest.class})
@ConditionalOnWebApplication(
    type = Type.SERVLET
)
@EnableConfigurationProperties({ServerProperties.class})

//---看这里---
@Import({ServletWebServerFactoryAutoConfiguration.BeanPostProcessorsRegistrar.class, EmbeddedTomcat.class, EmbeddedJetty.class, EmbeddedUndertow.class})
public class ServletWebServerFactoryAutoConfiguration {Copy to clipboardErrorCopied
```

`EmbeddedTomcat.class`：

```java
    @Configuration(
        proxyBeanMethods = false
    )
    //判断当前是否引入了Tomcat依赖；
    @ConditionalOnClass({Servlet.class, Tomcat.class, UpgradeProtocol.class})
    /**
      *判断当前容器没有用户自己定义ServletWebServerFactory：嵌入式的web服务器工厂；
      *作用：创建嵌入式的web服务器
      */
    @ConditionalOnMissingBean(
        value = {ServletWebServerFactory.class},
        search = SearchStrategy.CURRENT
    )
    public static class EmbeddedTomcat {Copy to clipboardErrorCopied
```

`ServletWebServerFactory`：嵌入式的web服务器工厂

```java
@FunctionalInterface
public interface ServletWebServerFactory {
    //获取嵌入式的Servlet容器
    WebServer getWebServer(ServletContextInitializer... initializers);
}Copy to clipboardErrorCopied
```

工厂实现类

![1574172121748](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574172121748.png)

`WebServer`：嵌入式的web服务器实现

![1574172310812](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574172310812.png)

以`TomcatServletWebServerFactory`为例，下面是TomcatServletWebServerFactory类

```java
    public WebServer getWebServer(ServletContextInitializer... initializers) {
        if (this.disableMBeanRegistry) {
            Registry.disableRegistry();
        }

        //创建一个Tomcat
        Tomcat tomcat = new Tomcat();

        //配置Tomcat的基本环境，（tomcat的配置都是从本类获取的，tomcat.setXXX）
        File baseDir = this.baseDirectory != null ? this.baseDirectory : this.createTempDir("tomcat");
        tomcat.setBaseDir(baseDir.getAbsolutePath());
        Connector connector = new Connector(this.protocol);
        connector.setThrowOnFailure(true);
        tomcat.getService().addConnector(connector);
        this.customizeConnector(connector);
        tomcat.setConnector(connector);
        tomcat.getHost().setAutoDeploy(false);
        this.configureEngine(tomcat.getEngine());
        Iterator var5 = this.additionalTomcatConnectors.iterator();

        while(var5.hasNext()) {
            Connector additionalConnector = (Connector)var5.next();
            tomcat.getService().addConnector(additionalConnector);
        }

        this.prepareContext(tomcat.getHost(), initializers);

        //将配置好的Tomcat传入进去，返回一个WebServer；并且启动Tomcat服务器
        return this.getTomcatWebServer(tomcat);
    }Copy to clipboardErrorCopied
```

我们对嵌入式容器的配置修改是怎么生效的？

### [配置修改原理](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=配置修改原理)

`ServletWebServerFactoryAutoConfiguration`在向容器中添加web容器时还添加了一个组件

![1574235580031](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574235580031.png)

`BeanPostProcessorsRegistrar`：后置处理器注册器(也是给容器注入一些组件)

```java
    public static class BeanPostProcessorsRegistrar implements ImportBeanDefinitionRegistrar, BeanFactoryAware {
        private ConfigurableListableBeanFactory beanFactory;

        public BeanPostProcessorsRegistrar() {...}

        public void setBeanFactory(BeanFactory beanFactory) throws BeansException {...}

        public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
            if (this.beanFactory != null) {
                //注册了下面两个组件
                this.registerSyntheticBeanIfMissing(registry, "webServerFactoryCustomizerBeanPostProcessor", WebServerFactoryCustomizerBeanPostProcessor.class);
                this.registerSyntheticBeanIfMissing(registry, "errorPageRegistrarBeanPostProcessor", ErrorPageRegistrarBeanPostProcessor.class);
            }
        }

        private void registerSyntheticBeanIfMissing(BeanDefinitionRegistry registry, String name, Class<?> beanClass) {...}
    }Copy to clipboardErrorCopied
webServerFactoryCustomizerBeanPostProcessor
public class WebServerFactoryCustomizerBeanPostProcessor implements BeanPostProcessor, BeanFactoryAware {

    ......

    //在Bean初始化之前
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        //判断添加的Bean是不是WebServerFactory类型的
        if (bean instanceof WebServerFactory) {
            this.postProcessBeforeInitialization((WebServerFactory)bean);
        }

        return bean;
    }

    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

    private void postProcessBeforeInitialization(WebServerFactory webServerFactory) {
        //获取所有的定制器，调用每一个定制器的customize方法来给Servlet容器进行属性赋值；
        ((Callbacks)LambdaSafe.callbacks(WebServerFactoryCustomizer.class, this.getCustomizers(), webServerFactory, new Object[0]).withLogger(WebServerFactoryCustomizerBeanPostProcessor.class)).invoke((customizer) -> {
            customizer.customize(webServerFactory);
        });
    }Copy to clipboardErrorCopied
```

关于配置文件是如何设置的，参考`EmbeddedWebServerFactoryCustomizerAutoConfiguration`类，最后还是使用上面的方便

总结：

1. SpringBoot根据导入的依赖情况，给容器中添加相应的`XXX`ServletWebServerFactory

2. 容器中某个组件要创建对象就会惊动后置处理器 `webServerFactoryCustomizerBeanPostProcessor`

   只要是嵌入式的是Servlet容器工厂，后置处理器就会工作；

3. 后置处理器，从容器中获取所有的`WebServerFactoryCustomizer`，调用定制器的定制方法给工厂添加配置

## [嵌入式Servlet容器启动原理](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=嵌入式servlet容器启动原理)

1. SpringBoot应用启动运行run方法

   ![1574242390909](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574242390909.png)

2. 153行，创建IOC容器对象，根据当前环境创建

   ![1574242209298](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574242209298.png)

3. 156行，刷新IOC容器

4. 刷新IOC容器中272行，onRefresh()；web的ioc容器重写了onRefresh方法，查看`ServletWebServerApplicationContext`类的onRefresh方法，在方法中调用了this.createWebServer();方法创建web容器

   ```java
       protected void onRefresh() {
           super.onRefresh();
   
           try {
               this.createWebServer();
           } catch (Throwable var2) {
               throw new ApplicationContextException("Unable to start web server", var2);
           }
       }Copy to clipboardErrorCopied
   ```

   98行获取嵌入式的web容器工厂

   ![1574243084120](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574243084120.png)

5. 接下来就是上面的上面的相关配置流程，在创建web容器工厂时会触发`webServerFactoryCustomizerBeanPostProcessor`

6. 然后99行**使用容器工厂获取嵌入式的Servlet容器**

7. 嵌入式的Servlet容器创建对象并启动Servlet容器；

8. 嵌入式的Servlet容器启动后，再将ioc容器中剩下没有创建出的对象获取出来(Controller,Service等)；

## [使用外置的Servlet容器](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=使用外置的servlet容器)

1. 将项目的打包方式改为war

2. 编写一个类继承`SpringBootServletInitializer`，并重写configure方法，调用参数的sources方法springboot启动类传过去然后返回

   ```java
   public class ServletInitializer extends SpringBootServletInitializer {
       @Override
       protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
           return application.sources(HelloSpringBootWebApplication.class);
       }
   }Copy to clipboardErrorCopied
   ```

3. 然后把tomcat的依赖范围改为provided

   ```xml
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-tomcat</artifactId>
               <version>2.2.1.RELEASE</version>
               <scope>provided</scope>
           </dependency>
   
           ......
   
       </dependencies>Copy to clipboardErrorCopied
   ```

1. 最后就可以把项目打包成war放到tomcat中了

2. 在IDEA中可以这样配置

   ![1574247311250](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574247311250.png)

3. 在创建项目时使用Spring Initializr创建选择打包方式为war，1，2，3步骤会自动配置

如果启动tomcat，报了一大堆错误，不妨把Tomcat改为更高的版本试试，如果你项目中的Filter是继承了HttpFilter，请使用tomcat9版本，9以下好像没有HttpFilter

### [原理](https://note.clboy.cn/#/backend/springboot/configservletcontainer?id=原理-1)

*TODO* 2019-11-20

1. Servlet3.0标准ServletContainerInitializer扫描所有jar包中METAINF/services/javax.servlet.ServletContainerInitializer文件指定的类并加载

2. 还可以使用@HandlesTypes，在应用启动的时候加载我们感兴趣的类；

3. 在spring-web-xxx.jar包中的METAINF/services下有javax.servlet.ServletContainerInitializer这个文件

   文件中的类是：

   ```
   org.springframework.web.SpringServletContainerInitializerCopy to clipboardErrorCopied
   ```

   对应的类：

   ```java
   @HandlesTypes({WebApplicationInitializer.class})
   public class SpringServletContainerInitializer implements ServletContainerInitializer {
       public SpringServletContainerInitializer() {
       }
   
       public void onStartup(@Nullable Set<Class<?>> webAppInitializerClasses, ServletContext servletContext) throws ServletException {
   
           ......Copy to clipboardErrorCopied
   ```

4. SpringServletContainerInitializer将@HandlesTypes(WebApplicationInitializer.class)标注的所有这个类型的类都传入到onStartup方法的`Set<Class<?>>`；为这些WebApplicationInitializer类型的类创建实例；

5. 每一个WebApplicationInitializer都调用自己的onStartup方法；

6. WebApplicationInitializer的实现类

   ![1574256076145](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574256076145.png)

7. 相当于我们的SpringBootServletInitializer的类会被创建对象，并执行onStartup方法

8. SpringBootServletInitializer实例执行onStartup的时候会createRootApplicationContext；创建容器

   ```java
   protected WebApplicationContext createRootApplicationContext(
         ServletContext servletContext) {
       //1、创建SpringApplicationBuilder
      SpringApplicationBuilder builder = createSpringApplicationBuilder();
      StandardServletEnvironment environment = new StandardServletEnvironment();
      environment.initPropertySources(servletContext, null);
      builder.environment(environment);
      builder.main(getClass());
      ApplicationContext parent = getExistingRootWebApplicationContext(servletContext);
      if (parent != null) {
         this.logger.info("Root context already created (using as parent).");
         servletContext.setAttribute(
               WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE, null);
         builder.initializers(new ParentContextApplicationContextInitializer(parent));
      }
      builder.initializers(
            new ServletContextApplicationContextInitializer(servletContext));
      builder.contextClass(AnnotationConfigEmbeddedWebApplicationContext.class);
   
       //调用configure方法，子类重写了这个方法，将SpringBoot的主程序类传入了进来
      builder = configure(builder);
   
       //使用builder创建一个Spring应用
      SpringApplication application = builder.build();
      if (application.getSources().isEmpty() && AnnotationUtils
            .findAnnotation(getClass(), Configuration.class) != null) {
         application.getSources().add(getClass());
      }
      Assert.state(!application.getSources().isEmpty(),
            "No SpringApplication sources have been defined. Either override the "
                  + "configure method or add an @Configuration annotation");
      // Ensure error pages are registered
      if (this.registerErrorPageFilter) {
         application.getSources().add(ErrorPageFilterConfiguration.class);
      }
       //启动Spring应用
      return run(application);
   }Copy to clipboardErrorCopied
   ```

9. Spring的应用就启动并且创建IOC容器

   ```java
   public ConfigurableApplicationContext run(String... args) {
      StopWatch stopWatch = new StopWatch();
      stopWatch.start();
      ConfigurableApplicationContext context = null;
      FailureAnalyzers analyzers = null;
      configureHeadlessProperty();
      SpringApplicationRunListeners listeners = getRunListeners(args);
      listeners.starting();
      try {
         ApplicationArguments applicationArguments = new DefaultApplicationArguments(
               args);
         ConfigurableEnvironment environment = prepareEnvironment(listeners,
               applicationArguments);
         Banner printedBanner = printBanner(environment);
         context = createApplicationContext();
         analyzers = new FailureAnalyzers(context);
         prepareContext(context, environment, listeners, applicationArguments,
               printedBanner);
   
          //刷新IOC容器
         refreshContext(context);
         afterRefresh(context, applicationArguments);
         listeners.finished(context, null);
         stopWatch.stop();
         if (this.logStartupInfo) {
            new StartupInfoLogger(this.mainApplicationClass)
                  .logStarted(getApplicationLog(), stopWatch);
         }
         return context;
      }
      catch (Throwable ex) {
         handleRunFailure(context, listeners, analyzers, ex);
         throw new IllegalStateException(ex);
      }
   }
   ```



# [Docker基本使用](https://note.clboy.cn/#/backend/springboot/docker?id=docker基本使用)

**Docker**是一个开源的应用容器引擎；是一个轻量级容器技术；

Docker支持将软件编译成一个镜像；然后在镜像中各种软件做好配置，将镜像发布出去，其他使用者可以直接使用这个镜像；

运行中的这个镜像称为容器，容器启动是非常快速的。

![img](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/20180303145450.png)

![img](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/20180303145531.png)

## [核心概念](https://note.clboy.cn/#/backend/springboot/docker?id=核心概念)

`docker主机(Host)`：安装了Docker程序的机器（Docker直接安装在操作系统之上）；

`docker客户端(Client)`：连接docker主机进行操作；

`docker仓库(Registry)`：用来保存各种打包好的软件镜像；

`docker镜像(Images)`：软件打包好的镜像；放在docker仓库中；

`docker容器(Container)`：镜像启动后的实例称为一个容器；容器是独立运行的一个或一组应用

![img](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/20180303165113.png)

使用Docker的步骤：

1. 确认要安装docker的系统的linux内核高于`3.10`，低于3.10使用`yum update`更新

   ```shell
   uname -rCopy to clipboardErrorCopied
   ```

2. 安装docker

   ```shell
   yum install dockerCopy to clipboardErrorCopied
   ```

3. 查看docker版本

   ```shell
   docker -vCopy to clipboardErrorCopied
   ```

4. 查看docker状态

   ```shell
   service docker statusCopy to clipboardErrorCopied
   ```

5. 启动docker

   ```shell
   service docker startCopy to clipboardErrorCopied
   ```

6. 停止docker

   ```shell
   service docker stopCopy to clipboardErrorCopied
   ```

7. 设置docker开机自启

   ```shell
   systemctl enable dockerCopy to clipboardErrorCopied
   ```

## [docker常用命令](https://note.clboy.cn/#/backend/springboot/docker?id=docker常用命令)

### [镜像操作](https://note.clboy.cn/#/backend/springboot/docker?id=镜像操作)

| 操作 | 命令                                         | 说明                                                    |
| ---- | -------------------------------------------- | ------------------------------------------------------- |
| 检索 | docker search 关键字 eg：docker search redis | 我们经常去docker hub上检索镜像的详细信息，如镜像的TAG。 |
| 拉取 | docker pull 镜像名:tag                       | :tag是可选的，tag表示标签，多为软件的版本，默认是latest |
| 列表 | docker images                                | 查看所有本地镜像                                        |
| 删除 | docker rmi image-id                          | 删除指定的本地镜像                                      |

### [修改镜像源](https://note.clboy.cn/#/backend/springboot/docker?id=修改镜像源)

修改 /etc/docker/daemon.json ，写入如下内容（如果文件不存在请新建该文件）

```
vim /etc/docker/daemon.json

#　内容：

{
"registry-mirrors":["http://hub-mirror.c.163.com"]
}Copy to clipboardErrorCopied
```

| 国内镜像源        | 地址                                                         |
| ----------------- | ------------------------------------------------------------ |
| Docker 官方中国区 | [https://registry.docker-cn.com](https://registry.docker-cn.com/) |
| 网易              | [http://hub-mirror.c.163.com](http://hub-mirror.c.163.com/)  |
| 中国科技大学      | [https://docker.mirrors.ustc.edu.cn](https://docker.mirrors.ustc.edu.cn/) |
| 阿里云            | [https://pee6w651.mirror.aliyuncs.com](https://pee6w651.mirror.aliyuncs.com/) |

## [容器操作](https://note.clboy.cn/#/backend/springboot/docker?id=容器操作)

**以tomcat为例：**

1. 下载tomcat镜像

   ```shell
   docker pull tomcatCopy to clipboardErrorCopied
   ```

   如需选择具体版本，可以在https://hub.docker.com/搜索tomcat

   ```shell
   docker pull tomcat:7.0.96-jdk8-adoptopenjdk-hotspotCopy to clipboardErrorCopied
   ```

2. 根据镜像启动容器，不加TAG默认latest，如果没有下载latest会先去下载再启动

   ```shell
   docker run --name mytomcat -d tomcat:latestCopy to clipboardErrorCopied
   ```

   `--name`：给容器起个名字

   `-d`：后台启动，不加就是前端启动，然后你就只能开一个新的窗口连接，不然就望着黑乎乎的窗口，啥也干不了，`Ctrl+C`即可退出，当然，容器也会关闭

3. 查看运行中的容器

   ```shell
   docker psCopy to clipboardErrorCopied
   ```

4. 停止运行中的容器

   ```shell
   docker stop  容器的id
   
   # 或者
   
   docker stop  容器的名称，就是--name给起的哪个名字Copy to clipboardErrorCopied
   ```

5. 查看所有的容器

   ```shell
   docker ps -aCopy to clipboardErrorCopied
   ```

6. 启动容器

   ```shell
   docker start 容器id/名字Copy to clipboardErrorCopied
   ```

7. 删除一个容器

   ```shell
   docker rm 容器id/名字Copy to clipboardErrorCopied
   ```

8. 启动一个做了端口映射的tomcat

   ```shell
    docker run -d -p 8888:8080 tomcatCopy to clipboardErrorCopied
   ```

   `-d`：后台运行 `-p`: 将主机的端口映射到容器的一个端口 `主机端口(8888)`:`容器内部的端口(8080)`

   外界通过主机的8888端口就可以访问到tomcat，前提是8888端口开放

9. 关闭防火墙

   ```shell
   # 查看防火墙状态
   service firewalld status
   
   # 关闭防火墙
   service firewalld stopCopy to clipboardErrorCopied
   ```

10. 查看容器的日志

    ```shell
    docker logs 容器id/名字Copy to clipboardErrorCopied
    ```

**以mysql为例：**

```shell
# 拉取镜像
docker pull mysql:5.7.28

# 运行mysql容器
 docker run --name mysql -e MYSQL_ROOT_PASSWORD=root -d mysql:5.7.28Copy to clipboardErrorCopied
```

`--name mysql`：容器的名字是mysql；

`MYSQL_ROOT_PASSWORD=root`：root用户的密码是root (必须指定)

连接容器内mysql

在使用 **-d** 参数时，容器启动后会进入后台。此时想要进入容器，可以通过以下指令进入：

- **docker attach**
- **docker exec**：推荐使用 docker exec 命令，因为此退出容器终端，不会导致容器的停止。

```shell
docker exec -it mysql bashCopy to clipboardErrorCopied
```

`-i`: 交互式操作。

`-t`: 终端。

`mysql`: 名为mysql的 镜像。

`bash`：放在镜像名后的是命令，这里我们希望有个交互式 Shell，因此用的是 bash，也可以用`/bin/bash`。

连接上以后就可以正常使用mysql命令操作了

```shell
mysql -uroot -prootCopy to clipboardErrorCopied
```

直接使用端口映射更加方便

```shell
docker run --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql:5.7.28
```



# [SpringBoot与数据库连接](https://note.clboy.cn/#/backend/springboot/jdbc?id=springboot与数据库连接)

## [依赖](https://note.clboy.cn/#/backend/springboot/jdbc?id=依赖)

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-thymeleaf</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.1.1</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>Copy to clipboardErrorCopied
```

## [配置数据库连接信息](https://note.clboy.cn/#/backend/springboot/jdbc?id=配置数据库连接信息)

```yaml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://172.16.145.137:3306/springboot
    driver-class-name: com.mysql.cj.jdbc.DriverCopy to clipboardErrorCopied
```

测试能否连接上数据库

```hava
@SpringBootTest
class SpringbootJdbcApplicationTests {

    @Autowired
    private DataSource dataSource;

    @Test
    void contextLoads() throws SQLException {
        System.out.println(dataSource.getClass());
        System.out.println(dataSource.getConnection());
    }

}Copy to clipboardErrorCopied
```

springboot默认是使用`com.zaxxer.hikari.HikariDataSource`作为数据源，2.0以下是用`org.apache.tomcat.jdbc.pool.DataSource`作为数据源；

数据源的相关配置都在DataSourceProperties里面；

## [自动配置原理](https://note.clboy.cn/#/backend/springboot/jdbc?id=自动配置原理)

*TODO*

jdbc的相关配置都在`org.springframework.boot.autoconfigure.jdbc`包下

参考`DataSourceConfiguration`，根据配置创建数据源，默认使用Hikari连接池；可以使用spring.datasource.type指定自定义的数据源类型；

springboot默认支持的连池：

- org.apache.commons.dbcp2.BasicDataSource
- com.zaxxer.hikari.HikariDataSource
- org.apache.tomcat.jdbc.pool.DataSource

自定义数据源类型：

```java
    @Configuration(
        proxyBeanMethods = false
    )
    @ConditionalOnMissingBean({DataSource.class})
    @ConditionalOnProperty(
        name = {"spring.datasource.type"}
    )
    static class Generic {
        Generic() {
        }

        @Bean
        DataSource dataSource(DataSourceProperties properties) {
             //使用DataSourceBuilder创建数据源，利用反射创建响应type的数据源，并且绑定相关属性
            return properties.initializeDataSourceBuilder().build();
        }
    }Copy to clipboardErrorCopied
```

## [启动应用执行sql](https://note.clboy.cn/#/backend/springboot/jdbc?id=启动应用执行sql)

SpringBoot在创建连接池后还会运行预定义的SQL脚本文件，具体参考`org.springframework.boot.autoconfigure.jdbc.DataSourceInitializationConfiguration`配置类，

在该类中注册了`dataSourceInitializerPostProcessor`

下面是获取schema脚本文件的方法

```java
List<Resource> scripts = this.getScripts("spring.datasource.schema", this.properties.getSchema(), "schema");Copy to clipboardErrorCopied
```

![1574409852703](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574409852703.png)

可以看出，如果我们没有在配置文件中配置脚本的具体位置，就会在classpath下找`schema-all.sql`和`schema.sql` platform获取的是all，platform可以在配置文件中修改

具体查看`createSchema()方法`和`initSchema()方法`

initSchema()方法获取的是`data-all.sql`，`data.sql`

我们也可以在配置文件中配置sql文件的位置

```yaml
spring:
  datasource:
    schema:
      - classpath:department.sql
      - 指定位置Copy to clipboardErrorCopied
```

**测试：**

在类路径下创建`schema.sql`，运行程序查看数据库是否存在该表

```sql
DROP TABLE IF EXISTS `department`;
CREATE TABLE `department` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `departmentName` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;Copy to clipboardErrorCopied
```

程序启动后发现表并没有被创建，DEBUG查看以下，发现在运行之前会有一个判断

![1574411869052](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574411869052.png)

![1574412098885](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574412098885.png)

上面方法也不知道在干什么，反正就是只要是`NEVER`和`EMBEDDED`就为true，而DataSourceInitializationMode枚举类中除了这两个就剩下`ALWAYS`了，可以在配置文件中配置为ALWAYS

![1574412237660](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574412237660.png)

```yaml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://172.16.145.137:3306/springboot
    driver-class-name: com.mysql.cj.jdbc.Driver
    initialization-mode: alwaysCopy to clipboardErrorCopied
```

`schema.sql`：建表语句

`data.sql`：插入数据

当然混合使用也可以，愿意咋来咋来

**注意：**项目每次启动都会执行一次sql

## [整合Druid数据源](https://note.clboy.cn/#/backend/springboot/jdbc?id=整合druid数据源)

> 选择哪个数据库连接池
>
> - DBCP2 是 Appache 基金会下的项目，是最早出现的数据库连接池 DBCP 的第二个版本。
> - C3P0 最早出现时是作为 Hibernate 框架的默认数据库连接池而进入市场。
> - Druid 是阿里巴巴公司开源的一款数据库连接池，其特点在于有丰富的附加功能。
> - HikariCP 相较而言比较新，它最近两年才出现，据称是速度最快的数据库连接池。最近更是被 Spring 设置为默认数据库连接池。
>
> 不选择 C3P0 的原因：
>
> - C3P0 的 Connection 是异步释放。这个特性会导致释放的在某些情况下 Connection 实际上 **still in use** ，并未真正释放掉，从而导致连接池中的 Connection 耗完，等待状况。
> - Hibernate 现在对所有数据库连接池一视同仁，官方不再指定『默认』数据库连接池。因此 C3P0 就失去了『官方』光环。
>
> 不选择 DBCP2 的原因：
>
> - 相较于 Druid 和 HikariCP，DBCP2 没有什么特色功能/卖点。基本上属于 `能用，没毛病` 的情况，地位显得略有尴尬。

1. 在 Spring Boot 项目中加入`druid-spring-boot-starter`依赖 ([点击查询最新版本](https://mvnrepository.com/artifact/com.alibaba/druid-spring-boot-starter))

   ```xml
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid-spring-boot-starter</artifactId>
       <version>1.1.20</version>
   </dependency>Copy to clipboardErrorCopied
   ```

2. 在配置文件中指定数据源类型

   ```yaml
   spring:
     datasource:
       username: root
       password: root
       url: jdbc:mysql://172.16.145.137:3306/springboot
       driver-class-name: com.mysql.cj.jdbc.Driver
       initialization-mode: always
       type: com.alibaba.druid.pool.DruidDataSourceCopy to clipboardErrorCopied
   ```

3. 测试类查看使用的数据源

   ```java
   @SpringBootTest
   class SpringbootJdbcApplicationTests {
   
       @Autowired
       private DataSource dataSource;
   
       @Test
       void contextLoads() throws SQLException {
           System.out.println(dataSource.getClass());
           System.out.println(dataSource.getConnection());
       }
   
   }Copy to clipboardErrorCopied
   ```

### [配置参数](https://note.clboy.cn/#/backend/springboot/jdbc?id=配置参数)

```yaml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://172.16.145.137:3306/springboot
    driver-class-name: com.mysql.cj.jdbc.Driver
    initialization-mode: always
    type: com.alibaba.druid.pool.DruidDataSource
    druid:
      # 连接池配置
      # 配置初始化大小、最小、最大
      initial-size: 1
      min-idle: 1
      max-active: 20
      # 配置获取连接等待超时的时间
      max-wait: 3000
      validation-query: SELECT 1 FROM DUAL
      test-on-borrow: false
      test-on-return: false
      test-while-idle: true
      pool-prepared-statements: true
      time-between-eviction-runs-millis: 60000
      min-evictable-idle-time-millis: 300000
      filters: stat,wall,slf4j
      # 配置web监控,默认配置也和下面相同(除用户名密码，enabled默认false外)，其他可以不配
      web-stat-filter:
        enabled: true
        url-pattern: /*
        exclusions: "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*"
      stat-view-servlet:
        enabled: true
        url-pattern: /druid/*
        login-username: admin
        login-password: root
        allow: 127.0.0.1Copy to clipboardErrorCopied
```

后台页面，访问http://localhost:8080/druid/login.html



# [SpringBoot整合Mybatis](https://note.clboy.cn/#/backend/springboot/mybatis?id=springboot整合mybatis)

## [引入依赖](https://note.clboy.cn/#/backend/springboot/mybatis?id=引入依赖)

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.1</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.20</version>
        </dependency>
    </dependencies>Copy to clipboardErrorCopied
```

依赖关系

![1574423628318](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574423628318.png)

## [项目构建](https://note.clboy.cn/#/backend/springboot/mybatis?id=项目构建)

1. 在resources下创建`department.sql`和`employee.sql`，项目启动时创建表

   ```sql
   DROP TABLE IF EXISTS `department`;
   CREATE TABLE `department` (
     `id` int(11) NOT NULL AUTO_INCREMENT,
     `departmentName` varchar(255) DEFAULT NULL,
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;Copy to clipboardErrorCopied
   ```

   ```sql
   DROP TABLE IF EXISTS `employee`;
   CREATE TABLE `employee` (
     `id` int(11) NOT NULL AUTO_INCREMENT,
     `lastName` varchar(255) DEFAULT NULL,
     `email` varchar(255) DEFAULT NULL,
     `gender` int(2) DEFAULT NULL,
     `d_id` int(11) DEFAULT NULL,
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
   Copy to clipboardErrorCopied
   ```

2. 实体类

   <details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">Department</summary></details>

   <details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">Employee</summary></details>

1. 配置文件

   ```yaml
   spring:
     datasource:
       username: root
       password: root
       url: jdbc:mysql://172.16.145.137:3306/springboot
       driver-class-name: com.mysql.cj.jdbc.Driver
       initialization-mode: always
       type: com.alibaba.druid.pool.DruidDataSource
       druid:
         # 连接池配置
         # 配置初始化大小、最小、最大
         initial-size: 1
         min-idle: 1
         max-active: 20
         # 配置获取连接等待超时的时间
         max-wait: 3000
         validation-query: SELECT 1 FROM DUAL
         test-on-borrow: false
         test-on-return: false
         test-while-idle: true
         pool-prepared-statements: true
         time-between-eviction-runs-millis: 60000
         min-evictable-idle-time-millis: 300000
         filters: stat,wall,slf4j
         # 配置web监控,默认配置也和下面相同(除用户名密码，enabled默认false外)，其他可以不配
         web-stat-filter:
           enabled: true
           url-pattern: /*
           exclusions: "*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*"
         stat-view-servlet:
           enabled: true
           url-pattern: /druid/*
           login-username: admin
           login-password: root
           allow: 127.0.0.1
       schema:
         - classpath:department.sql
         - classpath:employee.sqlCopy to clipboardErrorCopied
   ```

## [Mybatis增删改查](https://note.clboy.cn/#/backend/springboot/mybatis?id=mybatis增删改查)

1. 创建mapper接口

   ```java
   @Mapper
   public interface DepartmentMapper {
   
       @Select("select * from department")
       public List<Department> selectAll();
   
       @Select("select * from department where id=#{id}")
       public Department selectById(Integer id);
   
       @Options(useGeneratedKeys = true, keyProperty = "id")
       @Insert("insert into department(departmentName) values(#{departmentName})")
       public int save(Department department);
   
       @Update("update department set departmentName=#{departmentName}")
       public int update(Department department);
   
       @Delete("delete from department where id =#{id}")
       public int delete(Integer id);
   }Copy to clipboardErrorCopied
   ```

2. 创建Controller

   ```java
   @RestController
   public class DepartmentController {
   
       @Autowired
       private DepartmentMapper departmentMapper;
   
       @RequestMapping("/dep/{id}")
       public List<Department> getDepById(@PathVariable Integer id) {
           return departmentMapper.selectAll();
       }
   
       @RequestMapping("/dep")
       public Department getDepById(Department department) {
           departmentMapper.save(department);
           return department;
       }
   }Copy to clipboardErrorCopied
   ```

1. 访问：http://localhost:8080/dep?departmentName=PeppaPig 添加一条数据

   访问：http://localhost:8080/dep/1获取数据

## [Mybatis配置](https://note.clboy.cn/#/backend/springboot/mybatis?id=mybatis配置)

### [开启驼峰命名法](https://note.clboy.cn/#/backend/springboot/mybatis?id=开启驼峰命名法)

我们的实体类和表中的列名一致，一点问题也没有

我们把department表的departmentName列名改为department_name看看会发生什么

访问：http://localhost:8080/dep/1获取数据

```
[{"id":1,"departmentName":null}]Copy to clipboardErrorCopied
```

由于列表和属性名不一致，所以就没有封装进去，我们表中的列名和实体类属性名都是遵循驼峰命名规则的，可以开启mybatis的开启驼峰命名配置

```yaml
mybatis:
  configuration:
    map-underscore-to-camel-case: trueCopy to clipboardErrorCopied
```

然后重启项目，重新插入数据，再查询就发现可以封装进去了

也可以通过向spring容器中注入`org.mybatis.spring.boot.autoconfigure.ConfigurationCustomizer`的方法设置mybatis参数

```JAVA
@Configuration
public class MybatisConfig {

    @Bean
    public ConfigurationCustomizer mybatisConfigurationCustomizer() {
        return new ConfigurationCustomizer() {
            @Override
            public void customize(org.apache.ibatis.session.Configuration configuration) {
                configuration.setMapUnderscoreToCamelCase(true);
            }
        };
    }
}Copy to clipboardErrorCopied
```

![1574430056512](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574430056512.png)

![1574430280791](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574430280791.png)

## [Mapper扫描](https://note.clboy.cn/#/backend/springboot/mybatis?id=mapper扫描)

使用`@mapper注解`的类可以被扫描到容器中，但是每个Mapper都要加上这个注解就是一个繁琐的工作，能不能直接扫描某个包下的所有Mapper接口呢，当然可以，在springboot启动类上加上`@MapperScan`

```java
@MapperScan("cn.clboy.springbootmybatis.mapper")
@SpringBootApplication
public class SpringbootMybatisApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootMybatisApplication.class, args);
    }

}Copy to clipboardErrorCopied
```

## [使用xml配置文件](https://note.clboy.cn/#/backend/springboot/mybatis?id=使用xml配置文件)

1. 创建mybatis全局配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <typeAliases>
           <package name="cn.clboy.springbootmybatis.model"/>
       </typeAliases>
   </configuration>Copy to clipboardErrorCopied
   ```

2. 创建EmployeeMapper接口

   ```java
   public interface EmployeeMapper {
   
       List<Employee> selectAll();
   
       int save(Employee employee);
   }Copy to clipboardErrorCopied
   ```

3. 创建EmployeeMapper.xml映射文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="cn.clboy.springbootmybatis.mapper.EmployeeMapper">
       <select id="selectAll" resultType="employee">
           SELECT * FROM employee
       </select>
       <insert id="save" parameterType="employee" useGeneratedKeys="true" keyProperty="id">
          INSERT INTO employee(lastName,email,gender,d_id) VALUES (#{lastName},#{email},#{gender},#{d_id})
       </insert>
   </mapper>Copy to clipboardErrorCopied
   ```

4. 配置文件(application.yaml)中指定配置文件和映射文件的位置

   ```yaml
   mybatis:
     config-location: classpath:mybatis/mybatis-config.xml
     mapper-locations: classpath:mybatis/mapper/*.xmlCopy to clipboardErrorCopied
   ```

5. 给表中插入两个数据，用于测试

   ```sql
    INSERT INTO employee(lastName,email,gender,d_id) VALUES ('张三','123456@qq.com',1,1);
    INSERT INTO employee(lastName,email,gender,d_id) VALUES ('lisi','245612@qq.com',1,1);Copy to clipboardErrorCopied
   ```

6. 创建EmployeeController

   ```java
   @RestController
   public class EmployeeController {
   
       @Autowired
       private EmployeeMapper employeeMapper;
   
       @RequestMapping("/emp/list")
       public List<Employee> getALl() {
           return employeeMapper.selectAll();
       }
   
       @RequestMapping("/emp/{id}")
       public Employee save(Employee employee) {
           employeeMapper.save(employee);
           return employee;
       }
   }Copy to clipboardErrorCopied
   ```

   访问：http://localhost:8080/emp/list     

   

# [SpringBoot整合JPA](https://note.clboy.cn/#/backend/springboot/jpa?id=springboot整合jpa)

**SpringData：**

![SpringData](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/20180306105412.png)

## [依赖](https://note.clboy.cn/#/backend/springboot/jpa?id=依赖)

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>Copy to clipboardErrorCopied
```

![1574481366615](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574481366615.png)

## [实体类](https://note.clboy.cn/#/backend/springboot/jpa?id=实体类)

```java
package cn.clboy.springbootjpa.entity;


import javax.persistence.*;


@Entity //使用JPA注解配置映射关系,告诉JPA这是一个实体类（和数据表映射的类）
@Table(name = "tbl_user")   //@Table来指定和哪个数据表对应;如果省略默认表名类名小写；
public class User {

    @Id //这是一个主键
    @GeneratedValue(strategy = GenerationType.IDENTITY) //自增主键
    private Integer id;

    @Column(name = "last_name",length = 50) //这是和数据表对应的一个列
    private String lastName;
    @Column //省略默认列名就是属性名
    private String email;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
Copy to clipboardErrorCopied
```

## [DAO](https://note.clboy.cn/#/backend/springboot/jpa?id=dao)

```java
package cn.clboy.springbootjpa.repository;

import cn.clboy.springbootjpa.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;

/**
 * 继承JpaRepository来完成对数据库的操作
 * 泛型是（实体类，主键）
 */
public interface UserRepository extends JpaRepository<User, Integer> {

}Copy to clipboardErrorCopied
```

## [配置文件](https://note.clboy.cn/#/backend/springboot/jpa?id=配置文件)

```yaml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://172.16.145.137:3306/springboot
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: trueCopy to clipboardErrorCopied
```

## [Controller](https://note.clboy.cn/#/backend/springboot/jpa?id=controller)

```java
Copy to clipboardErrorCopied
```

添加User，访问：

http://localhost:8080/user?lastName=zhangsan&email=123456@qq.com

http://localhost:8080/user?lastName=lisi&email=78215646@qq.com

查询用户访问：

http://localhost:8080/user/1，然后你就会发现抛出500错误，原因是getOne方法使用的懒加载，获取到的只是代理对象，转换为json时会报错

**解决方法有两种：**

1. 关闭懒加载，在实体类上加`@Proxy(lazy = false)`注解

   ```java
   @Entity
   @Table(name = "tbl_user")
   @Proxy(lazy = false)
   public class UserCopy to clipboardErrorCopied
   ```

2. 转json的时候忽略`hibernateLazyInitializer`和`handler`属性

   ```java
   @Entity
   @Table(name = "tbl_user")
   @JsonIgnoreProperties(value = {"hibernateLazyInitializer", "handler"})
   public class User 
   ```



# [SpringBoot启动流程](https://note.clboy.cn/#/backend/springboot/startprocess?id=springboot启动流程)

![1574497298469](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574497298469.png)

## [启动原理](https://note.clboy.cn/#/backend/springboot/startprocess?id=启动原理)

```java
    public static void main(String[] args) {
        //xxx.class：主配置类，（可以传多个）
        SpringApplication.run(xxx.class, args);
    }Copy to clipboardErrorCopied
```

1. 从run方法开始，创建SpringApplication，然后再调用run方法

   ```
   /**
    * ConfigurableApplicationContext(可配置的应用程序上下文)
    */
   public static ConfigurableApplicationContext run(Class<?> primarySource, String... args) {
       //调用下面的run方法
       return run(new Class[]{primarySource}, args);
   }
   
   public static ConfigurableApplicationContext run(Class<?>[] primarySources, String[] args) {
       return (new SpringApplication(primarySources)).run(args);
   }Copy to clipboardErrorCopied
   ```

2. 创建SpringApplication

   ```java
   //primarySources：主配置类
   new SpringApplication(primarySources)Copy to clipboardErrorCopied
   ```

   ```java
       public SpringApplication(Class<?>... primarySources) {
           //调用下面构造方法
           this((ResourceLoader) null, primarySources);
       }
   
       public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
           this.sources = new LinkedHashSet();
           this.bannerMode = Mode.CONSOLE;
           this.logStartupInfo = true;
           this.addCommandLineProperties = true;
           this.addConversionService = true;
           this.headless = true;
           this.registerShutdownHook = true;
           this.additionalProfiles = new HashSet();
           this.isCustomEnvironment = false;
           this.lazyInitialization = false;
           this.resourceLoader = resourceLoader;
           Assert.notNull(primarySources, "PrimarySources must not be null");
           //保存主配置类
           this.primarySources = new LinkedHashSet(Arrays.asList(primarySources));
           //获取当前应用的类型，是不是web应用，见2.1
           this.webApplicationType = WebApplicationType.deduceFromClasspath();
           //从类路径下找到META‐INF/spring.factories配置的所有ApplicationContextInitializer；然后保存起来，见2.2
           this.setInitializers(this.getSpringFactoriesInstances(ApplicationContextInitializer.class));
           //从类路径下找到META‐INF/spring.ApplicationListener；然后保存起来,原理同上
           this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
           //从多个配置类中找到有main方法的主配置类，见下图(在调run方法的时候是可以传递多个配置类的)
           this.mainApplicationClass = this.deduceMainApplicationClass();
           //执行完毕，SpringApplication对象就创建出来了，返回到1处，调用SpringApplication对象的run方法，到3
       }Copy to clipboardErrorCopied
   ```

   ![1574503716694](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574503716694.png)

   <details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">2.1 判断是不是web 应用</summary></details>

   <details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">2.2 getSpringFactoriesInstances(ApplicationContextInitializer.class)</summary></details>

1. 调用SpringApplication对象的run方法

   ![1574512218795](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574512218795.png)

   ```java
       public ConfigurableApplicationContext run(String... args) {
           StopWatch stopWatch = new StopWatch();
           stopWatch.start();
           //声明IOC容器
           ConfigurableApplicationContext context = null;
           Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList();
           this.configureHeadlessProperty();
           //从类路径下META‐INF/spring.factories获取SpringApplicationRunListeners，原理同2中获取ApplicationContextInitializer和ApplicationListener
           SpringApplicationRunListeners listeners = this.getRunListeners(args);
           //遍历上一步获取的所有SpringApplicationRunListener，调用其starting方法
           listeners.starting();
   
           Collection exceptionReporters;
           try {
               //封装命令行
               ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
               //准备环境，把上面获取到的listeners传过去，见3.1
               ConfigurableEnvironment environment = this.prepareEnvironment(listeners, applicationArguments);
               this.configureIgnoreBeanInfo(environment);
               //打印Banner，就是控制台那个Spring字符画
               Banner printedBanner = this.printBanner(environment);
               //根据当前环境利用反射创建IOC容器
               context = this.createApplicationContext();
           //从类路径下META‐INF/spring.factories获取SpringBootExceptionReporter，原理同2中获取ApplicationContextInitializer和ApplicationListener
               exceptionReporters = this.getSpringFactoriesInstances(SpringBootExceptionReporter.class, new Class[]{ConfigurableApplicationContext.class}, context);
               //准备IOC容器，见3.3
               this.prepareContext(context, environment, listeners, applicationArguments, printedBanner);
               //刷新IOC容器，可查看配置嵌入式Servlet容器原理 链接在3.4
               this.refreshContext(context);
               //这是一个空方法
               this.afterRefresh(context, applicationArguments);
               stopWatch.stop();
               if (this.logStartupInfo) {
                   (new StartupInfoLogger(this.mainApplicationClass)).logStarted(this.getApplicationLog(), stopWatch);
               }
               //调用所有SpringApplicationRunListener的started方法
               listeners.started(context);
               //见3.5 ，从ioc容器中获取所有的ApplicationRunner和CommandLineRunner进行回调ApplicationRunner先回调，CommandLineRunner再
               this.callRunners(context, applicationArguments);
           } catch (Throwable var10) {
               this.handleRunFailure(context, var10, exceptionReporters, listeners);
               throw new IllegalStateException(var10);
           }
   
           try {
               //调用所有SpringApplicationRunListener的running方法
               listeners.running(context);
               return context;
           } catch (Throwable var9) {
               this.handleRunFailure(context, var9, exceptionReporters, (SpringApplicationRunListeners)null);
               throw new IllegalStateException(var9);
           }
       }Copy to clipboardErrorCopied
   ```

   //容器创建完成，返回步骤1处，最后返回到启动类

   <details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">3.1</summary></details>

   <details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">3.2</summary></details>

   <details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">3.3</summary></details>

   <details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">3.4</summary></details>

   <details style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;"><summary style="-webkit-font-smoothing: antialiased; -webkit-tap-highlight-color: rgba(0, 0, 0, 0); text-size-adjust: none; box-sizing: border-box;">3.5</summary></details>

## [几个重要的事件回调机制](https://note.clboy.cn/#/backend/springboot/startprocess?id=几个重要的事件回调机制)

**配置在META-INF/spring.factories**

- ApplicationContextInitializer
- SpringApplicationRunListener

**只需要放在ioc容器中**

- ApplicationRunner
- CommandLineRunner

## [测试](https://note.clboy.cn/#/backend/springboot/startprocess?id=测试)

1. 创建`ApplicationContextInitializer`和`SpringApplicationRunListener`的实现类，并在META-INF/spring.factories文件中配置

   ```java
   public class TestApplicationContextInitializer implements ApplicationContextInitializer {
   
       @Override
       public void initialize(ConfigurableApplicationContext configurableApplicationContext) {
           System.out.println("TestApplicationContextInitializer.initialize");
       }
   }Copy to clipboardErrorCopied
   ```

   ```java
   public class TestSpringApplicationRunListener implements SpringApplicationRunListener {
       @Override
       public void starting() {
           System.out.println("TestSpringApplicationRunListener.starting");
       }
   
       @Override
       public void environmentPrepared(ConfigurableEnvironment environment) {
           System.out.println("TestSpringApplicationRunListener.environmentPrepared");
       }
   
       @Override
       public void contextPrepared(ConfigurableApplicationContext context) {
           System.out.println("TestSpringApplicationRunListener.contextPrepared");
       }
   
       @Override
       public void contextLoaded(ConfigurableApplicationContext context) {
           System.out.println("TestSpringApplicationRunListener.contextLoaded");
       }
   
       @Override
       public void started(ConfigurableApplicationContext context) {
           System.out.println("TestSpringApplicationRunListener.started");
       }
   
       @Override
       public void running(ConfigurableApplicationContext context) {
           System.out.println("TestSpringApplicationRunListener.running");
       }
   
       @Override
       public void failed(ConfigurableApplicationContext context, Throwable exception) {
           System.out.println("TestSpringApplicationRunListener.failed");
       }
   }Copy to clipboardErrorCopied
   ```

   ```properties
   org.springframework.context.ApplicationContextInitializer=\
   cn.clboy.springbootprocess.init.TestApplicationContextInitializer
   
   org.springframework.boot.SpringApplicationRunListener=\
   cn.clboy.springbootprocess.init.TestSpringApplicationRunListenerCopy to clipboardErrorCopied
   ```

   启动报错：说是没有找到带org.springframework.boot.SpringApplication和String数组类型参数的构造器，给TestSpringApplicationRunListener添加这样的构造器

   ![1574515721669](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574515721669.png)

   ```java
       public TestSpringApplicationRunListener(SpringApplication application,String[] args) {
       }Copy to clipboardErrorCopied
   ```

2. 创建`ApplicationRunner`实现类和`CommandLineRunner`实现类，注入到容器中

   ```java
   @Component
   public class TestApplicationRunner implements ApplicationRunner {
   
       @Override
       public void run(ApplicationArguments args) throws Exception {
           System.out.println("TestApplicationRunner.run\t--->"+args);
       }
   }Copy to clipboardErrorCopied
   ```

   ```java
   @Component
   public class TestCommandLineRunn implements CommandLineRunner {
   
       @Override
       public void run(String... args) throws Exception {
           System.out.println("TestCommandLineRunn.runt\t--->"+ Arrays.toString(args));
       }
   }Copy to clipboardErrorCopied
   ```

   ![1574517578711](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574517578711.png)

## [修改Banner](https://note.clboy.cn/#/backend/springboot/startprocess?id=修改banner)

默认是找类路径下的`banner.txt`，可以在配置文件中修改

```properties
spring.banner.location=xxx.txtCopy to clipboardErrorCopied
```

生成banner的网站：http://patorjk.com/software/taag

![1574508283758](https://cdn.static.note.zzrfdsn.cn/images/springboot/assets/1574508283758.png)

也可以使用图片(将其像素解析转换成assii编码之后打印)，默认是在类路径下找名为`banner`后缀为`"gif", "jpg", "png"`的图片

```
static final String[] IMAGE_EXTENSION = new String[]{"gif", "jpg", "png"};Copy to clipboardErrorCopied
```

也可以在配置文件中指定

```properties
spring.banner.image.location=classpath:abc.png
```



